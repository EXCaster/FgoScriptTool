## fgo小工具使用教程

本软件完全开源，使用本软件造成后果由使用者本人承担。

### 第一步：下载mumu模拟器和fgo

如果用的不是mumu，在config.py里面可以修改fgo程序名。需要使用带键位映射的模拟器。

华为的fgo需要先下载华为应用市场的apk，再在应用市场里下载fgo。

### 第二步：在mumu模拟器上设置键位

只要自己打一把就可以设置好。下面是用到的键位的说明：

1：先点进去一个本，再点返回，刚才点进去的本自动刷到的地方（也就是你打完一个本之后，再点进去要点的地方）

2、3、4：敌方三个人的头像

W、E、R：三张宝具卡

A、S、D、F、G：五张指令卡

Z、X、C、V、B、N、M、逗号、句号：从左到右的9个技能

]：御主技能按钮

O、P、[：从左到右的3个御主技能

空格：作战编制界面的进本按钮

回车：战斗界面的Attack（进入选卡界面）按钮

J、K、L：使用需要选择目标的技能（如狐嫁女）的时候，从左到右3个目标

5、6、7、8、9、0：换人界面的6个目标

-：换人界面的确定按钮

### 第三步：安装Python 3（不要装成2）

### 第四步：安装Python包

首先需要在https://www.lfd.uci.edu/~gohlke/pythonlibs/#pyhook 处下载PyHook，这玩意不能用pip直接安装。

在cmd里输入命令：`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple PyUserInput`，以及`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pywinauto`，安装需要使用的库

### 第五步：修改config.py

我们的小工具包括4个Python文件，其中auto.py和enter.py不必修改。

config.py里规定了一些公共参数：

如果用的是mumu，第一个变量“FGO窗口名”不用改。

接下来是操作时间（单位：秒），其中前4个时间和网速有关，可以试一局之后掐秒表根据情况调整。换面时间是不包括宝具动画的，请务必注意换面时间要预留触发chain时额外的1.5秒左右的时间。

“操作完后回到原来窗口”这个选项你可以设为False（需要保持FGO窗口最前）或True（不需要保持FGO窗口最前，比如你原来打开着VS，每次脚本点的时候是点一下FGO窗口然后闪回VS，但是有很多问题，比如你本来在VS里打字，突然切到FGO窗口了，你打的字会被映射到FGO里造成脚本失常，总之这个选项打开是有风险的，但是如果你在看不进行交互的窗口比如直播网页，这个选项是没有关系的）

“宝具时间”是我自己查表得到的，基本包括了所有常用的非拐从者，需要增加的话自己测或查表，向上取整。

### 第六步：写脚本

play.py里写的是脚本，我已经让脚本非常接近自然语言。

我们用刷尼禄祭初赛王者级的脚本举例。

```python
def 脚本():
    设置阵容([
        '大英雄',
        '肯娘',
        '孔明1',
        '小太阳',
        '孔明2',
        '占位从者'
    ])
    使用技能('孔明1', 3)
    使用技能('孔明1', 2)
    使用技能('孔明1', 1, 目标='肯娘')
    宝具('大英雄')
    使用技能('咕哒', 3)
    换人('孔明1', '孔明2')
    使用技能('孔明2', 3)
    使用技能('孔明2', 2)
    使用技能('孔明2', 1, 目标='肯娘')
    使用技能('小太阳', 2)
    宝具('小太阳')
    等待(10.0)
    使用技能('肯娘', 2)
    使用技能('肯娘', 3)
    宝具('肯娘')
```

我们写的脚本是函数“脚本”里的内容。

进本、出本之类的内容不用你写，你的第一句话是设置阵容（一定要设置满6个人！！！！！），格式如上，需要保证每个从者的名字不一样，并且会放宝具的从者名必须在“宝具时间”的列表里（否则宝具时间按18秒算），比如“棉被”在列表里而“呆毛”不在，所以阵容里写“棉被”是正常的，但写“呆毛”我会认为你的呆毛需要放18秒宝具。

接下来是你的操作，这个工具支持3种操作：

* 使用技能，第一个参数为使用者（要在阵容里，或者为“咕哒”表示御主技能），第二参数为这是几技能（1~3），后面可以跟两种补充参数。
  * 目标=xxx，需要选择目标的技能（如狐嫁女）选择的目标（写名字）
  * 敌方目标=x，需要选择敌方目标的技能（如咒术）选择的目标（1~3，从左到右）
* 宝具，只有一个参数，使用者。可以跟两种补充参数。
  * 敌方目标=x，单体宝具需要更换目标时指定。
  * 手动选卡=True，默认是False，设成True的话宝具后补刀的两张卡会等玩家手选，手选完以后在cmd里敲回车让脚本继续。
* 换人：使用换人技能时，因为换人有两个目标，所以特殊处理了，格式如上。

另外意料之外的等待，可以用“等待”函数加秒数完成。这个本里海妈和老福有特写，所以需要写等待。

### 第七步：运行脚本

cmd运行`python enter.py`就可以运行脚本。

每次脚本的第一步是点入本，最后一步是点副本名，也就是说，如果不需要吃苹果，每次只需要点一下选助战就可以。吃苹果并选完助战以后，在cmd里敲个回车，下一次脚本开始运作。

