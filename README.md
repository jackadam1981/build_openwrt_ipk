# build_openwrt_ipk
随便记  
openwrt官方已经升级到21.02.rc4了，可我们的几位大佬还坚守在19.07.  
那么我要做什么呢？刷官方的包，再装中国特色插件。opkg安装。  
适合的包可不多啊  


|包名|LUCI服务名|git地址|更新|备注|
|------|------|------|------|------|
|luci-app-ssr-plus|ShadowSocksRPlus+|https://github.com/fw876/helloworld.git|![更新图标1](https://img.shields.io/github/last-commit/fw876/helloworld)|历史悠久的ssrp+|
|luci-app-vssr|HelloWorld|https://github.com/jerrykuku/luci-app-vssr.git|![更新图标2](https://img.shields.io/github/last-commit/jerrykuku/luci-app-vssr)|基于SSRP+,增加了一些方便的服务状态，目的地可访问状态。支持七组分流，可以给七组分配不同的服务器|
|luci-app-passwall|PassWall|https://github.com/xiaorouji/openwrt-passwall.git|![更新图标2](https://img.shields.io/github/last-commit/xiaorouji/openwrt-passwall)||
|luci-app-bypass|bypass|https://github.com/kiddin9/openwrt-bypass.git|![更新图标3](https://img.shields.io/github/last-commit/kiddin9/openwrt-bypass)||
|luci-app-openclash|OpenClash|https://github.com/vernesong/OpenClash.git|![更新图标3](https://img.shields.io/github/last-commit/vernesong/OpenClash)||


还重名了。包名不一样，应该可以区分。

自动编译介绍：   
|作者|项目地址|最后更新|介绍|  
|---|---|---|---|
|P3TERX|https://github.com/P3TERX/Actions-OpenWrt| ![更新图标3](https://img.shields.io/github/last-commit/P3TERX/Actions-OpenWrt)|git actions 鼻祖，有中文说明|
|MrH723|https://github.com/MrH723/Actions-OpenWrt| ![更新图标3](https://img.shields.io/github/last-commit/MrH723/Actions-OpenWrt)|适配的很多|
|IvanSolis1989|https://github.com/IvanSolis1989/OpenWrt-DIY|![更新图标3](https://img.shields.io/github/last-commit/IvanSolis1989/OpenWrt-DIY)|适配的很多|
|kiddin9|https://github.com/kiddin9/OpenWrt_x86-r2s-r4s|![更新图标3](https://img.shields.io/github/last-commit/kiddin9/OpenWrt_x86-r2s-r4s)|适配的不多|
|1orz|https://github.com/1orz/My-action|![更新图标3](https://img.shields.io/github/last-commit/1orz/My-action)|适配不多|
|ylqjgm|https://github.com/ylqjgm/OpenWrt-Actions|![更新图标3](https://img.shields.io/github/last-commit/ylqjgm/OpenWrt-Actions)|K2P N1|


更新一些可以下载包的地址：  
http://repo.elecity.top:8880/openwrt/  
https://op.supes.top/  
http://openwrt-dist.sourceforge.net/  
https://down.cloudorz.com/  

改变内存和闪存大小  
关于设备内存和闪存的配置，一般位于源码target/linux/ramips/dts/mt7621_phicomm_k2p.dts（K2P示例）  
编辑DTS文件  
内存：  
```
memory@0{
	device_type = "memory";
	reg= <0x0 0x8000000>;
};
```

reg = <0x0 0x10000000>; // 256MB RAM  
reg = <0x0 0x8000000>; // 128MB RAM  
reg = <0x0 0x4000000>; // 64MB RAM  

闪存FLASH        
```
partition@5000{
	label = "firmware";
	reg = <0x50000 0xfb0000> }
;
```

reg = <0x50000 0x7b0000>; // 8MB flash  
reg = <0x50000 0xfb0000>; // 16MB RAM  
reg = <0x50000 0x1fb0000>; // 32MB RAM  
           
介绍一下openwrt 源码补丁的概念  
像上面改FLASH大小的工作，不能每次都去改啊。那么就发明了补丁，补丁在，执行命令，可以实现自动修改相关源码。  
手边只有一个K2是修改过FLASH的，8M改了16M。   
那么需要修改的配置文件是：/openwrt/target/linux/ramips/dts/mt7620a_phicomm_psg1218b.dts

创建补丁：   
将1218b.dts复制一份重命名为1218c.dts，修改
```
将   reg = <0x50000 0x7b0000>;
改为 reg = <0x50000 0xfb0000>; 
````
在openwrt目录执行：  
```
#diff  老文件 新文件 -u > 补丁文件名
diff target/linux/ramips/dts/mt7620a_phicomm_psg1218b.dts target/linux/ramips/dts/mt7620a_phicomm_psg1218c.dts -u >k2_16m.patch
```
既可生成补丁，但是生成的补丁还需要修改一下：
自动生成：
```
--- target/linux/ramips/dts/mt7620a_phicomm_psg1218b.dts	2021-08-28 19:12:54.000000000 +0800
+++ target/linux/ramips/dts/mt7620a_phicomm_psg1218c.dts	2021-08-28 17:12:41.605830913 +0800
@@ -9,6 +9,6 @@
 	partition@50000 {
 		compatible = "denx,uimage";
 		label = "firmware";
-		reg = <0x50000 0x7b0000>; 
+		reg = <0x50000 0xfb0000>; 
 	};
 };

```
修改为：
```
--- target/linux/ramips/dts/mt7620a_phicomm_psg1218b.dts
+++ target/linux/ramips/dts/mt7620a_phicomm_psg1218b.dt
@@ -9,6 +9,6 @@
 	partition@50000 {
 		compatible = "denx,uimage";
 		label = "firmware";
-		reg = <0x50000 0x7b0000>; 
+		reg = <0x50000 0xfb0000>; 
 	};
 };

```
主要是修改+++那行的文件名，删除多余的时间标记。

打补丁：  
