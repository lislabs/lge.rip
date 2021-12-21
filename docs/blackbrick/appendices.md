---
title: 附录
---

##  【维护中】刷入模块变砖了还没TWRP该怎么办？

- 在备用机上安装Magisk Manager，无需Root

- 自定义更新通道：https://raw.githubusercontent.com/Cinhi/magisk-recovery/master/custom.json


> 这是来自@Cyanoxyen 大佬的Magisk core-only更新通道 可能需要魔法上网

- 参考上方【STEP 2】KDZ解包教程解包砖机的boot分区
- 将解包获得的boot\_a/b.img文件在备用机上使用以上更新通道打包
- 打包完成后将其刷入后开机，Magisk会自动开启核心模式，屏蔽所有模块，但依然有Root权限
- 直接打开Magisk Manager卸载模块，或者使用MT管理器，进入/data/adb/modules 删除引起变砖的模块
- 在Magisk Manager中重新安装Magisk卡刷包,即修复Magisk运行环境,使其他模块生效

> 已经打包完成的救砖boot.img：https://www.coolapk.com/feed/17710014?shareKey=MDIxN2JhYWQwNzM4NWVhZDc0Njk~&shareUid=1639436&shareFrom=com.coolapk.market\_10.1.2

## 💿 刷入TWRP

###  TWRP维护人员名单:

- Rogue-LG G8 ThinQ(US)
- ErickG-LG G8 ThinQ(KR)
- Mr.zhu-LG G8X ThinQ
- Mr.zhu-LG V50 ThinQ 5G(US)
- ErickG-LG V50 ThinQ 5G(KR)

> TWRP的安装会丢失所有数据，请提前做好备份

> 本篇教程只是提供规范的刷入步骤以及各种注意事项，避免可能踩的坑

> 由于一些特殊原因,G8在TWRP安装完成后Hand ID将会失效,TOF镜头的其他功能不受影响,V50暂未发现显性bug

###  通过Magisk刷入

- 下载并在Magsk Manager中刷入 [TWRP安装脚本 (opens new window)](https://wwx.lanzoui.com/b00zfs4ra "TWRP安装脚本")

- 在Magisk Manager中安装对应的TWRP刷入模块，记得先别重启

> 需区分机型和美(US)/韩(KR)版，Android Q/P通用

模块是一次性的，TWRP刷入成功后自动删除

- 在Magisk Manager中重新刷入
- [Magisk20.4卡刷包 (opens new window)](https://wwx.lanzoui.com/ibwhhfc "Magisk20.4卡刷包")

> 安装TWRP刷入模块会导致丢失ROOT，在开机状态下重新刷入Magisk完整包即可保留Root

###  验证TWRP是否安装成功

- 启用ADB命令行，输入

> 当然很多ROOT工具箱也提供一键重启至recovery的选项

- 等待手机重启至TWRP界面，相信大家都知道这玩意长啥样... 就不配图了。直接进系统的话请重刷TWRP
- 当前TWRP是没有触摸的，以后也不会有(雾，其实这和TWRP没关系)

###  使用硬格进入TWRP来使触摸可用

- 标准手法

- 长按电源键+音量↓键强制重启手机，在黑屏的一瞬间保持音量↓键按下状态，当屏幕亮起时松开电源键后马上按下，进入硬格模式

- 使用音量键选择Yes，按下电源键确认，重复2次，等待重启进入TWRP

- Sprint官解手法

- 按住电源键和音量↓不放，等待屏幕上出现Recovery Mode字样

- Sprint&Verzion版官解会进入特殊的CWM Recovery，使用音量键选择Factory Data Reset后按下电源键确认，等待重启进入TWRP

- 滑动，你会发现触摸可用了

> 注：**部分硬解机**或者使用LGUP写入过ftm分区的官解机可能无法进入硬格，因此需要刷入**原装**的ftm分区来还原硬格，这里提供镜像文件，解压后刷入ftm分区即可

- [ftm分区镜像备份 (opens new window)](https://wwx.lanzoui.com/ibwhuje "ftm分区镜像备份")

> 你的SN会被更改，可以在刷入前使用16进制编辑镜像文件把SN改成你自己的 应该只适用于美版G8，韩版和v50全版本都不行，所以硬解韩版和硬解v50需自行找原装的老哥要个备份

###  手动解密DATA

**目前该TWRP还不支持自动解密data，需要格式化data分区后才能正常使用**

- 这里先选择一下语言，然后点击Cancel(取消)

- 清除 - 格式化 DATA分区 - 输入yes - 手动格式化DATA，请做好数据备份

> 格式化完成后,可能无法访问内部存储,需要重启进入TWRP来重新挂载userdata,同理使用硬格手动进入TWRP,避免触摸不可用

- 下载并移动至手机内部存储,在安装界面刷入

- [Disable\_Dm-Verity (opens new window)](https://www.lanzoui.com/b00zfs7ta "Disable_Dm-Verity")

> 避免开机提示解密失败

- 你不会想看到这玩意的:(

- 至此，教程结束，TWRP已可正常使用
