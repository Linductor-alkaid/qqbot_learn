## 第三步——怎么做（配置环境，部署qq机器人）
怎么办，这么多部分，看起来要做一个qq机器人似乎很复杂。
天塌了总有大佬顶着，不管是反向websocket客户端还是机器人主体都有大佬们写好的框架可以使用，我们可以直接借来部署，实现自己的聊天机器人。

### 反向websocket客户端——LLonebot

2023年QQ更新了nt框架的版本，同时也增强了对于机器人登录的检测，像go-cqhttp等以前的qq客户端都很难在连接到qq上了。而适配QQNT的插件加载器[LiteLoaderQQNT](https://liteloaderqqnt.github.io/)利用了QQ官方的客户端，直接获取websocket的信息，绕过了登录检测，但是这也意味着需要在打开qq的情况下才能使用，也就是无法在没有桌面的服务器中使用。
利用LiteLoaderQQNT插件加载器中的插件[LLonebot](https://github.com/LLOneBot/LLOneBot)可以使你的 NTQQ 支持[ OneBot 11 协议](https://onebot.dev/)进行 QQ 机器人开发。

这一节不讲复杂的原理，只讲怎么部署，想要了解这些内容的工作方式与原理的可以直接点链接去它们的官网查看。

### 下载安装插件管理器LiteLoaderQQNT

在`LiteLoaderQQNT`的文档中有提到具体的[下载方法](https://liteloaderqqnt.github.io/guide/install.html)，或者你可以使用下载工具下载。
- [Linux下载工具](https://github.com/Mzdyl/LiteLoaderQQNT_Install/releases/download/1.16/install.sh)
- [Windows下载工具](https://github.com/Mzdyl/LiteLoaderQQNT_Install/releases/download/1.16/install_windows.exe)

mac/linux 终端运行脚本 install.sh即可
windows 下载exe，双击运行即可，全自动安装（如遇权限问题请右击使用管理员权限运行）（摘自github中[该项目](https://github.com/Mzdyl/LiteLoaderQQNT_Install/releases)的下载界面）

下载好后打开QQ可能还会报一个需要修改文件地址的操作的提示框，按照提示操作后重新打开QQ，就可以在QQ的设置界面看到插件加载器`LiteLoaderQQNT`了。

### 下载插件LLonebot
参考官方的[下载方法](https://llonebot.github.io/zh-CN/guide/getting-started)

### 机器人主体——nonebot2

[nonebot](https://nonebot.dev/)是一个基于Python的机器人框架，它提供了丰富的插件系统，可以轻松地扩展机器人的功能。nonebot支持多种协议，包括QQ、Telegram、Discord等，因此你可以根据自己的需求选择合适的协议。

### 部署nonebot2

由于nonebot2对python环境有要求，所以建议使用虚拟环境或者anaconda来部署nonebot2，具体步骤如下：