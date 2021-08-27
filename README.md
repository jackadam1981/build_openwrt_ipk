# build_openwrt_ipk
随便记  
openwrt官方已经升级到21.02.rc4了，可我们的几位大佬还坚守在19.07.  
那么我要做什么呢？刷官方的包，再装中国特色插件。opkg安装。  
适合的包可不多啊  


|包名                   | 软件名         | git地址                                               | 备注    |  
|------               |------     | ------                                              |------  |  
|luci-app-ssr-plus     |helloworld      |https://github.com/fw876/helloworld.git            | ![更新图标1](https://img.shields.io/github/last-commit/fw876/helloworld )          |  
|luci-app-vssr         |HelloWorld     |https://github.com/jerrykuku/luci-app-vssr.git      | ![更新图标2](https://img.shields.io/github/last-commit/jerrykuku/luci-app-vssr )  |
|luci-app-passwall| openwrt-passwall|https://github.com/xiaorouji/openwrt-passwall.git    | ![更新图标2](https://img.shields.io/github/last-commit/xiaorouji/openwrt-passwall )  |
|luci-app-openclash    |OpenClash      |https://github.com/vernesong/OpenClash.git           | ![更新图标3](https://img.shields.io/github/last-commit/vernesong/OpenClash )      |  
|luci-app-bypass    |bypass      |https://github.com/kiddin9/openwrt-bypass.git           | ![更新图标3](https://img.shields.io/github/last-commit/kiddin9/openwrt-bypass )  |

还重名了。包名不一样，应该可以区分。

自动编译介绍：   
|作者|项目地址|最后更新|介绍|  
|---|---|---|---|
|MrH723|https://github.com/MrH723/Actions-OpenWrt| ![更新图标3](https://img.shields.io/github/last-commit/MrH723/Actions-OpenWrt)|适配的很多|
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
           
