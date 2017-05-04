<h1>iTunes Connect 的 In-App Purchase 配置指南</h1>
[原文地址](https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnectInAppPurchase_Guide_SCh/Chapters/Introduction.html#//apple_ref/doc/uid/TP40014488-CH1-SW1)
<h2>简介</h2>

In-App Purchase 是一项 Apple 技术，通过该技术，用户可以从您的 app 中购买内容和服务。您可以通过 iTunes Connect（基于 Web 的工具套件）创建 In-App Purchase 产品。您可以使用 Store Kit 框架在您的 app 中实施 In-App Purchase。

例如，您可以使用 In-App Purchase 实施以下任何方案之一：

具有其他高级功能的 app 基础版本
允许用户购买和下载新书籍的电子书阅读器 app
提供新的探索环境（级别）的游戏
允许玩家购买虚拟资产的游戏
提供路线图服务访问权限的 app
订阅数字杂志和时事新闻
设计了在您的 app 中提供的一个或多个产品后，您可以在 iTunes Connect 中添加 In-App Purchase 配置信息。

![](https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnectInAppPurchase_Guide_SCh/Art/iap_flow_big_picture_2x.png)
如果您对 In-App Purchase 尚不熟悉，请阅读 [iOS 和 OS X 中的 In-App Purchase 使用入门](https://developer.apple.com/app-store/) 和 [将 In-App Purchase 添加到您的 iOS 和 Mac 应用程序中](https://developer.apple.com/library/content/technotes/tn2259/_index.html)。 如果您对 iTunes Connect 尚未熟悉，请阅读 iTunes Connect 开发者指南 (iTunes Connect Developer Guide)。

<h2>概况一览</h2>

使用 iTunes Connect 添加、设置测试、提交以及管理您的 In-App Purchase 产品配置。

配置您的 In-App Purchase 产品
为您的 app 创建 iTunes Connect 记录后，您可以配置要通过您的 app 提供的 In-App Purchase 产品。

**相关章节：** [创建 In-App Purchase 产品](https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnectInAppPurchase_Guide_SCh/Chapters/CreatingInAppPurchaseProducts.html#//apple_ref/doc/uid/TP40014488-CH3-SW1)， [以多种语言显示产品](https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnectInAppPurchase_Guide_SCh/Chapters/DisplayingInMoreLanguages.html#//apple_ref/doc/uid/TP40014488-CH32-SW1)
<h3>测试您的 In-App Purchase 产品</h3>
测试您配置的 In-App Purchase 产品，以确保这些产品显示在您的 app 商店中，并确保财务交易能正常运作。

**相关章节：** [测试 In-App Purchase 产品](https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnectInAppPurchase_Guide_SCh/Chapters/TestingInAppPurchases.html#//apple_ref/doc/uid/TP40014488-CH4-SW1)
<h3>提交您的 In-App Purchase 产品以供审核</h3>
添加与产品相关的所有信息并测试产品以确保在您的 app 商店中显示后，您可以提交该产品进行审核，并在商店中提供该产品。

**相关章节：** [提交 In-App Purchase 产品](https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnectInAppPurchase_Guide_SCh/Chapters/SubmittingInAppPurchases.html#//apple_ref/doc/uid/TP40014488-CH5-SW1)
<h3>管理您的 app 中可用的 In-App Purchase 产品</h3>
在 In-App Purchase 产品获得批准且准备就绪可供销售后，您可以继续在商店中管理产品的显示、价格以及可用情况。

**相关章节：** [使用您的产品的状态](https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnectInAppPurchase_Guide_SCh/Chapters/WorkingWithYourProductsStatus.html#//apple_ref/doc/uid/TP40014488-CH33-SW1)， [处理您产品的元数据](https://developer.apple.com/library/content/documentation/LanguagesUtilities/Conceptual/iTunesConnectInAppPurchase_Guide_SCh/Chapters/WorkingWithYourProductsMetadata.html#//apple_ref/doc/uid/TP40014488-CH7-SW1)
<h3>先决条件</h3>

要配置 iTunes Connect 中的 In-App Purchase 内容，您需要具有：

* 最新的 iOS 或 Mac Developer Program 许可协议 
请参阅 [Member Center](https://idmsa.apple.com/IDMSWebAuth/login?appIdKey=891bd3417a7776362562d2197f89480a8547b108fd934911bcbea0110d07f757&path=%2Faccount%2F&rv=1#agreements)。

* 最新的付费 app 合同，iOS 或 Mac 皆可
打开 iTunes Connect 中的“Contracts, Tax, and Banking”（合同、税务和银行）模块，如 [管理合同、税务和银行]() 中的 iTunes Connect 开发者指南。

* 具有管理员或技术人员角色的 iTunes Connect 用户帐户
请参阅 [创建用户帐户]() 中的 iTunes Connect 开发者指南。

* 您的 app 的 iTunes Connect 记录
请参阅 [添加新 app]() 中的 iTunes Connect 开发者指南。

<h3>另请参阅</h3>

当您在 app 中添加 In-App Purchase 产品后，您可能想要参考其他介绍相关业务和开发指南及要求的 Apple 资源：

* [iOS 和 OS X 中的 In-App Purchase 使用入门]() 提供了 In-App Purchase 市场营销策略和业务要求的概览。
* [添加功能]() 中的 《app 发布指南》 阐释了如何使用 Xcode 启用 In-App Purchase 等 Apple 服务。
* [iTunes Connect 的 In-App Purchase 配置指南]() 介绍使用 Store Kit 框架将商店嵌入到您的 app 中。
* [适用于开发者的 In-App Purchase]() 列出了在开发您的 app 和 In-App Purchase 内容时提供支持的可用参考。
* iTunes Connect 开发者指南 (iTunes Connect Developer Guide) 包含有关为您的 app 创建 iTunes Connect 记录以将其提交到 App Store 或 Mac App Store 的一般信息。此外，此文档还介绍了为销售您的 app 而需要采取的其他步骤，包括设置您组织的合同和银行信息以及提交 app 元数据（包括插图和本地化信息）。其中还包含有关如何监控您的 app 成功与否的信息。

纵观此文档，找到相关链接，阅读介绍本文档中更多特定主题的其他文档。

<h2>创建 In-App Purchase 产品</h2>
通过 In-App Purchase，您可以直接在您的免费或付费 app 中销售一系列虚拟产品。本章介绍了 In-App Purchase 产品类型以及如何在 iTunes Connect 中配置这些产品类型。

<h3>关于 In-App Purchase 产品</h3>

阅读 表 1-1 了解如何在不同的产品类型之间进行选择，以向客户提供虚拟内容或服务。

有关每种产品类型的开发事项的详细信息，请参阅 [设计您的 app 的产品]()。
表 1-1 In-App Purchase 产品类型

| 产品<br>类型  | 说明            |
|:----|:-------------------------------| 
| 消耗型<br>项目       | 使用一次后即消耗殆尽、需要再次购买的产品通常实施为消耗品。<br>例如，渔业 app 中的鱼食可以实施为消耗品。     | 
| 非消耗<br>型产品    | 用户只需购买一次非消耗型产品，而且这类产品不会过期或随着使用而减少。<br>例如，某个游戏中的新赛道可实施为非消耗型产品。Apple 可为您托管非消耗型产品。<br>请参阅 [通过 Apple 托管非消耗型产品]()。       | 
| 自动再<br>生订阅| 通过自动续订订阅，用户可以购买固定期限的动态内容，如杂志订阅。<br>除非用户选择停止续订，否则将自动续订订阅。<br>如果您要提供的内容不符合 [App 审核准则]()中概述的内容，<br>请考虑通过非续订订阅提供该内容。自动续订订阅可为与您共享联系人信息的客户提供激励。      | 
| 免费订<br>阅      | 通过免费订阅，用户可以下载固定期限的动态内容，如杂志订阅。对于开发者而言，<br>免费订阅是一种在 App Store 的“报刊杂志”中提供免费内容的方式。用户注册免费订阅后，<br>在与用户的 Apple ID 相关联的所有设备上都可以获取订阅内容。<br>请注意，免费订阅不会过期，且只能在启用了“报刊杂志”的 app 中提供。<br>与自动续订订阅不同，免费订阅不提供营销选择激励，但是会提示用户共享他们的信息。<br>免费订阅 不适用于 Mac app。 |
| 非再生<br>订阅 | 非续订订阅允许在限定的期限内销售产品。它们用于提供基于时间的静态内容访问的产品。<br>如果使用非续订订阅，您的 app 负责将订阅传送到与用户的 Apple ID 相关联的所有设备。<br>由于非续订订阅需要用户在每次订阅结束时进行续订，因此您的 app 必须含有可识别订阅何时过期以及何时应提醒用户购买新订阅的代码。| 

<h3>配置产品</h3>

对于每个 app，您可以创建多达 1000 个单独的 In-App Purchase 产品。您要在商店中提供的每个产品都必须在 iTunes Connect 中进行配置。由于 In-App Purchase 产品与单个 app 相关联，因此您需要在 iTunes Connect 的“App Summary”（app 摘要）页面中创建这些产品。





