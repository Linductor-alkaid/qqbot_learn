# 深度学习项目实践——qq聊天机器人（transformer）（二）配置环境与部署

上一节我们讲解了qq聊天的原理和qq机器人的框架以及运行流程，这一节我们来讲怎么配置环境，部署qq机器人。

## 第一步——配置环境

有关代替qq客户端实现反向websocket连接这一部分内容由于一些原因，我无法在这里具体讲解，我把这部分内容放到了github中[我的项目](https://github.com/Linductor-alkaid/qqbot_learn)内,请自行查看。


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
下载对应系统版本的压缩包，然后在`LiteLoaderQQNT`的插件管理器中选择安装插件导入，再重启qq就完成了。
当打开qq设置可以看到`llonebot`的时候，就说明已经安装成功了。

### 机器人主体——nonebot2

[nonebot](https://nonebot.dev/)是一个基于Python的机器人框架，它提供了丰富的插件系统，可以轻松地扩展机器人的功能。nonebot支持多种协议，包括QQ、Telegram、Discord等，因此你可以根据自己的需求选择合适的协议。

### 部署nonebot2

由于nonebot2对python环境有要求，所以建议使用虚拟环境或者anaconda来部署nonebot2，具体步骤如下：

#### 推荐方法：使用anaconda配置环境

具体anaconda的安装方法可以参考我之前发过的在[ubuntu上配置pytorch环境](http://t.csdnimg.cn/B30Yi)的文章
从官网下载（参考： [http://t.csdnimg.cn/5CWjX](http://t.csdnimg.cn/5CWjX) ）

下载地址传送门：

    官网首页：https://www.anaconda.com/
    官网下载页：https://www.anaconda.com/products/individual#Downloads

直接选择相应的installer即可
![下载页面](https://i-blog.csdnimg.cn/direct/6fc86cb701a94e76ba409bd03b9dc83f.png)


# 安装python3.10
在 Ubuntu 中，可以通过 Anaconda 创建一个 Python 3.10 的 Conda 虚拟环境。以下是详细步骤：

1. **打开终端**：

   打开你的终端（Terminal）。
2. **进入conda（如果终端中用户左边没有出现(base)）**

   使用以下命令进入conda环境
   ```bash
   cd ~
   source anaconda3/bin/activate
   ```

3. **创建 Conda 虚拟环境**：

   使用以下命令创建一个新的虚拟环境，并指定 Python 版本为 3.10：

   ```bash
   conda create -n qqbot python=3.10
   ```

   其中，`qqbot` 是你为虚拟环境指定的名字，请记住它，以便以后激活该环境。

4. **激活虚拟环境**：

   创建完成后，激活你的虚拟环境：

   ```bash
   conda activate qqbot
   ```

5. **安装所需的包（nonebot2）**：

   我们现在安装nonebot2,使用nonebot脚手架，可以参考nonebot2的[官方文档](https://nonebot.dev/)进行安装

   ```bash
   pip install nb-cli
   ```

6. **查看虚拟环境列表（可选）**：

   查看所有的 Conda 虚拟环境可以运行：

   ```bash
   conda env list
   ```

7. **退出虚拟环境（可选）**：

   当完成工作后，可以退出虚拟环境：

   ```bash
   conda deactivate
   ```
## 第二步：部署简单的qq机器人

### 创建nonebot2项目

参考nonebot2的[官方文档](https://nonebot.dev/)，使用nonebot脚手架创建一个简单的nonebot2项目

在打开虚拟环境的终端中输入以下命令：

```bash
nb create
```

这一指令将会执行创建项目的流程，你将会看到一些询问：

- 项目模板
```bash
    [?] 选择一个要使用的模板: bootstrap (初学者或用户)
```
这里我们选择 bootstrap 模板，它是一个简单的项目模板，能够安装商店插件。如果你需要自行编写插件，这里请选择 simple 模板。

- 项目名称
```bash
[?] 项目名称: qqbat
```
这里我们以 qqbot 为例，作为项目名称。你可以根据自己的需要来命名。

- 其他选项 请注意，多选项使用空格选中或取消，回车确认。
```bash
[?] 要使用哪些驱动器? FastAPI (FastAPI 驱动器)
[?] 要使用哪些适配器? Console (基于终端的交互式适配器)
[?] 立即安装依赖? (Y/n) Yes
[?] 创建虚拟环境? (Y/n) Yes
```
这里我们选择了创建虚拟环境，nb-cli 在之后的操作中将会自动使用这个虚拟环境。如果你不需要自动创建虚拟环境或者已经创建了其他虚拟环境，nb-cli 将会安装依赖至当前激活的 Python 虚拟环境。

- 选择内置插件
```bash
[?] 要使用哪些内置插件? echo
```
这里我们选择 echo 插件作为示例。这是一个简单的复读回显插件，可以用于测试你的机器人是否正常运行。

### 启动nonebot2项目

在命令行中进入项目文件夹，然后输入以下命令启动nonebot2项目：

```bash
cd qqbot # 项目名称，进入项目文件夹
nb run
```

现在应该已经运行起来了你的第一个 NoneBot 项目了！请注意，生成的项目中使用了 FastAPI 驱动器和 Console 适配器，你之后可以自行修改配置或安装其他适配器。但此时的qq机器人并没有登陆qq，也无法与qq进行交互，接下来我们需要在qq配置反向websocket客户端

### 配置反向websocket客户端

此部分内容由于一些原因无法在此处展示，请查阅github中[该项目](https://github.com/Linductor-alkaid/qqbot_learn)中这一节的内容

打开qq，右下角打开qq设置
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6f9624e320414778bdd0e2015041a1a7.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c0c9d75b97ee43cdb2f76809c9c2aa27.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f4fe9c9b1bb640d785df7a561956c5ab.png)
添加反响websocket服务的监听地址
```
ws://127.0.0.1:8080/onebot/v11/ws
```

修改完成后向下划找到保存，保存后就能在终端中看到提示`nonebot`连接到qq了，这个时候我们的简单qq机器人就部署完成了。

### 尝试使用

在项目运行起来后，`Console` 适配器会在你的终端启动交互模式，你可以直接在输入框中输入 `/echo hello world` 来测试你的机器人是否正常运行。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/296dd24f1157422684ed55e5583a647d.png)