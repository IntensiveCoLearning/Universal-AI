---
timezone: UTC+8
---

# 王谦(Joy Wang)

**GitHub ID:** JoyWQ

**Telegram:** @JoyWQQ

## Self-introduction

目前在一家传统物联网公司做数据预警平台的后端研发工作 主要涉及流式数据处理和复杂规则预警 工作中会用到一些数据挖掘和自然语言学习的小模型 对于Web3、AI、复杂DeFi策略交汇地带抱有浓厚的学习兴趣 希望能通过此次活动更大幅度地提升自己 并结识到优秀、有意思的新朋友 也希望在未来能从事更cool的工作或者事业

## Notes

<!-- Content_START -->
# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->
-   Qwen-AI入门demo的代码编写
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->

-   Qwen-AI入门demo的代码编写
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->


-   Qwen-AI的入门demo设计
    
-   串联Zetachain、Qwen-AI的整体流程设计
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->



-   成功跑通Sepolia到ZetaChain跨链消息转发的小demo，里面有一些坑：
    
    -   一定要注意防御性的条件判断，如requires(...)/if (...) {revert()}这样的内容，测试阶段数据很容易被过滤
        
    -   一定要注意Sepolia的消息数据应该发往Sepolia的Gateway，ZetaChain testnet的Gateway放在Sepolia上只是一个毫无意义的普通地址，每条链都一定是先发送数据给自己链的观察者
        
    -   注意，交易结果有可能在 [ZetaChain testnet exployer](https://testnet.zetascan.com/) 上展示不出来，但用命令行可以正确查询
        
-   以下是代码（为了调试方便，主合约ZetaEasyMQ我从Messaging接口又换回到了UniversalContract接口）
    

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import {UniversalContract, MessageContext} from "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
import {RevertContext} from "@zetachain/protocol-contracts/contracts/Revert.sol";

/// @title ZetaEasyMQ - 简化版跨链消息接收合约
/// @notice 直接继承UniversalContract，无复杂权限检查，用于学习和调试
/// @dev 部署在ZetaChain上，接收来自其他链的跨链消息
contract ZetaEasyMQ is UniversalContract {
    error ZetaMQ__InvalidMessageID();

    struct Message {
        uint256 id;
        address senderEVM;      // EVM链上的发送者地址
        bytes senderRaw;        // 原始发送者数据（支持非EVM链）
        uint256 sourceChainId;  // 源链ID
        string content;         // 消息内容
        uint256 timestamp;      // 接收时间戳
    }

    uint256 public messageCount;
    mapping(uint256 => Message) public messages;

    // 详细的调试事件
    event MessageReceived(
        uint256 indexed id,
        address indexed senderEVM,
        uint256 indexed sourceChainId,
        string content,
        uint256 timestamp
    );

    // 原始调用事件，用于调试
    event RawCallReceived(
        address senderEVM,
        bytes senderRaw,
        uint256 chainId,
        address zrc20,
        uint256 amount,
        bytes message
    );

    // 解码失败事件
    event DecodeError(
        address senderEVM,
        uint256 chainId,
        bytes rawMessage,
        string errorReason
    );

    // 回滚事件
    event RevertReceived(
        address sender,
        address asset,
        uint256 amount,
        bytes data
    );

    /// @notice 构造函数 - 自动从Registry获取Gateway地址
    /// @dev UniversalContract的构造函数会自动调用registry.gatewayZEVM()
    constructor() {}

    /// @notice 处理跨链调用的核心函数
    /// @dev 当Gateway收到来自其他链的消息时，会调用此函数
    /// @param context 消息上下文，包含发送者信息和源链ID
    /// @param zrc20 ZRC20代币地址（如果有代币转移）
    /// @param amount 代币数量
    /// @param message ABI编码的消息内容
    function onCall(
        MessageContext calldata context,
        address zrc20,
        uint256 amount,
        bytes calldata message
    ) external override onlyGateway {
        // 首先记录原始调用数据，便于调试
        emit RawCallReceived(
            context.senderEVM,
            context.sender,
            context.chainID,
            zrc20,
            amount,
            message
        );

        // 尝试解码消息内容
        string memory content;
        if (message.length > 0) {
            // 尝试解码为string
            try this.decodeString(message) returns (string memory decoded) {
                content = decoded;
            } catch {
                // 解码失败，记录错误但仍然存储消息
                content = "[decode_failed]";
                emit DecodeError(
                    context.senderEVM,
                    context.chainID,
                    message,
                    "Failed to decode as string"
                );
            }
        } else {
            content = "[empty_message]";
        }

        // 存储消息
        messageCount++;
        messages[messageCount] = Message({
            id: messageCount,
            senderEVM: context.senderEVM,
            senderRaw: context.sender,
            sourceChainId: context.chainID,
            content: content,
            timestamp: block.timestamp
        });

        // 发出消息接收事件
        emit MessageReceived(
            messageCount,
            context.senderEVM,
            context.chainID,
            content,
            block.timestamp
        );
    }

    /// @notice 处理跨链调用回滚
    /// @dev 当跨链调用失败时，Gateway会调用此函数
    function onRevert(RevertContext calldata context) external onlyGateway {
        emit RevertReceived(
            context.sender,
            context.asset,
            context.amount,
            context.revertMessage
        );
    }

    /// @notice 外部函数用于解码字符串（供try-catch使用）
    /// @param data ABI编码的字符串
    /// @return 解码后的字符串
    function decodeString(bytes calldata data) external pure returns (string memory) {
        return abi.decode(data, (string));
    }

    /// @notice 获取指定ID的消息
    /// @param id 消息ID
    /// @return 消息结构体
    function getMessage(uint256 id) external view returns (Message memory) {
        if (id == 0 || id > messageCount) revert ZetaMQ__InvalidMessageID();
        return messages[id];
    }

    /// @notice 获取消息总数
    /// @return 消息总数
    function getTotalMessages() external view returns (uint256) {
        return messageCount;
    }

    /// @notice 获取最新的消息
    /// @return 最新的消息结构体
    function getLatestMessage() external view returns (Message memory) {
        if (messageCount == 0) revert ZetaMQ__InvalidMessageID();
        return messages[messageCount];
    }

    /// @notice 获取Gateway地址（用于调试）
    /// @return Gateway合约地址
    function getGateway() external view returns (address) {
        return address(gateway);
    }
}

// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.26;

import "forge-std/Script.sol";
import "../src/ZetaEasyMQ.sol";

contract DeployZetaEasyMQ is Script {
    function run() external {
        uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");
        address deployerAddress = vm.addr(deployerPrivateKey);

        console.log("==========================================");
        console.log("Deploying ZetaEasyMQ...");
        console.log("Deployer:", deployerAddress);
        console.log("==========================================");

        vm.startBroadcast(deployerPrivateKey);

        // 新合约无需参数，自动从Registry获取Gateway
        ZetaEasyMQ mq = new ZetaEasyMQ();

        console.log("");
        console.log("==========================================");
        console.log("ZetaEasyMQ deployed successfully!");
        console.log("Contract address:", address(mq));
        console.log("Gateway address:", mq.getGateway());
        console.log("==========================================");

        vm.stopBroadcast();
    }
}

// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.26;

import "forge-std/Test.sol";
import "../src/ZetaEasyMQ.sol";
import {MessageContext} from "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";

contract ZetaEasyMQTest is Test {
    ZetaEasyMQ public mq;
    address public mockGateway;
    address public constant REGISTRY = 0x7CCE3Eb018bf23e1FE2a32692f2C77592D110394;

    function setUp() public {
        // Create mock gateway
        mockGateway = address(0x1234567890123456789012345678901234567890);
        
        // Mock the registry call to return our mock gateway
        vm.mockCall(
            REGISTRY,
            abi.encodeWithSignature("gatewayZEVM()"),
            abi.encode(mockGateway)
        );

        // Deploy contract - 无需参数，自动从Registry获取Gateway
        mq = new ZetaEasyMQ();
    }

    function test_initial_state() public view {
        // 测试初始状态
        assertEq(mq.messageCount(), 0);
        assertEq(mq.getTotalMessages(), 0);
    }

    function test_gateway_address() public view {
        // 测试Gateway地址是否正确设置
        assertEq(mq.getGateway(), mockGateway);
    }

    function test_decode_string() public view {
        // 测试消息解码功能
        string memory testMessage = "Hello ZetaChain!";
        bytes memory encoded = abi.encode(testMessage);
        
        string memory decoded = mq.decodeString(encoded);
        assertEq(decoded, testMessage);
    }

    function test_get_message_validation() public {
        // 测试消息ID验证 - ID为0应该失败
        vm.expectRevert(ZetaEasyMQ.ZetaMQ__InvalidMessageID.selector);
        mq.getMessage(0);
    }

    function test_get_message_nonexistent() public {
        // 测试不存在的消息ID
        vm.expectRevert(ZetaEasyMQ.ZetaMQ__InvalidMessageID.selector);
        mq.getMessage(1);
        
        vm.expectRevert(ZetaEasyMQ.ZetaMQ__InvalidMessageID.selector);
        mq.getMessage(999);
    }

    function test_get_latest_message_empty() public {
        // 测试没有消息时获取最新消息
        vm.expectRevert(ZetaEasyMQ.ZetaMQ__InvalidMessageID.selector);
        mq.getLatestMessage();
    }

    function test_onCall_success() public {
        // 模拟Gateway调用onCall
        string memory testMessage = "Hello from Sepolia!";
        bytes memory messageData = abi.encode(testMessage);
        
        MessageContext memory context = MessageContext({
            sender: abi.encodePacked(address(0xABCD)),
            senderEVM: address(0xABCD),
            chainID: 11155111 // Sepolia chain ID
        });

        // 以Gateway身份调用
        vm.prank(mockGateway);
        mq.onCall(context, address(0), 0, messageData);

        // 验证消息已存储
        assertEq(mq.messageCount(), 1);
        assertEq(mq.getTotalMessages(), 1);

        // 获取并验证消息内容
        ZetaEasyMQ.Message memory msg1 = mq.getMessage(1);
        assertEq(msg1.id, 1);
        assertEq(msg1.senderEVM, address(0xABCD));
        assertEq(msg1.sourceChainId, 11155111);
        assertEq(msg1.content, testMessage);
    }

    function test_onCall_empty_message() public {
        // 测试空消息
        MessageContext memory context = MessageContext({
            sender: abi.encodePacked(address(0xABCD)),
            senderEVM: address(0xABCD),
            chainID: 11155111
        });

        vm.prank(mockGateway);
        mq.onCall(context, address(0), 0, "");

        assertEq(mq.messageCount(), 1);
        ZetaEasyMQ.Message memory msg1 = mq.getMessage(1);
        assertEq(msg1.content, "[empty_message]");
    }

    function test_onCall_decode_failure() public {
        // 测试解码失败的情况
        bytes memory invalidData = hex"1234567890"; // 无法解码为string
        
        MessageContext memory context = MessageContext({
            sender: abi.encodePacked(address(0xABCD)),
            senderEVM: address(0xABCD),
            chainID: 11155111
        });

        vm.prank(mockGateway);
        mq.onCall(context, address(0), 0, invalidData);

        assertEq(mq.messageCount(), 1);
        ZetaEasyMQ.Message memory msg1 = mq.getMessage(1);
        assertEq(msg1.content, "[decode_failed]");
    }

    function test_onCall_unauthorized() public {
        // 测试非Gateway调用应该失败
        MessageContext memory context = MessageContext({
            sender: abi.encodePacked(address(0xABCD)),
            senderEVM: address(0xABCD),
            chainID: 11155111
        });

        // 以非Gateway身份调用应该revert
        vm.expectRevert();
        mq.onCall(context, address(0), 0, "");
    }

    function test_multiple_messages() public {
        // 测试多条消息
        MessageContext memory context = MessageContext({
            sender: abi.encodePacked(address(0xABCD)),
            senderEVM: address(0xABCD),
            chainID: 11155111
        });

        vm.startPrank(mockGateway);
        
        mq.onCall(context, address(0), 0, abi.encode("Message 1"));
        mq.onCall(context, address(0), 0, abi.encode("Message 2"));
        mq.onCall(context, address(0), 0, abi.encode("Message 3"));
        
        vm.stopPrank();

        assertEq(mq.messageCount(), 3);
        
        assertEq(mq.getMessage(1).content, "Message 1");
        assertEq(mq.getMessage(2).content, "Message 2");
        assertEq(mq.getMessage(3).content, "Message 3");
        
        // 测试getLatestMessage
        assertEq(mq.getLatestMessage().content, "Message 3");
    }

    function test_contract_deployment() public view {
        // 测试合约成功部署
        assert(address(mq) != address(0));
    }
}
```

-   以下是命令（要确保环境变量里有私钥）
    
    -   编译：forge build
        
    -   部署：forge script script/DeployZetaEasyMQ.s.sol:DeployZetaEasyMQ --rpc-url [https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public) --broadcast -vvvv
        
    -   发送跨链消息：zetachain evm call --gateway 0x0c487a766110c85d301d96e33579c5b317fa4995 --receiver 0x3e8759081a64541aD0F28A55cFd1a0E9AA1d3dd9 --rpc [https://ethereum-sepolia.publicnode.com](https://ethereum-sepolia.publicnode.com) --types string --values "Hello from Sepolia to ZetaChain!" --private-key %PRIVATE\_KEY% --yes
        
    -   确认收到消息的总数：cast call 0x3e8759081a64541aD0F28A55cFd1a0E9AA1d3dd9 "getTotalMessages()(uint256)" --rpc-url [https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)
        
    -   确认收到消息的内容：cast call 0x3e8759081a64541aD0F28A55cFd1a0E9AA1d3dd9 "getMessage(uint256)((uint256,address,bytes,uint256,string,uint256))" 1 --rpc-url [https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)
        
-   以下是最终结果
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/JoyWQ/images/2025-12-01-1764583615684-image.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->




-   使用example-contructs里面提供的封装好的Messaging.sol接口用来替换UniversalContract接口
    
-   对Sepolia发送消息到ZetaChain不成功的问题进行调试
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->





-   简单跨链消息传递的代码案例
    

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.26;

import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";

contract ZetaEasyMQ is UniversalContract {
    // 定义错误
    error ZetaMQ__InvalidMessageID();
    error ZetaMQ__AssetsNotAccepted();
    error ZetaMQ__MessageDecodeFailed();

    // 消息结构
    struct Message {
        uint256 id;
        uint256 sourceChainId;
        address sourceSender;
        string content;
        uint256 timestamp;
    }

    // 状态变量
    uint256 public messageCount;
    mapping(uint256 => Message) public messages;
    mapping(uint256 => uint256[]) public chainMessages;

    // 事件
    event MessageLogged(
        uint256 indexed id,
        uint256 sourceChainId,
        address indexed sourceSender,
        string content,
        uint256 timestamp
    );

    event MessageDecodeFailed(
        uint256 sourceChainId,
        address sourceSender,
        bytes messageData,
        string error
    );

    // 接收跨链调用
    function onCall(
        MessageContext calldata context,
        address zrc20,
        uint256 amount,
        bytes calldata message
        ) external override onlyGateway {

            // 记录任何调用（用于调试）
            emit MessageLogged(
                999999, // 特殊ID表示调试信息
                context.chainID,
                context.senderEVM,
                string(abi.encodePacked("DEBUG: Received call with ", uint2str(message.length), " bytes")),
                block.timestamp
            );

            // 1.安全检查：只接受纯消息，不接受代币转账
            if (amount != 0 || zrc20 != address(0)) {
                revert ZetaMQ__AssetsNotAccepted();
            }

            // 2.尝试消息解码
            string memory content;
            try this.safeDecodeMessage(message) returns (string memory decoded) {
                content = decoded;
            } catch {
                // 解码失败，记录错误但不revert
                emit MessageDecodeFailed(
                    context.chainID,
                    context.senderEVM,
                    message,
                    "Failed to decode as string"
                );
                return; // 退出，不记录消息
            }

            // 3.创建消息记录
            messageCount++;
            Message memory newMessage = Message({
                id: messageCount,
                sourceChainId: context.chainID,
                sourceSender: context.senderEVM,
                content: content,
                timestamp: block.timestamp
            });

            // 4.存储消息
            messages[messageCount] = newMessage;
            chainMessages[newMessage.sourceChainId].push(messageCount);

            // 5.触发事件
            emit MessageLogged(
                newMessage.id,
                newMessage.sourceChainId,
                newMessage.sourceSender,
                newMessage.content,
                newMessage.timestamp
            );
        }

    // 安全的消息解码函数
    function safeDecodeMessage(bytes calldata message) external pure returns (string memory) {
        return abi.decode(message, (string));
    }

    // 工具函数：uint转string
    function uint2str(uint256 _i) internal pure returns (string memory str) {
        if (_i == 0) {
            return "0";
        }
        uint256 j = _i;
        uint256 length;
        while (j != 0) {
            length++;
            j /= 10;
        }
        bytes memory bstr = new bytes(length);
        uint256 k = length;
        j = _i;
        while (j != 0) {
            bstr[--k] = bytes1(uint8(48 + j % 10));
            j /= 10;
        }
        str = string(bstr);
    }

    // 根据 ID 查询消息
        function getMessage(uint256 id) external view returns (Message memory) {
            if (id == 0 || id > messageCount) {
                revert ZetaMQ__InvalidMessageID();
            }
            return messages[id];
        }

        // 获取某个链的所有消息ID
        function getChainMessageIds(uint256 chainId) external view returns (uint256[] memory) {
            return chainMessages[chainId];
        }

        // 获取总消息数
        function getTotalMessages() external view returns (uint256) {
            return messageCount;
        }

}

// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.26;

import "forge-std/Test.sol";
import "../src/ZetaEasyMQ.sol";

contract ZetaEasyMQTest is Test {
    ZetaEasyMQ public mq;

    function setUp() public {
        // Mock the registry call to return address(0) for gateway
        vm.mockCall(
            0x7CCE3Eb018bf23e1FE2a32692f2C77592D110394,
            abi.encodeWithSignature("gatewayZEVM()"),
            abi.encode(address(0))
        );

        mq = new ZetaEasyMQ();
    }

    function test_initial_state() public {
        // 由于setUp中合约部署，检查初始状态
        assertEq(mq.messageCount(), 0);
        assertEq(mq.getTotalMessages(), 0);
    }

    function test_get_message_invalid_id() public {
        // 测试无效ID查询 - ID 0无效
        vm.expectRevert(ZetaEasyMQ.ZetaMQ__InvalidMessageID.selector);
        mq.getMessage(0);
    }


    function test_get_message_nonexistent() public {
        // 测试不存在的消息ID
        vm.expectRevert(ZetaEasyMQ.ZetaMQ__InvalidMessageID.selector);
        mq.getMessage(999);
    }

    /*
    // 暂时注释掉复杂的跨链测试 - 需要完整的ZetaChain测试环境
    function test_receive_messages_from_multiple_chains() public {
        RevertOptions memory revertOptions = RevertOptions({
            abortAddress: address(0),
            callOnRevert: false,
            onRevertGasLimit: 0,
            revertAddress: address(0),
            revertMessage: ""
        });

        // 从 Goerli (5) 发送消息
        evmSetup.wrapGatewayEVM(5).call(
            address(mq),
            abi.encode("Message 1 from Goerli"),
            revertOptions
        );

        // 从 BSC Testnet (97) 发送消息
        evmSetup.wrapGatewayEVM(97).call(
            address(mq),
            abi.encode("Message 2 from BSC"),
            revertOptions
        );

        // 从 Polygon Mumbai (80001) 发送消息
        evmSetup.wrapGatewayEVM(80001).call(
            address(mq),
            abi.encode("Message 3 from Polygon"),
            revertOptions
        );

        // 验证总消息数
        assertEq(mq.messageCount(), 3);

        // 验证每条消息的源链
        assertEq(mq.getMessage(1).sourceChainId, 5);
        assertEq(mq.getMessage(2).sourceChainId, 97);
        assertEq(mq.getMessage(3).sourceChainId, 80001);

        // 验证链消息索引
        uint256[] memory goerliMessages = mq.getChainMessageIds(5);
        assertEq(goerliMessages.length, 1);
        assertEq(goerliMessages[0], 1);
    }

    function test_query_chain_messages() public {
        RevertOptions memory revertOptions = RevertOptions({
            abortAddress: address(0),
            callOnRevert: false,
            onRevertGasLimit: 0,
            revertAddress: address(0),
            revertMessage: ""
        });

        // 从同一个链发送多条消息
        evmSetup.wrapGatewayEVM(5).call(
            address(mq),
            abi.encode("First message"),
            revertOptions
        );

        evmSetup.wrapGatewayEVM(5).call(
            address(mq),
            abi.encode("Second message"),
            revertOptions
        );

        evmSetup.wrapGatewayEVM(5).call(
            address(mq),
            abi.encode("Third message"),
            revertOptions
        );

        // 查询该链的所有消息
        uint256[] memory chainMsgIds = mq.getChainMessageIds(5);
        assertEq(chainMsgIds.length, 3);
        assertEq(chainMsgIds[0], 1);
        assertEq(chainMsgIds[1], 2);
        assertEq(chainMsgIds[2], 3);
    }
    */
}

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "forge-std/Script.sol";
import "../src/ZetaEasyMQ.sol";

contract DeployZetaEasyMQ is Script {
    // ZetaChain Athens 测试网 Gateway
    address constant GATEWAY = 0x6c533f7fE93fAE114d0954697069Df33C9B74fD7;

    // ZetaChain Athens RPC
    string constant RPC_URL = "https://zetachain-athens-evm.blockpi.network/v1/rpc/public";

    function run() external {
        uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");

        vm.startBroadcast(deployerPrivateKey);

        ZetaEasyMQ mq = new ZetaEasyMQ();

        console.log("==========================================");
        console.log("ZetaEasyMQ deployed!");
        console.log("Contract address:", address(mq));
        console.log("Gateway address:", GATEWAY);
        console.log("==========================================");

        vm.stopBroadcast();
    }
}
```

-   目前从Sepolia测试链发送消息到Zeta测试链有点问题，有可能是不兼容，明天换个测试链再试试
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->






-   设计跨链消息传递的入门demo用来驱动学习
    
-   编写合约代码，并做好部署、测试的相应准备
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->







-   通过FaucetMe网站领取ZetaChain的测试币，并在小狐狸钱包里手动配置ZetaChain的网络
    
-   学习Universal合约模式及核心的on call方法
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->








-   QWEN-AI控制台账号注册（资料里提供的是国际版，国内用户感觉可以自己搜索网页，不然电话验证会很麻烦）
    
-   QWEN API简单调用
    

```
curl -X POST "https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation" -H "Content-Type: application/json" -H "Authorization: Bearer sk-021ed70a929f43fa9b049e8dc6dcf689" -d "{\"model\": \"qwen-turbo\", \"input\": {\"messages\": [{\"role\": \"user\", \"content\": \"请用一句话介绍你自己。\"}]}, \"parameters\": {}}"
{"output":{"finish_reason":"stop","text":"我是一个大型语言模型，能够帮助用户回答问题、创作文字、逻辑推理、编程等多种任务。"},"usage":{"input_tokens":18,"output_tokens":22,"prompt_tokens_details":{"cached_tokens":0},"total_tokens":40},"request_id":"8ed95b94-3c6d-4a2d-a168-c92ce670e82c"}
```

-   Zetachain本地安装
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
