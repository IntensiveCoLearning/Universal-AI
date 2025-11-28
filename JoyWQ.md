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
