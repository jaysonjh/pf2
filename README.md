# pf2

#### 介绍
写于十几年前飞狐软件纯C语言的飞狐版缠论公式DLL，现在改造为通达信版本

#### 安装教程

编译成32位，下面以Visual Studio 2019举例，作者用的是MS Builder。
在VSCode 里设置运行任务：

{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "msbuild",
            "args": [
                "FoxFunc.vcxproj",
                "/p:Configuration=Debug",
                "/p:Platform=Win32"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": {
                "reveal": "silent"
            },
            "problemMatcher": "$msCompile"
        }
    ]
}

#### 特色

由于千人千缠，笔者理解的缠论与其他人不太一样，在处理笔时，如果笔内K线有高低，会被视为笔内波动，高低点合并进笔处理（我公式也没完全处理好，计划修正中）。
十多年的缠论实践，没有发财，但理解了禅师的很多理念，现在重新再出发，重新再写一遍，由于走势延伸，背了又背等问题，要把假信号也保留以便盘后统计分析。
另外关于操作级别，笔者这里是没有笔中枢概念的，为了走势的稳定，一切以线段走势为准，一切操作信号也以段信号为准。

### 样例图

![输入图片说明](https://foruda.gitee.com/images/1740223864137426611/caa93000_11322109.png "屏幕截图")
![输入图片说明](https://foruda.gitee.com/images/1740224005678728817/ce9dc5c8_11322109.png "屏幕截图")
