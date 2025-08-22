
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
#### 通达信中使用
![输入图片说明](image.png)

P:=26;S:=12;M:=9;
DIFF : EMA(CLOSE,S) - EMA(CLOSE,P),COLOR000088;
DEA : EMA(DIFF,M),COLOR008800;
MACD :=2*(DIFF-DEA);

STICKLINE(MACD>0&&MACD>=REF(MACD,1),0,MACD,6,0),COLOR000044;
STICKLINE(MACD>0&&MACD<REF(MACD,1),0,MACD,6,0),COLOR000033;
STICKLINE(MACD<0&&MACD>=REF(MACD,1),0,MACD,6,0),COLOR444400;
STICKLINE(MACD<0&&MACD<REF(MACD,1),0,MACD,6,0),COLOR555500;

笔:=TDXDLL2(2,0,H,L);
段:=TDXDLL2(3,笔,H,L);

距离段起点周期:=BARSLAST(段=1000||段=-1000);
段周期:=REF(距离段起点周期,1)+1; 
末段尾点类型:=REF(段,距离段起点周期);
末段类型:=REF(末段尾点类型,1);{-1=上升,1=下降}
段MACD:=IF(末段类型=-1000 AND MACD>0,MACD,IF(末段类型=1000 AND MACD<0,MACD,0));
段面积:IF(末段类型=-1000 ,SUM(段MACD,段周期)/20,IF(末段类型=1000 ,SUM(段MACD,段周期)/20,0)),COLOR555555;

DRAWNUMBER(段=1000 ,段面积,段面积*20),COLORRED;
DRAWNUMBER(段=-1000 ,段面积 ,段面积*20),COLORGREEN;
DRAWNUMBER(ISLASTBAR AND 段面积>0,段面积 ,段面积*20),COLORRED;
DRAWNUMBER(ISLASTBAR AND 段面积<0 ,段面积 ,段面积*20),COLORGREEN;

#### 特色
解决走势延伸，背了又背等问题。同时为了自动化交易要把行情中的各种信号留痕包括假信号也保留以便盘后统计分析。

由于千人千缠，作者理解的缠论操作与其他人不太一样，但践行的是原缠，不作任何修改和扩展。
首先是定义，作者理解下分操作级别与观察级别，一般人方便拿到的数据最低就是1分钟K线，所以作者认为一般情况下可以稳定操作的级别是围绕1F中枢的走势类型，由于1F段走势容易被笔破坏，所以1F段做操作级别也不可靠，1F走势类型下仅作为观察级别。因此在一个闭环的买卖点循环里，只有1F走势类型产生标准背驰走势1买点时，也就是1F段是次级别走势类型，只能作为观察级别，1F走势类型递归上去是5F段，才是操作级别，在无法精确判断背驰时，只有5F下跌段中的1F段破坏之后的再次段回调才是可靠的买点。很多学缠的人包括我自己开始时搞不清这些都吃了大亏。为了走势的稳定，一切以线段走势为准，一切操作信号也以段信号为准。
在处理笔时，如果笔内K线有高低，会被视为笔内波动，高低点合并进笔处理。
在处理中枢时，缠文里提过 中枢与中枢之间可以是跳空连接（本项目未来再处理跳空）。
十多年的缠论实践，没有发财，但理解了禅师的很多理念，现在重新再出发，重新写一遍。

### 概念
K线信号分类：  笔顶、笔底、废笔顶、废笔底、K、笔顶分、笔底分、笔破坏、段顶分、段底分、废段顶、废段底、段破坏、顶分破坏、底分破坏、废除段信号、废除顶信号
### 样例图

![输入图片说明](https://foruda.gitee.com/images/1740223864137426611/caa93000_11322109.png "屏幕截图")
![输入图片说明](https://foruda.gitee.com/images/1740224005678728817/ce9dc5c8_11322109.png "屏幕截图")

### 交流
微信号 dukechen2010 
QQ号   47835265 
加友请备注来自：gitee

