# Minecraft函数调试器

![banner.png](https://i.loli.net/2021/02/17/3lkRqAjT5hNGorJ.png)

语言:

[English](https://github.com/hugeBlack/McfDebugger_Extension/blob/master/README.md) / 简体中文

## 简介

你还在为写函数的时候哪个指令没有执行而不知所措吗？你还在为调试而各种/say吗？你还在为不知道哪个实体执行了function而一头雾水吗（跑

Minecraft Function Debugger (VSCode Extension Part)，简称 McFD，中文名「函数调试器(VSCode插件部分)」是一个能够为 Minecraft Java版的函数提供调试支持的VSCode插件，你可以使用它像调试其他语言一样调试mcfunction

若要使用此插件，你必须安装 [对应的Fabric模组](https://github.com/hugeBlack/McfDebugger_Mod/releases) 来和游戏通信

## 特性

**简体中文**：

* 创建/取消断点
* 逐语句执行
* 在出现异常时暂停并查看异常
* 查看某条指令输出
* 查看当前函数执行环境(执行实体，位置等)
* 查看当前调用堆栈
* 获取实体/计分板信息
* 重启调试时自动重载数据包

## 开始

1. 打开一个数据包工作区  
   （请注意：目录结构请务必至少包含data文件夹）
2. 打开运行，创建launch.json
3. 应该会自动填写所需的配置，如果没有的话请自行指定端口号(port)，默认为1453。  
   如果在游戏中进行过配置请自行调整  
   更详细的设置请详见"launch.json"章节
4. 点击开始调试
5. **警告：不要使用单步跳出和单步跳过功能，会导致出现问题**(例如你的游戏可能会卡住)！这两个功能可能会在将来的版本实装

## 用法

您正在使用VSCode，显然，您熟知基本的调试方法(创建断点，步过等)我们在此处不再赘述。

在这里介绍所谓「调试器指令」。

调试器指令是由#@开头的指令。我们这样设计，可以使得即便您忘记了在发布前删除，也不会造成太大的影响。它们是如下几个：

### #@loud
  
  该指令用于强制其下方的指令输出执行结果并暂停，像这样：

  ```mcfunction
  #@loud
  fill ~-1 ~ ~-1 ~1 ~ ~1
  ```

  运行到fill指令的时候，调试器会**暂停**游戏并以异常的形式输出执行结果

### #@log

  该指令用于强制其下方的指令输出执行结果但**不暂停**，像这样：

  ```mcfunction
  #@log
  fill ~-1 ~ ~-1 ~1 ~ ~1
  ```

  运行到fill指令的时候，调试器会在调试控制台中输出执行结果

### #@mute

  该指令用于使调试器忽略下方指令产生的错误，像这样：

  ```mcfunction
  #@mute
  kill Huge_Black
  ```

  一般来说，玩家Huge_Black不在您的游戏中，所以这条指令会抛出"实体不存在"异常，但因为mute指令的存在，调试器会忽略这个错误且不暂停游戏

### #@getScoreboard

  该指令用于暂停并输出请求的计分板，语法如下：

  ```mcfunction
  #@getScoreboard byEntity <选择器/假名>
  ```

  可获取对应选择器/假名的所有分数

  ```mcfunction
  #@getScoreboard byObjective <计分板名>
  ```

  可获取指定计分板的所有记录的分数

### #@getEntity

  该指令用于暂停并输出选择器获得的实体的一些信息

  ```mcfunction
  #@getEntity <选择器>
  ```

值得注意的是，**如果调试器指令语法错误也会抛出对应的异常**  

## launch.json配置

在launch.json中，你可以设置启用或禁用一些功能，你可以在“features”项中设置他们。一个完整的launch.json可以像这样：

```json  
{
  "version": "0.2.0",
  "configurations": [
      {
          "type": "mcf",
          "request": "launch",
          "name": "Start Debugging",
          "port": 1453,
          "features": [
              "--stopOnException"
          ]
      }
  ]
}
```

现在可用的特性有：  

* --stopOnException：  
  禁用所有异常暂停功能，转为控制台输出异常
* debugThisMod：  
  在Minecraft的日志中输出调试信息

## 设置语言

你可以在vscode的设置中搜索"mcfdebugger.display_language"设置语言。目前支持zh_cn和en_us
