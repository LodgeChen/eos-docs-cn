Transaction API
---

定义用于发送事务和内联消息的API
> [Transaction C API]()   
定义用于发送事务的API

> [Transaction C++ API]()   
类型安全的Trasaction C API的C++封装

详细描述
---

A EOS.IO事务具有以下抽象结构：
```C++
struct transaction {
  Name scope[]; 
  Name readScope[]; 
  message messages[]; 
};
```

该API使您的合约能够构建并且发送交易.

延期交易直到未来的区块才会被处理。因此，只要它们看起来形成良好，它们就可以不影响其父交易的成功。如果任何其他情况导致父事务被标记为失败，那么延期事务将永远不会被处理。

延期交易必须遵守父交易可用的权限，或者将来可以委托给合约帐户供将来使用。

内联消息允许一个合约向另一个合约发送一个消息，该消息在当前消息处理结束后立即处理，使得父事务的成功或失败取决于消息的成功。如果内联消息在处理中失败，那么根源于该块的整个事务和消息树将被标记为失败，并且数据库上的任何影响都不会持续。

由于这一点以及事务应用程序的并行性质，内联消息可能不会影响未在其父事务范围中列出的任何范围。他们也可能不会读取其父事务范围或readScope中未列出的任何范围。

内联消息和延期交易必须遵守父交易可用的权限，或者将来可以委托给合同帐户供将来使用。