# 通过钻石标准解决以太坊合约大小限制

以太坊合约包含太多函数和代码，将会达到合约24KB大小的最大限制。对于某些类型的合约来说，这是一个真正的问题。

一些合约标准需要许多功能。例如，[ERC1400安全代币标准](https://github.com/ethereum/eips/issues/1411
)需要27个函数和13个事件。[ERC-998可组合不可替换代币](https://eips.ethereum.org/EIPS/eip-998)标准指定了31个函数。通过实现这些标准的其他特定于应用程序的代码合约，可以很容易地超出24KB的限制。

即使不是实现大型标准合约，一些开发人员也会希望开发大型合约，以便将相关代码放在同一个以太坊地址下。而且，如果状态和功能一起保存在同一个以太坊地址下，那么访问和修改合约存储就更容易、更灵活。

## 怎么办呢?

有人提议增加或取消合约的最大规模限制。但维塔利克.布特林(Vitalik Buterin)出于技术原因反对该协议，并捍卫使用“代理合约”和“委托合约”的做法。代理合约是指通过使用称为“delegatecall”的低层操作通过调用其他合约功能来保持当前合约较小规模的合约。

基于以上分歧，我认为把如何操作进行标准化会是一个好主意。如何创建代理合约来模拟可能超过24kb大小限制的大型合约。

这就是为什么我制定了[钻石标准](https://github.com/ethereum/EIPs/issues/2535)。标准规范如何去创建一个小合约，这个合约可以像使用当前合约的代码一样使用任何数量的其他合约的代码。
​

执行钻石标准的合约称为钻石。“钻石”一词用于将钻石与只能从单个合约借用代码的常规合约和代理合约区分开来。

此外，术语“钻石”用来建立一个形象的概念，用于了解其如何工作。真正的钻石有不同的侧面，叫做切面（facet）。可以设想，一个在以太坊钻石合约也有不同的切面。每一个钻石借用功能的合约都是不同的侧面或切面（facet）。

钻石标准使用类比的方式扩展了“钻石切割”的功能 ，用于增加，替换，或删除切面和功能。这类似于给予一个真正的钻石新切面，而是通过合约来切割。


此外，钻石标准提供了称为“放大镜(The Loupe)”的功能，返回关于切面的信息和钻石存在的功能。在钻石行业，“放大镜”是一种用来检查钻石的工具。


钻石标准中定义的新术语与真钻石的类比是一致的。这用于定义和区分钻石和其他类型的合约。不幸的是，新的术语可能会成为一些人学习该标准的障碍。但是新的术语很少，我们在本文中已经介绍了这些新术语。术语是:**钻石(diamond)**，**钻石切割(diamondCut)**，**切面(facet)**和**放大镜(The Loupe)**。

我最近写了一篇关于钻石的更深入的文章，其中包括如何开始实现钻石标准:[理解以太坊钻石标准](https://dev.to/mudgen/understanding-diamonds-on-ethereum-1fb)

不可不提的是钻石标准包括一个创建可升级钻石合约的灵活而透明的方法。实现钻石标准的合约可升级是可选的，但有些人可能正是冲着可升级功能而创建钻石标准合约的。



## 钻石标准越来越受欢迎



上个月，[ConsenSys Diligence](https://diligence.consensys.net) 对[Codefi](https://codefi.consensys.net/)的合约进行了一次公共安全审计。ConsenSys Diligence[建议](https://diligence.consensys.net/audits/2020/06/codefi-erc1400-assessment/#diamond-standard)Codefi使用钻石标准来解决合约最大规模限制问题。

ERC-1155多代币标准[提到](https://eips.ethereum.org/EIPS/eip-1155#upgrades)了升级合约的钻石标准(EIP-2535)。

许多个人和公司联系了我，告诉我他们正在使用钻石标准或正在为他们的系统实现钻石标准。以下是一些公开发表过相关文章的人提供的一些信息:

[VolleyFire](http://joeyzacherl.com/2018/10/volleyfire-liquidity-provider-for-decentralized-exchanges/)，一个分散式交易所的流动性供应商正在使用钻石标准。

VolleyFire的开发人员乔伊·扎克尔发布了一个名为[Diamond Setter](https://github.com/lampshade9909/DiamondSetter)的Python工具，它是钻石合约管理器。以下是他的博客文章:[Diamond Setter,以太坊智能合约管理器](http://joeyzacherl.com/2020/06/diamond-setter-ethereum-smart-contract-manager)



Ronan Sandford ([wighawag](https://twitter.com/wighawag))是一位杰出的智能合约开发人员，也是ERC-1155标准的作者，他[宣布](https://twitter.com/wighawag/status/1280992800545349644)他正在致力于在[buidler-deploy](https://github.com/wighawag/buidler-deploy#readme)中增加对钻石的支持，从而使钻石的部署/切割变得非常容易。buidler-deploy是一种将合约部署到任何网络、跟踪它们并复制相同环境进行测试的机制。



[Nayms](https://nayms.io/)在生产环境中使用钻石。[Ram](https://twitter.com/hiddentao)写了一篇关于其实现的博文:[使用钻石标准的可升级智能合约](https://hiddentao.com/archives/2020/05/28/upgradeable-smart-contracts-using-diamond-standard)

在Reddit上关于钻石标准第一个[受欢迎的帖子](https://www.reddit.com/r/ethereum/comments/gze6k3/a_diamond_is_a_set_of_contracts_that_can_access)。

如果你想了解更多关于钻石的知识，请阅读这篇文章:[理解以太坊的钻石标准](https://dev.to/mudgen/understanding-diamonds-on-ethereum-1fb)。



## 加入讨论

我最近创建了一个[Discord论坛](https://discord.gg/kQewPw2)来讨论钻石和钻石标准。

原文链接：https://dev.to/mudgen/ethereum-s-maximum-contract-size-limit-is-solved-with-the-diamond-standard-2189 作者：[Nick Mudge](https://dev.to/mudgen)

