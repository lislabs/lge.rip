---
title: LG855系列 - 快速黑砖手册
---

## ⚠️ 警告

本教程及作者的最终目的是让你的设备**黑砖**，这点是显而易见的。当你开始使用本教程的时候，我们的目的就达成了一致。如果你黑砖了，那恭喜你达成了目的。如果没有，也不要沮丧，毕竟好运终难常相随。**这也正是本教程标题的含义**

**简单来说就是如何快速变砖寄修的详细指南**

**本手册对可能造成的各种情况不负任何责任**

### 写在前面

- 由于之前全流程指北的模式导致很多小白无法从中找出自己所需的教程
  > ~（比如已多次出现已经Root的小白想要开启VoLTE又按照教程试图重新解锁BL）~
- 为了提高本手册的适用性，每一部分的教程都进行了独立,你可以根据自己的进度和需求来从中选择特定的教程
  > 比如想了解如何开启VoLTE的话可以直接跳到后面，无需把前面看完
-  各部分教程仍以正常刷机流程排序
  > ADB命令行可以使用KDZTools中的Command Prompt，也可以使用酷安@晨钟酱 的 [搞工具箱](https://www.coolapk.com/feed/28107922shareKey=MzM2ZjJiYTA5ZTUwNjBmNjYwNmQ "搞机工具箱")

### 备份一次，胜造七级浮屠

**请备份以下分区*- `efs` `fsg` `fsc` `ftm` `modemst1` `modemst2`这对非硬解机来说**非常非常重要！**

> 当然如果你不知道或者不能备份，请接着往下看
> 玩♂机♂人♂士，提前备份数据是很好的习惯

#### 有记录的致命突发情况

1. 9008时QFIL卡死
2. LGUP遇到有错误代码的报错(0×800002EC等等)

**注意：*- 9008永远是最终手段，LGUP产生的错误可以使用9008救回。但9008中遇到的突发状况通常都是致命(造成硬件损坏)的

### 📱 关于版本查询

### 工程模式入门

1. 打开拨号键盘
2. 输入`*#546368#*机型代码#`<br>
  **例如：*- `*#546368#*820#`
3. SVC Menu - MID Info

> 注意：这对Verizon和Sprint版(系统)不可用..所以你懂的

#### 机型代码对照表

- G8 - 820
- G8S - 810
- G8X - 850
- V50 - 500
- V50S - 510

~日版？docomo手机关我LG什么事（~

~如图是一台三码合一的硬解机,如果我再把IMPL改为1,那就可以对外声称这是原装北美公开版LG G8 (滑稽)~

### MID Info阅读理解：

NT Code/IMEI/SN/FTMModelName/CSN信息存储在ftm分区中，可以提取分区镜像使用16进制进行修改。硬解机可以直接在HiddenMenu中手动输入除SN外的所有信息 查询IMEI对于官解来说是较为可靠的，如果与HiddenMenu中信息有出入，请以查询结果为准。

### LGIMEI查询链接：[LGBBS](http://www.lgbbs.cc/lg_mobile-lg_mobile.html "LGBBS")

~哒哒看到了记得打钱~

> 注意：在fsg中还有一份相同的NT Code

#### IMPL

1为官解/无锁机型，0为硬解网络锁(换字库或SOC或搬套件)或展示机/工程机/大修机

> 清空modest1/2分区会导致IMPL显示为0，官解机型永久锁定，实际上IMPL仍然为1
> 该补充条目来自酷安@哒哒科技 大佬

#### NT Code

个人理解为ftm标记码，LG855的mbn加载与之有关。也可以通过修改它来获得不同版本ftm分区的特性，比如Verizon版的REC菜单，或者手动重启至fastboot

#### IMEI

国际移动设备识别码，通过它可以查询出完整的SN/FTMModelName/CSN信息，所以只要记住IMEI，就可以在刷机丢失信息后重新找回。

#### FTMModelName

版本标记，用以直接标记出本机版本 ~（可修改，懂自懂，图一乐，别找我）~

### LG855可黑砖机型

- Verizon，U.S. Cellular，Amazon，韩版(LG U+、SKT、KTF)，欧版，各公开版原生无锁。~有些进了运营商黑名单被锁的机器就另当别论了~
- ATT，Sprint，T-Mobile原生有锁，存在官解。ATT官解最多，Sprint少很多，T-Mobile基本没有

> 冷知识：QM地区的LG855机身左侧没有谷歌按键

## ⛏️ 没有人比我更懂KDZ

1.  KDZ是LG官方提供给不同型号不同版本LG手机的线刷包。通过解包KDZ，我们可以从中获得自己想要的分区镜像文件用以实现Root,关闭AVB验证,9008针对性救砖等等操作
2.  KDZ的a/b分区中的内容完全一致，无需纠结提取boot\_a还是boot\_b

### KDZ的解包

**举例：解包制作Magiskboot所需的boot\_a镜像文件**

- 使用@gress 大佬的 [EXE版KDZTools及配套教程](https://bbs.lge.fun/thread-69.htm "EXE版KDZTools及配套教程")
- 注意，上面这行命令包含了后面那个“.”，要是不小心少了的话会导致第一步就失败
- 从KDZ到DZ时，文件名也发生了改变，在输入下一行命令时请注意更改文件名

> 碎碎念一下，这里/前面的数字就是KDZ中该分区的代号，每个KDZ可能都有所不同，比如我们现在要提取图中kdz的boot分区，可以看到它的代号为47，那么我们只要将gress大佬的最后一行命令当中的24改成47即可提取出所需的boot\_a.image文件。67位置的是boot\_b，随便提取一个就行

- 如果你没看懂，也没有关系

> 使用教程中的一键解包脚本，你需要等待更长的时间和提供更大的磁盘空间

### 使用Magisk打包boot镜像

- 提取结束之后，将文件传输到手机端
- 安装 [Magisk Manager](http://www.coolapk.com/apk/com.topjohnwu.magisk "Magisk Manager")
- 点击安装，选择并进行修补
- 修补完成后，将修补好的img文件传回电脑，等待下一步操作
> 无法修补的可以使用该更新通道 https://gitee.com/mintimate/magick\_custom\_update\_source/raw/master/v20/Magisk\_204.json
- [LG学院](http://lgkdz.xyz/ "LG学院")
> 这里贴出已经制作完成的Magiskboot，可以直接使用(滑稽)

### KDZ版本区分

**有锁机能秒我？你要是让我用不了卡，我当场 把这个 ATT 硬解**

**硬解机随便刷机啊官解，官解你快点啊官解，还在找自己的KDZ呢，公开版你都刷不了吗官解**

**你可能不知道美国最大运营商的机器没有KDZ是什么概念，我们通常用两个字来形容这种机器，孤儿。**

- G820N 韩版(OPEN/SK/KT/LGU\_KR)

> 享有最快的更新速度和一些独占功能,单从系统层面来讲非常优秀

- G820QM 北美公开版/亚马逊定制版(NAO\_US/AMZ\_US)

~QM20a，狗都不用~

> 和韩版几乎相同的更新速度,但并没有什么独占的功能,还经常会有奇奇怪怪的bug,每一次大更结束后安全补丁还会落后UM和N一个版本???

- G820UM 加拿大公开版(OPEN\_CA)、Verizon版(VZW\_US)、其他版本

> UM包含了很多版本,但能让你下载到KDZ的就只有加拿大公开版以及USC/Verizon两个运营商定制版本

> 加拿大公开版更新频率很低,相当于把QM好几个月的东西堆在一起发布,bug也很少，你可以把它理解为稳定版

> USC/Verzion版没有刷的必要，就不细讲了

- G820TM T-Mobile版(TMO\_US)

> TMobile的团队个个都是人才...活又好，提前几个月就把其他版本上普遍存在的掉帧问题解决了，还老喜欢在系统里加各种奇奇怪怪的东西,经常会把搞机玩家整的一脸懵逼。该版本更新频率和UM几乎相同

### 一些KDZ下载站：

- [LG-Firmwares](https://lg-firmwares.com/ "LG-Firmwars")

> LG FANS俱乐部，限速限量下载很淦，好在KDZ很齐全，其中谷歌验证需要科学上网

- [LG学院](http://lgkdz.xyz/ "LG学院")

> 目前体验最好的国内下载站

- [LG Info](https://www.gresslg.tk/ "LG Info")

> 最新资讯&冷门KDZ&资源站 仅支持中国大陆IP访问

## 👨‍💻 LGUP的使用

1.  为了避免各种无法识别/连接失败等玄学问题，请在**刷机前**将手机重启到DOWNLOAD模式，用**可靠**的数据线插在机箱后端口

2.  请**卸载**所有旧版本LGUP和DLL后再安装本教程内的LGUP，特别注意1.14等**旧版本**是不可用的！

### LGUP1.16.0.3的安装与破解

**LG UP即LG官方的线刷工具，对其进行破解以解锁完整功能**

- 下载 [所需文件](https://wwx.lanzoui.com/b00zaimde)

- 按顺序，把三个zip文件解压

- 先安装Lab

- 再安装DLL

- 最后将UI\_Config.lgl放在C:\\Program Files (x86)\\LG Electronics\\LGUP\\model\\Common目录下。右键该文件属性，勾选只读并应用。

- 安装LG Mobile驱动

> 直接运行安装就完事

### LGUP标准刷机流程

#### 重启到DOWNLOAD模式

- 将手机关机，按住音量↑键的同时插入数据线即可

**如果你已经变砖**

- 请先用数据线将手机连接至电脑
- 然后长按电源键+音量下键强制重启
- 在屏幕关闭的瞬间松开所有按键并按住音量上键
- 等待手机进入DOWNLOAD模式

#### 使用LGUP进行刷机操作

> 每次打开都会有上面这个警告，问题不大

- 将KDZ拖入File Type下面的框里

- 选择所需的模式点击Start进行刷机

> 有时候损坏的KDZ并不会被LGUP成功检测，建议在使用刚下载的KDZ时先用Partition DL模式进行测试，如果能出现复选框的话，就没有问题。若4秒钟后直接显示complete，则KDZ损坏或。切勿使用其他模式进行刷机，会导致直接黑砖。

### 常用刷机模式介绍

**注意：非硬解机型需使用与自己运营商版本对应的KDZ，否则无法刷入**

- REFURBISH模式：默认的刷机模式，正常情况下用它来进行混刷/切换版本/普通救砖，会清除数据

- UPGRADE模式：升级模式，只适用于同地区的KDZ间的保留数据升级,ROOT和VoLTE无法保留

- ChipErase模式：全清模式，全量刷入kdz中的所有分区，并清空部分其他分区(不包括frp)，可以解决99%的软件问题，会丢失IMEI/SN/CSN信息以及造成BootLoader回锁

- PARTITION DL模式：好用极了，可以指定KDZ中的几个分区进行刷入，可用于进入Fastboot/还原特定的分区/保留Root和数据升级系统


> ftm分区中存储有IMEI/SN/CSN等手机识别信息，在使用PD模式全选刷入时，记得取消勾选此分区来避免丢失信息，否则ftm将被写空

### 硬解机使用同型号任意KDZ混刷注意事项

**官解请出门右转→**

- 韩版和美版V50基带不通用，混刷会导致无信号/WIFI，需要提前备份modem\_a/b和ftm分区并还原，Verizon版演示机同理

- [V50 Sprint版 modem镜像](https://wwx.lanzoui.com/ir4R2lncggd "V50 Sprint版 modem镜像")

- [G8 Verizon展示机 modem镜像](https://wwx.lanzoui.com/iwD2ylmdolg "G8 Verizon展示机 modem镜像")

- 美版G8混刷韩版相机FC修复教程：

1. 解除AVB验证

> 参考结尾处VoLTE教程中的解除方法

2. 打开 /system/build.prop 或 /product/OP/cust.prop

> 二选一

3. 在你选择的文件中修改或添加以下内容后保存并重启

#### 【附加】快捷的保留ROOT升级

##### 线刷方法，适用于有KDZ的官解、无锁机型

- 把**将要升级到**的版本的 Magiskboot **先行**刷入boot\_a/b分区

> 你可以使用dd命令，各种ROOT工具箱，或者通过9008，~亦或者先用LGUP进入Fastboot来刷入它~

- 使用LGUP的PARTITION DL模式全选，并取消勾选boot\_a，boot\_b和ftm，点击OK进行刷入，即可保留ROOT升级

> 这只是一个快捷方式,免去了进FastBoot进行ROOT的步骤,所以可以少下载一个Android P的KDZ来节省时间和空间,和先刷机后ROOT在结果上无任何区别

##### 在OTA升级时通过验证并保留Root

我想没人会用到这奇怪的教程

- 确认你真地能够收到OTA

> 事实上真正需要这份教程的Sprint官解大多都已被商家混刷，无法接收OTA了

- 准备好你的**原**boot备份

> 例如Spirit G8 UM21f boot

- 将备份**同时**刷入boot\_a/b分区

> OTA时会对各分区进行校验，你需要对各分区的内容有绝对的把握

- 在系统更新中安装OTA更新，安装完成后**不要**重启

> Root过后boot分区无法通过校验，还原之后即可

- 打开Magisk，使用安装选项，选择**安装到另一插槽**

> 如果你的安装里没有此选项,则需手动提取boot\_a/b后用Magisk打包后刷回去

- 完成后重启

> 插槽会被自动切换。因为Magisk已提前对该插槽进行写入，开机即有ROOT

## 🔓 终末之路 - 9008

- 这里是终点，这里是起点
- 这里曾经遥不可及，而现在却唾手可得
- 这里是一切的捷径，不论你将前往何处
- 虽然我们曾经背离，但是探索永无止境

> [【纪念】临时ROOT解锁BootLoader教程](https://bbs.lge.fun/thread-155.htm "【临时ROOT解锁BootLoader教程】")

### QFIL - 潘多拉的魔盒

- 如果你的目的是解锁BootLoader，请在手机开发者选项中打开oem解锁
- 解锁BootLoader将清除所有个人数据，请提前做好备份工作
- 相对上面的教程，本教程操作简单，成功率高，但在9008出现的不规范操作几乎都是致命的(造成硬件损坏的)

#### 比如：

**光速拔线炸字库 一键Erase出奇迹**


#### 下载所需文件

- [QPST & LG855 Firehose](https://wwx.lanzoui.com/b00zz0iod "QPST & LG855 Firehose")

#### 安装刷机环境

- 解压QPST软件和驱动，打开Drive文件夹，安装Qualcomm USB Driver V1.0.exe

- 运行QPST.2.7.496.exe，一路Next即可

#### 进入9008模式

- 安装完毕后，打开QFIL，USB连接电脑，按住音量下加电源键重启，黑屏后不要松开音量下和电源键，狂敲音量上，观察QFIL窗口有无出现新设备

> 亮屏即为失败，多试几次总能进去 找不到QFIL的请去开始菜单里面搜索

#### QFIL的设置

- 等待连接成功后，上方No Port Available会显示出设备

> 有时会显示Please select a port，此时点击右上角的Select a port选择你的设备即可

- 选择Flat Build

- Select Programmer里选择下载好的LGE855 Firehose

- Configuration-FireHose Configuration-Device Type-ufs-OK

> Device Type默认是emmc

- Tools-Partiton Manager-OK

注意：每次引导失败后都必须重新进入一次9008，否则会一直卡住

### 解锁BootLoader

#### 准备所需文件

- G8 [工程机ABL以及配套引导](https://wwx.lanzoui.com/iu8Hnfuwe3e "工程机ABL以及配套引导")

- V50 [工程机ABL以及配套引导](https://wwx.lanzoui.com/iXqHYfuwjfg "工程机ABL以及配套引导")

- G8X&V50S [工程机ABL以及配套引导](https://wwx.lanzoui.com/b01015vfe "工程机ABL以及配套引导")

> [G8X&V50S解锁后指纹修复教程](https://www.coolapk.com/feed/26578393?shareKey=YmYzNzU2NmQzYWZlNjBiY2M4MTI "G8X&V50S解锁后指纹修复教程")


#### 刷入工程机三件套

- 列表里仔细找到abl\_a，先**左键**选中

- 右键-Partiton Manager Data-Load Image

- 选择V500ES\_abl\_a.image，打开

- 按下Raw Data Manager中右下方的Close

- 同理再把abl\_b分区刷入V500ES\_abl\_a.image

- xbl\_a和xbl\_b分区刷入xbl\_a.image

- xbl\_config\_a和xbl\_config\_b分区刷入xbl\_config\_a.image

- 全部完成后点击Close，等待下方Status中Finish Reset To EDL出现

- 按住音量下和电源键，尝试重启进入工程机Fastboot

> 在Android P上可能会正常开机，手动关机后按住音量下键插入数据线即可


**日后搞机把哪个分区刷炸了，也可以强制进9008按上面刷分区的方法刷回原Kdz中的分区**

#### 解锁BootLoader


Verizon运营商版本的特殊步骤 还在为设置中找不到OEM解锁而烦恼吗，别担心，由Kamio大佬倾情提取的frp分区，为您的黑砖生活保驾护航 \> 其他版本请忽略，但如果你忘了打开OEM解锁，这就是补救方法。除G8以外的机型请勿刷入

- 下载 [frp备份](https://www.lanzoui.com/ictudih "frp备份")

- 解压文件，并将其移动到ADB工具根目录，输入以下命令


- 继续执行下方步骤


- 使用**音量键**选择Restart bootloader，按下**电源键**来**重新**进入FastBoot

> ~你问我为什么？什么叫经验啊(战术后仰)~

> 原理未知的操作，若不执行，将无法成功解锁BL

- 在ADB命令行中输入


- 然后你会看到以下界面

> Not allowed？之前让你开OEM解锁干什么去了

- 使用音量键选择UNLOCK THE BOOTLOADER，按下电源键确认，手机会自动重启并解锁BL。如果成功，那它将是这样的

> 若失败，你自己好好想想之前是不是让你先重启一次


### 退出Fastboot

> Android Q在工程机FastBoot下无法开机，需要还原原来的分区

#### 恢复原FastBoot

**这里仅提供Android Q的，Android P请自行从KDZ中解包**

- 下载G8 [原FastBoot备份](https://wwx.lanzoui.com/icqnuwd "原FastBoot备份")

- 下载V50 [原FastBoot备份](https://wwx.lanzoui.com/icqo1jc "原FastBoot备份")


> 请解压后再使用

- 将文件移动到[ADB工具](https://www.coolapk.com/feed/24703263?shareKey=NzFmZmJiZDQ0OWE3NjAyMmJlMDg "ADB工具")根目录，并在ADB命令行中输入以下命令：


**别复制错了/少了，一共要将三个镜像刷入6个不同的分区，请认真检查**

**然后大佬们就可以直接滚回9008备份devinfo分区，在下次BootLoader意外回锁时重新刷入来直接解锁本机**


#### ROOT

1.  此步紧接上一步教程，意在节省时间一步到位。
2.  如果你已退出工程Fastboot，或是刷机后丢失ROOT，请使用下方教程
3.  无需重做解锁BL步骤承担不必要风险

##### 确认手机状态

- 确认手机处于工程机Fastboot中
- 确认DEVICE STATE - unlock

> 即确认BL已解锁

- 往下看，跳过“进入FastBoot”，直接执行ADB命令，你现在仍处于Fastboot中！

## #️⃣ Root

- 准备好合适你KDZ版本的Magiskboot

> 显然，我们在“没有人比我更懂KDZ”章节中学习了如何制作MagiskBoot。如果你完全没明白，为什么不去问问神奇的 [LG学院](http://lgkdz.xyz/ "LG学院") 呢


**警告：韩版不同运营商的同版本Magiskboot不可混用！**

> 比如KT20z和SKT20z，除非你想让你的手机变成聋哑人

- 将Magiskboot文件重命名为magisk\_patched.img并移动至ADB根目录

> 谨防新型隐藏后缀诈骗


### 进入FastBoot

**解锁BootLoader后，进入Fastboot可以通过使用LGUP的PD模式刷入安卓版本不对应的KDZ中的boot\_a/b分区来实现(G8X和V50S不适用)**

> 比如Android Q 就刷 P 的boot\_a到boot\_a和boot\_b，Android P反之

#### 通过LGUP的PD模式进入FastBoot

- 准备Android P的KDZ，使用LGUP的PD模式，选择并刷入KDZ中的boot\_a/b分区

- 等待手机重启到FastBoot模式，成功后手机屏幕的左上角会显示一个绿色的START

- 在Fastboot中使用ADB命令刷回原系统的magisk\_patched.img重启即可退出


#### 通过ROOT进入FastBoot模式

- 使用超级终端/某些工具箱，给予Root权限后将安卓版本不对应的boot.image刷入boot\_a/b分区

> 如果刷完显示红色感叹号，是因为BootLoader没有解锁。等待十几秒钟，手机会自动关机，按住音量↑并插入数据线进入DOWNLOAD模式，使用LGUP还原原系统的boot\_a/b分区即可正常开机

#### G8X&V50S进入FastBoot

boot爆炸法：

- 9008下使用QFIL erase boot\_a/b分区后重启

laf升天法：

- 9008下使用QFIL erase laf\_a/b分区后使用进入DOWNLOAD模式的方法进入FastBoot

> 然后你将无法使用LGUP进行刷机操作，所以我建议erase之前备份一下laf，虽然这不是必须的


#### 刷入MagiskBoot

- 在ADB命令行中输入以下指令

- ###### 最后输入fastboot reboot重启


### 修复Magisk运行环境

> 如果你刷入模块报错,很大概率是因为没有修复运行环境

- 先安装在手机上Magisk Manager，打开可能会提示修复运行环境，点击取消，将Magisk20.4卡刷包作为模块刷入后重启即可
- [Magisk卡刷包](https://wwx.lanzoui.com/icvoopg "Magisk20.4卡刷包")

### 【附加】从莫名其妙的FastBoot中退出

我不太确定你为什么被炸进FastBoot，但通常是更新Magisk或者修复Magisk运行环境导致的

- 从KDZ或者隔壁插槽中获取正常的boot/dtbo镜像

- 执行以下命令：

## 📞 开启Volte

手动开启VoLTE

#### 【TMoblie】开启没有开关的两网VoLTE

> 这只适用于TMobile系统,不需要Root,只能电信/联通VoLTE,其中VoLTE开关问题还是得不到解决

**★联通用户**

**★电信用户**

###### 手动添加APN

- 打开设置-网络和互联网-移动网络-接入点名称，点击右上角-添加APN。

- 其他的无需修改，点击右上角-保存

###### 手动开启VoLTE

- 打开拨号键盘输入`*#546368#*820#`，Field Test - IMS Setting - Slot-0:TMO/US - SIP - Target Number Format选择Local Number

> 进不去的话请提前移除SIM卡

- 回Field Test,拉到最下面选择Universal Enabler - Create New Config然后如下图设置  完成后点击CONFIRN

- 回到拨号界面，输入`*#*#4636#*#*`(这里可能会卡顿/没有及时弹出菜单，等一会或者多试几次即可)。然后选择手机信息，改成下图所示

- 完事了，重启喜提VoLTE

#### VoLTE模块

- 作者酷安@漠云,脚本由@KamioRinn提供帮助,Verizon版VoLTE文件由@上帝之眼提供,仅支持LG UX系统
- HiddenMenu.apk由酷安@诺贝尔螺旋酱 修改并提供
- 仅支持Android Q
- apns.xml来自MIUI12
- 支持语音清晰
- 支持通话录音
- 支持韩版全版本,加拿大公开版,北美公开版,Verizon,TMobile,Sprint

###### 已测试的系统版本：

- QM：20b,20e,20f
- UM：20d,20e,20h,20i,20j,VZW20d,VZW20e,VZW20g
- TM：20d,20e,20j
- KR：N20b,N20f,N20m,N20o,N20t,N20w,N20s,N20z,N21a

> 以上KR包含韩版各运营商版本及公开版 并不是所有版本都能在设置中找到VoLTE开关，有无开关本身与VoLTE是否生效并无直接关系

##### 关闭AVB2.0校验

> 若刷入与当前版本不对应的vbmeta.img,会导致开机卡安全验证损失所有数据，但不影响最终效果

- 从当前系统KDZ中解包出vbmeta.img或者从当前系统中直接提取vbmeta.img

> [各版本vbmeta镜像收录](https://wwx.lanzoui.com/b00zqs7uh)

###### 其实只有加版的，只要我够懒，就能逼的你们全刷最新UM

- 进入FastBoot

- 把vbmeta.img拽到ADB工具根目录

- 在ADB命令行里输入

> 注意，以下命令需要最新的ADB环境，否则会提示unknow命令,建议使用 酷安@晨钟酱 的 [搞机工具箱](https://www.coolapk.com/feed/19905756?shareKey=N2RkMGY0OTMxNDFkNWYxMzA0MjE~&shareUid=1639436&shareFrom=com.coolapk.market_10.4)

> 这步用于还原boot

- 最后输入fastboot reboot重启即可

> 若开机卡在安全启动,可以连输30次密码然后触发硬格来进入系统。。

- 下载并刷入 [VolteEnabler脚本模块](https://wwx.lanzoui.com/b00zix47i "VolteEnabler脚本模块")

### 【VoLTE玄学问题处理】为什么照做了还是无法开启VoLTE？

查看`*#- #4636#*#*` - 手机信息 - VoLTE Provisioned是否开启

- 若开启：没什么大问题

- 设置-网络和互联网-移动网络-接入点名称-右上角还原默认设置
- 尝试在设置中切换一下网络制式
- 检查SIM卡是否开启了VoLTE服务

> 某些老旧的SIM卡和特殊的地区无法开启VoLTE,可以尝试补卡

- 甚至可能啥都不用干，等几十秒就好了
- 若关闭：
- - 打开拨号键盘,输入`*#546368#*820/500#` - FiedTest - Universal Enabler - Create New Config检查配置文件是否生效(打勾)
- 检查AVB验证是否已解除

#### ★花絮

- _operator="TMO" country="US"组合可以在各种版本的kdz上大幅增强信号(UI,仅适用于电信用户，联通在其他版本的kdz上并未测试，移动完全不适用)电信没有VoLTE开关_
- _没有VoLTE开关的版本可以在工程模式中开出一个重启就失效的临时开关FieldTest-GPRI VoLTE/VoWIFI-UX-display\_volte\_setting_
- custom\_apk\_list.cfg 可根据个人需要修改，我的是这样的

> ###### 美公开/加公开原内置用的是谷歌信息，我还是喜欢LG画风，就改回LG信息了
>
> ###### 我这里倒数的三个不能改，其他随意，不影响

- +com.lge.intelligent:/product/priv-app/SmartMessagingEngine
- +com.android.mms:/product/priv-app/LGMessage
- +com.lge.gba.android:/product/priv-app/GBAService
- +com.lge.imsvt:/product/priv-app/ImsVT
- +com.lge.ims:/product/priv-app/Ims6

