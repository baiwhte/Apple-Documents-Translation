<h1>iOS App编程指南</h1>
[原文地址](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007072)
<h2>关于 iOS App 架构</h2>
Apps基于iOS运行以确保提供极好的用户体验。一个极好的用户体验不仅仅是一个好的app设计和用户界面，还包括其它许多因素。用户希望iOS App可以流畅同时希望App尽可能的使用电源。Apps需要支持所有最新的iOS设备，同时仍然显示为应用程序针对当前设备。开始实现这些所有的功能似乎很困难，但是iOS在你需要帮助的时候提供帮助。
![](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Art/ios_pg_intro_2x.png)

该文档强调了使您的App在iOS上正常运行的核心行为。 您可能不会实现本文档中描述的每个功能，但您应该为您创建的每个项目考虑这些功能。

```
注意: 开发iOS apps需要一台基于Intel-based Macintosh并安装了iOS SDK的电脑。了解更多信息关于如何获取iOS SDK，跳转到[iOS Dev Center](https://developer.apple.com/download/)
```
<h2>At a Glance</h2>

When you are ready to take your ideas and turn them into an app, you need to understand the interactions that occur between the system and your app.

Apps Are Expected to Support Key Features
The system expects every app to have some specific resources and configuration data, such as an app icon and information about the capabilities of the app. Xcode provides some information with every new project but you must supply resource files and you must make sure the information in your project is correct before submitting your app.

```
Relevant Chapter: [Expected App Behaviors](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/ExpectedAppBehaviors/ExpectedAppBehaviors.html#//apple_ref/doc/uid/TP40007072-CH3-SW2)
```
Apps Follow Well-Defined Execution Paths
From the time the user launches an app to the time it quits, apps follow a well-defined execution path. During the life of an app, it can transition between foreground and background execution, it can be terminated and relaunched, and it can go to sleep temporarily. Each time it transitions to a new state, the expectations for the app change. A foreground app can do almost anything but background apps must do as little as possible. You use the state transitions to adjust your app’s behaviors accordingly.

Relevant Chapter: The App Life Cycle, Strategies for Handling App State Transitions
Apps Must Run Efficiently in a Multitasking Environment
Battery life is important for users, as is performance, responsiveness, and a great user experience. Minimizing your app’s usage of the battery ensures that the user can run your app all day without having to recharge the device, but launching and being ready to run quickly are also important. The iOS multitasking implementation offers good battery life without sacrificing the responsiveness and user experience that users expect, but the implementation requires apps to adopt system-provided behaviors.

Relevant Chapters: Background Execution, Strategies for Handling App State Transitions
Communication Between Apps Follows Specific Pathways
For security, iOS apps run in a sandbox and have limited interactions with other apps. When you want to communicate with other apps on the system, there are specific ways to do so.

Relevant Chapter: Inter-App Communication
Performance Tuning is Important for Apps
Every task performed by an app has a power cost associated with it. Apps that drain the user’s battery create a negative user experience and are more likely to be deleted than those that appear to run for days on a single charge. So be aware of the cost of different operations and take advantage of power-saving measures offered by the system.

Relevant Chapter: Performance Tips
How to Use This Document

This document is not a beginner’s guide to creating iOS apps. It is for developers who are ready to polish their app before putting it in the App Store. Use this document as a guide to understanding how your app interacts with the system and what it must do to make those interactions happen smoothly.

Prerequisites

This document provides detailed information about iOS app architecture and shows you how to implement many app-level features. This book assumes that you have already installed the iOS SDK, configured your development environment, and understand the basics of creating and implementing an app in Xcode.

If you are new to iOS app development, read Start Developing iOS Apps (Swift). That document offers a step-by-step introduction to the development process to help you get up to speed quickly. It also includes a hands-on tutorial that walks you through the app-creation process from start to finish, showing you how to create a simple app and get it running quickly.

See Also

If you are learning about iOS, read iOS Technology Overview to learn about the technologies and features you can incorporate into your iOS apps.




