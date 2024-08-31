# 深度学习项目实践——QQ聊天机器人（transformer）（三）NoneBot2插件结构与编写

在前两节中，我们详细讲解了QQ聊天的原理、QQ机器人的框架与环境配置的流程。本节将重点介绍NoneBot2的插件构成，以及如何从零开始编写一个属于自己的插件。

## 第一步：了解NoneBot2的架构

在开始编写插件之前，我们先来了解一下NoneBot2的基本架构和构成。

### 1. NoneBot2的基本架构

NoneBot2 是一个基于Python的插件化机器人框架，具有高度扩展性和灵活性。它的架构主要由以下几个核心部分组成：

- **机器人主体（Bot）：** 这是整个系统的核心，负责管理和协调插件的运行。它可以被看作是一个大脑，控制着整个机器人的逻辑流。

- **驱动器（Driver）：** 驱动器负责与外部世界进行通信，例如与QQ服务器建立连接、发送和接收消息等。NoneBot2 支持多种驱动器，常见的有 FastAPI 和 WebSocket。

- **适配器（Adapter）：** 适配器用来将不同平台的消息格式转化为NoneBot2可以理解的标准格式。适配器决定了机器人支持的平台，如QQ、Telegram等。

- **插件（Plugin）：** 插件是NoneBot2的灵魂，几乎所有的功能都由插件实现。你可以通过编写插件来扩展机器人的能力，比如实现关键词回复、自动化任务等。

### 2. NoneBot2 项目的基本构成

一个典型的NoneBot2项目由以下几个文件和文件夹组成：

- **bot.py:** 这是NoneBot2机器人的入口文件，负责启动整个机器人。
- **plugins/**: 这是存放所有插件的文件夹，每个插件都是一个独立的Python文件或模块。
- **config.py:** 配置文件，用于设置机器人的相关配置，如日志级别、插件加载选项等。

了解了这些基本构成之后，我们就可以开始编写插件了。

## 第二步：编写自己的NoneBot2插件

编写插件是NoneBot2开发中最有趣的部分，它允许你根据自己的需求为机器人添加各种功能。下面我们通过一个简单的例子，展示如何编写一个插件。

## 插件编写准备

在正式编写插件之前，我们需要先了解一下插件的概念和结构。

### 插件结构

在 NoneBot 中，插件即是 Python 的一个模块（module）。NoneBot 会在导入时对这些模块做一些特殊的处理，使得它们成为一个插件。插件间应尽量减少耦合，可以进行有限制的相互调用，NoneBot 能够正确解析插件间的依赖关系。

### 单文件插件

一个普通的 `.py` 文件即可以作为一个插件，例如创建一个 `foo.py` 文件：

```
Project
📂 plugins
└── 📜 foo.py
```

这个时候模块 `foo` 已经可以被称为一个插件了，尽管它还什么都没做。

### 包插件

一个包含 `__init__.py` 的文件夹即是一个常规的 Python 包（package），例如创建一个 `foo` 文件夹：

```
Project
📂 plugins
└── 📂 foo
    └── 📜 __init__.py
```

这个时候包 `foo` 同样是一个合法的插件，插件内容可以在 `__init__.py` 文件中编写。

## 创建插件

### 项目结构调整

如果在之前的快速上手章节中已经使用 `bootstrap` 模板创建了项目，那么你需要做出如下修改：

1. 在项目目录中创建一个两层文件夹 `qqbot/plugins`

```
Project
📦 qqbot
├── 📂 qqbot
│   └── 📂 plugins
├── 📜 pyproject.toml
└── 📜 README.md
```

2. 修改 `pyproject.toml` 文件中的 `nonebot` 配置项，在 `plugin_dirs` 中添加 `qqbot/plugins`

```toml
[tool.nonebot]
plugin_dirs = ["qqbot/plugins"]
```

### 加载插件

如果你使用自定义的入口文件 `bot.py`，那么你需要在 `bot.py` 中加载插件：

```python
import nonebot

nonebot.init()

# 加载插件
nonebot.load_builtin_plugins("echo")  # 内置插件
nonebot.load_plugins("qqbot/plugins")  # 本地插件

nonebot.run()
```

## 使用 nb-cli 创建插件

创建插件可以通过 `nb-cli` 命令从完整模板创建，也可以手动新建空白文件。通过以下命令创建一个名为 `weather` 的插件：

```bash
$ nb plugin create
[?] 插件名称: weather
[?] 使用嵌套插件? (y/N) N
[?] 输出目录: qqbot/plugins
```

`nb-cli` 会在 `qqbot/plugins` 目录下创建一个名为 `weather` 的文件夹，其中包含的文件将在稍后章节中用到。

```
Project
📦 qqbot
├── 📂 qqbot
│   └── 📂 plugins
│       └── 📂 weather
│           ├── 📜 __init__.py
│           └── 📜 config.py
├── 📜 pyproject.toml
└── 📜 README.md
```

## 加载插件的方式

加载插件是在机器人入口文件中完成的，需要在框架初始化之后，运行之前进行。以下是为加载插件提供的几种方式：

### load_plugin

通过点分割模块名称或使用 `pathlib` 的 `Path` 对象来加载插件。通常用于加载第三方插件或者项目插件。例如：

```python
from pathlib import Path

nonebot.load_plugin("path.to.your.plugin")  # 加载第三方插件
nonebot.load_plugin(Path("./qqbot/plugins/weather.py"))  # 加载项目插件
```

### load_plugins

加载传入插件目录中的所有插件，通常用于加载一系列本地编写的项目插件。例如：

```python
nonebot.load_plugins("qqbot/plugins", "path/to/your/plugins")
```

### load_all_plugins

这种加载方式是以上两种方式的混合，加载所有传入的插件模块名称，以及所有给定目录下的插件。例如：

```python
nonebot.load_all_plugins(["path.to.your.plugin"], ["qqbot/plugins"])
```

### load_from_json

通过 JSON 文件加载插件，是 `load_all_plugins` 的 JSON 变种。通过读取 JSON 文件中的 `plugins` 字段和 `plugin_dirs` 字段进行加载。例如：

```json
{
  "plugins": ["path.to.your.plugin"],
  "plugin_dirs": ["qqbot/plugins"]
}
```

```python
nonebot.load_from_json("plugin_config.json", encoding="utf-8")
```

### load_from_toml

通过 TOML 文件加载插件，是 `load_all_plugins` 的 TOML 变种。通过读取 TOML 文件中的 `[tool.nonebot]` 表中的 `plugins` 和 `plugin_dirs` 数组进行加载。例如：

```toml
[tool.nonebot]
plugins = ["path.to.your.plugin"]
plugin_dirs = ["qqbot/plugins"]
```

```python
nonebot.load_from_toml("plugin_config.toml", encoding="utf-8")
```

### load_builtin_plugin

加载一个内置插件，传入的插件名必须为 NoneBot 内置插件。该方法是 `load_plugin` 的封装。例如：

```python
nonebot.load_builtin_plugin("echo")
```

### load_builtin_plugins

加载传入插件列表中的所有内置插件。例如：

```python
nonebot.load_builtin_plugins("echo", "single_session")
```

---

