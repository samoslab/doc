**目录**



**编译原始镜像**
1.从github上[https://github.com/garywangcn/bpi-r2_lede](https://github.com/garywangcn/bpi-r2_lede)下载源码
编译环境:Ubuntu 14.04.5 LTS
2.安装一下插件
apt-get install -y subversion build-essential g++ libncurses5-dev zlib1g-dev bison flex unzip autoconf gawk make gettext gcc binutils patch bzip2 libz-dev git asciidoc
3.在bpi的根目录下执行如下命令
./scripts/feeds update -a
./scripts/feeds install -a
4.按需要选择编译的插件
make menuconfig
进入如下画面
![图片](https://images-cdn.shimo.im/mWYBCTaATHY4Ob7g/image.undefined!thumbnail)
芯片选择MediaTek Ralink ARM
Boot Loaders选择 u-boot-bpi_r2

5.编译内核
make -j1 V=s
参数说明 j 是指定参与编译的CPU数,出现错误时候建议用j1来调试内核.V指定生成后的镜像安装位置s是安装在SDCARD,还可以选择安装在emmc或sata硬盘上.

6.获取生成镜像
镜像可以选择有两种类型的安装方式一种是卡刷方式,另一种是在线升级方式.
分别可以在如下目录获取到他们的安装包:
任何时候网页升级模式下失败都可以用卡刷方式恢复系统.用户可以放心使用.

1.编译后在build_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/linux-mediatek_32 目录下生成镜像文件.
mtk-bpi-r2-SD.img (对应的是tf卡镜像)
mtk-bpi-r2-EMMC.img (对应的是emmc镜像)
2.网页升级文件:
在bin/targets/mediatek/32目录下的
XXX-mediatek-32-bananapi,bpi-r2-sysupgrade.tar (tf卡的网页升级文件)
XXX--mediatek-32-mediatek,mt7623-rfb-emmc-sysupgrade.tar (emmc的网页升级文件)

**编译配置参考样例**
na->* 代表选项中没有的就需要打开 ,(取消) 代表该项不能打开,M代表可以生成插件包后安装
Target Images
  ext4  (取消)
     Create a journaling filesystem (取消)
  Root filesystem partition size(in MB) 256->4096 (取消)
   
Global buildsetting
  perferred standard c++ 
     library.uclibc++ -> libstdc++(取消)
Advanced configuration options 
  Toolchain options
  Target Options na->*
     Use software floating point by default na->* 
Image configuration na->*
  修改了两个版本名字  
Base system 
  block-mount na->* 
  blockd na->* 
  bridge na->*
  libstdcpp na->*
  qos-scripts na->*
  wireless-tools na->*
  dropbear(取消)
  libatomic na->*
  busybox na->*
    customize busybox options na->*
      Coreutils 
        nohup na->*
    linux module utilities
      depmod na->* (取消)
      insmod na->* (取消)
      lsmod na->* (取消)
      modinfo na->* (取消)
      modprobe na->* (取消)
      rmmod na->* (取消)
      Support version 2.2/2.4 Linux kernels na->* 
    Linux System Utilities
      lsusb na->*
   telnet lsusb na->*   

Firmware
  mt7601u-firmware (取消)
Administration
  htop na->*
  monit na->*
  monit-nossl na->*
  netdata na->*
  sudo na->*
  syslog-ng na->*
development
  ar na->*
  autoconf na->*
  automake na->*
  binutils na->*
  gcc na->*
  gdb na->*
  gdbserver na->*
  libtool-bin na->*
  m4 na->*
  make na->*
  objdump na->*
  patch na->*
  pkg-config na->*
kernel modules
  Block Devices
     kmod-ata-core na->* (应该SATA要用到) 
      kmod-ata-ahci na->* (高速sata) 
     kmod-loop na->* (取消)
  filesystems
     kmod-fs-ext4 na->*
     kmod-fs-ntfs na->*
     kmod-fs-vfat na->*
  input modules
     kmod-hid na->*
     kmod-hid-generic na->*
     kmod-input-core na->*
     kmod-input-evdev na->* 
     
     kmod-input-gpio-keys na->* (取消)
  LED modules
     kmod-leds-gpio
  usb support
     kmod-usb-core na->*
     kmod-usb-hid na->*
     kmod-usb3 na->*
     kmod-usb-storage 
     kmod-usb2 
  Network Support
     kmod-atm (取消)
     kmod-pppoa na->* (取消)
     kmod-pppol2tp na->* (取消)
     kmod-pppox na->* (取消)
     kmod-pptp na->* (取消)
  Wireless Drivers
     kmod-cfg80211 na->* (wifi 80211支持)
     kmod-lib80211 na->* (wifi 80211支持)

  Other modules
     kmod-gpio-button-hotplug (取消)   

libraries
  compression
     libbz2 na->*
     liblzma na->*
     libunrar na->*
Network
  ssh
     autossh （取消）
     openssh-client-utils na->*
     openssh-keygen na->*
     openssh-server na->*
     openssh-client na->*
     openssh-sftp-server na->*
     sshtunne(取消)
  Version Control Systems
     git na->*
     git-gitweb na->*
     git-http na->*
     subversion-client na->*
     subversion-libs na->*
     subversion-server na->*
     subversion server na->*
   ppp-mod-pppoa na->* (取消)
   ppp-mod-passwordfd na->* (取消)
   ppp-mod-pppol2tp na->* (取消)
   ppp-mod-pptp na->* (取消)
   ppp-mod-radius na->* (取消)
   ppp-multilink na->* (取消)
   pppossh na->* (取消)
   iperf
   iperf3 (取消)
   iperf-ssl (取消)
   iotivity(取消)
   shadowsocks-libev(取消)
   Web Servers/Proxies
    shadowsocks-client(取消)
   squid (取消)
   odhcp6c(这两个跳出来会占CPU很高，卸载待定)
   odhcpd(这两个跳出来会占CPU很高，卸载待定)
   wireless
     wifischedule (M)
     horst na->*
   wshaper (取消)
   Bitorrent
     transmission-cli-openssl na->* (取消)
     transmission-daemon-openssl na->* (取消)
     transmission-remote-openssl na->* (取消)
     transmission-web na->* (取消)
   wpa-supplicant (取消)
   wpad na->*
   wpa-supplicant-mesh (取消)
   wpa-supplicant-p2p(取消)
   hostapd na->*
   hostapd-utils na->*

Luci
  Collection
     luci na->*
     luci-ssl-openssl na->*
  modules
     luci-mod-failsafe (取消)
  Applications
     luci-app-ddns na->*
     luci-app-openvpn na->*
     luci-app-mmc-over-gpio na->* (取消)
     luci-app-samba na->* (取消-)
     luci-app-squid na->* (取消)
     luci-app-rp-pppoe-server na->* (取消-)
     luci-app-upnp na->* (取消-)
     luci-app-wifischedule na->* (取消-)
     luci-app-wshaper na->* (取消)
     luci-app-cshark na->* (取消)
     luci-app-ntpc na->*
     luci-app-rp-pppoe-server na->* (取消)
   Themes
     全开
Utilities
  Compression 
     bzip2 na->*
     gzip na->*
     unrar na->*
     unzip na->*
     xz-unitls na->*
     zip na->*
   disk
     cfdisk na->*
     fdisk na->*
   editor
     nano na->*
   filesystem
     ntfs-3g na->*
   shell
     bash na->*
   Terminal
     screem na->*
   dmesg na->*
   grep na->*
   pciutils na->*
   tar na->*
   hwclock na->*
   io na->*
   irqbalance na->*
   mmc-utils na->* (取消)
   ugps na->* (取消)
   usbutils na->* (取消)
   wifitoggle na->*
   wmt na->*


**QA**

1.编译问题luci不匹配内核

解决,改用luci 17.01版本,修改安装源
在源码根目录下
编辑 feeds.conf.default
看到如下源
src-git packages [https://git.lede-project.org/feed/packages.git](https://git.lede-project.org/feed/packages.git)
src-git luci [https://git.lede-project.org/project/luci.git](https://git.lede-project.org/project/luci.git)
src-git routing [https://git.lede-project.org/feed/routing.git](https://git.lede-project.org/feed/routing.git)
src-git telephony [https://git.lede-project.org/feed/telephony.git](https://git.lede-project.org/feed/telephony.git)
全部屏蔽,然后添加源
src-git packages [https://git.lede-project.org/feed/packages.git;lede-17.01](https://git.lede-project.org/feed/packages.git;lede-17.01)
src-git luci [https://git.lede-project.org/project/luci.git;lede-17.01](https://git.lede-project.org/project/luci.git;lede-17.01)
src-git routing [https://git.lede-project.org/feed/routing.git;lede-17.01](https://git.lede-project.org/feed/routing.git;lede-17.01)
src-git telephony [https://git.lede-project.org/feed/telephony.git;lede-17.01](https://git.lede-project.org/feed/telephony.git;lede-17.01)
然后重新执行
./scripts/feeds update -a
./scripts/feeds install -a
全内核重新编译

2.编译时候遇到错误如下
opmini.o: In function `Perl_ck_rvconst':
opmini.c:(.text+0x62ba): undefined reference to `_Static_assert'
opmini.o: In function `Perl_rpeep':
opmini.c:(.text+0xe169): undefined reference to `_Static_assert'
toke.o: In function `Perl_lex_start':
toke.c:(.text+0x92df): undefined reference to `_Static_assert'

解决:
这gcc需要升级到4.8或以上支持C11。因为4.4版本的不支持静态断言
用 update-alternatives --config gcc 可以切换GCC版本

3.编译时候遇到错误如下
 * check_data_file_clashes: Package libustream-openssl wants to install file /media/sdc/bpi-r2-on-lede-17.01.2/build_dir/target-arm_cortex-a7+neon-vfpv4_musl-1.1.16_eabi/root-mediatek/lib/libustream-ssl.so
	But that file is already provided by package * libustream-mbedtls
 * opkg_install_cmd: Cannot install package libustream-openssl.
 * check_data_file_clashes: Package libustream-openssl wants to install file /media/sdc/bpi-r2-on-lede-17.01.2/build_dir/target-arm_cortex-a7+neon-vfpv4_musl-1.1.16_eabi/root-mediatek/lib/libustream-ssl.so
	But that file is already provided by package * libustream-mbedtls
 * opkg_install_cmd: Cannot install package luci-ssl-openssl.
make[2]: *** [package/install] Error 255
make[2]: Leaving directory `/media/sdc/bpi-r2-on-lede-17.01.2'
make[1]: *** [/media/sdc/bpi-r2-on-lede-17.01.2/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl-1.1.16_eabi/stamp/.package_install] Error 2
make[1]: Leaving directory `/media/sdc/bpi-r2-on-lede-17.01.2'
make: *** [world] 错误 2

解决:
在源码根目录下编辑.config
修改如下编译选项
CONFIG_PACKAGE_libmbedtls=y
改为 
CONFIG_PACKAGE_libmbedtls=m

CONFIG_PACKAGE_libustream-mbedtls=y
改为
CONFIG_PACKAGE_libustream-mbedtls=m

4.boot loader没有选项

解决:
编辑 .config
添加
# 
# Boot Loaders
# 
CONFIG_PACKAGE_u-boot-bpi_r2=y

5.编译错误如下
Collected errors:
 * satisfy_dependencies_for: Cannot satisfy the following dependencies for px5g-mbedtls:
 * 	libmbedtls * 
 * opkg_install_cmd: Cannot install package px5g-mbedtls.
 * satisfy_dependencies_for: Cannot satisfy the following dependencies for luci-ssl:
 * 	libustream-mbedtls * 
 * opkg_install_cmd: Cannot install package luci-ssl.

解决:
编辑 .config
CONFIG_PACKAGE_px5g-mbedtls=y
改为
CONFIG_PACKAGE_px5g-mbedtls=m

6.编译错误如下

Collected errors:
 * satisfy_dependencies_for: Cannot satisfy the following dependencies for luci-ssl:
 * 	libustream-mbedtls * 	px5g * 
 * opkg_install_cmd: Cannot install package luci-ssl.
make[2]: *** [package/install] Error 255
make[2]: Leaving directory `/media/sdc/bpi-r2-on-lede-17.01.2'
make[1]: *** [/media/sdc/bpi-r2-on-lede-17.01.2/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl-1.1.16_eabi/stamp/.package_install] Error 2
make[1]: Leaving directory `/media/sdc/bpi-r2-on-lede-17.01.2'
make: *** [world] 错误 2

解决:
编辑 .config
CONFIG_PACKAGE_luci-ssl=y
改为
CONFIG_PACKAGE_luci-ssl=m

7.编译错误如下:
 * satisfy_dependencies_for: Cannot satisfy the following dependencies for libustream-mbedtls:
 * 	libmbedtls * 
 * opkg_install_cmd: Cannot install package libustream-mbedtls.

解决:
编辑 .config
CONFIG_PACKAGE_libustream-mbedtls=y 直接屏蔽

8.域名不能解释的问题
在源码目录进入target/linux/mediatek/base-files/etc/init.d
添加bootnet的文件加入如下代码:
#!/bin/sh /etc/rc.common

START=100

start() {
    rm /etc/resolv.conf
    ln -s /tmp/resolv.conf.auto /etc/resolv.conf   
}
重新打包镜像,即可恢复域名.
或直接在镜像安装完后在
/etc/rc.local中添加
rm /etc/resolv.conf 
ln -s /tmp/resolv.conf.auto /etc/resolv.conf

8.修正网络不能连接问题

解决:
进入源码目录target/linux/mediatek/base-files/etc/config
编辑network文件

把config interface 'wan'
    option ifname 'eth1'
    option proto 'dhcp'
改为
config interface 'wan'
    option ifname 'wan'(<<<<<<<)
    option proto 'dhcp'

把config interface 'wan6'        
    option ifname 'eth1'      
    option proto 'dhcpv6' 

改为
config interface 'wan6'        
    option ifname 'wan6'  (<<<<<<<)    
    option proto 'dhcpv6' 

重新打包镜像,即可恢复网络连接

9.编译时出现如下错误
horst出现 error: 'mempcpy' undeclared here (not in a function)
 _FORTIFY_FN(mempcpy) void *mempcpy(void *__d, const void *__s, size_t __n)

解决:
编辑源码feeds/packages/net/horst/Makefile
PKG_FORTIFY_SOURCE:=0

10.编译hostap出现 Line 11: unknown configuration item 'ieee80211n'

解决:
编辑源码package/network/services/hostapd/Config.in
修改为如下
config DRIVER_WEXT_SUPPORT
    bool
    default y

config DRIVER_11N_SUPPORT
    bool
    default y

config WPA_RFKILL_SUPPORT
    bool "Add rfkill support"
    depends on PACKAGE_wpa-supplicant || PACKAGE_wpa-supplicant-mesh || PACKAGE_wpa-supplicant-mini || PACKAGE_wpad || PACKAGE_wpad-mini || PACKAGE_wpad-mesh
    default n

config DRIVER_11AC_SUPPORT
    bool
    default y

config DRIVER_11W_SUPPORT
    bool
    default y

11.手工添加重定向管理端口,无需每次刷内核后重新打开的方法
进入build_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/root-mediatek/etc/config/目录
编辑firewall添加如下代码
config redirect
    option target 'DNAT'
    option src 'wan'
    option dest 'lan'
    option proto 'tcp'
    option src_dport '80'
    option dest_ip '192.168.103.1'
    option dest_port '80'
    option name 'http'
config redirect
    option target 'DNAT'
    option src 'wan'
    option dest 'lan'
    option proto 'tcp'
    option src_dport '443'
    option dest_ip '192.168.103.1'
    option dest_port '443'
    option name 'https'

config redirect
    option target 'DNAT'
    option src 'wan'
    option dest 'lan'
    option proto 'tcp'
    option src_dport '22'
    option dest_ip '192.168.103.1'
    option dest_port '22'
    option name 'ssh'

12.解决启动无线卡每次系统启动后自动添加新的无线接口bug

分析wmt_loader
调用顺序wmt_loader.c->wmt_detect.c->conn_drv_init.c->wlan_drv_init.c->gl_init.c
调用WMT_LOADER的时候会初始化WIFI驱动，然后驱动不是直接去修改/etc/config/wireless，
而是触发了一个注册的WIFI事件，然后事件调用了lib下的几个.sh文件，然后这些文件再调用了
/sbin/wifi最后调用/lib/wifi/mac80211.sh生成接口触发条件 wifi config

解决:
修改build_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/root-mediatek/lib/wifi/mac80211.sh 如下
修改check_mac80211_device() 函数
在
	while :; do
		config_get type "radio$devidx" type		
[ -n "$type" ] || break
		devidx=$(($devidx + 1))
	done
后加入如下代码让脚本当检测到配置里面已经加入了选项的时候不再自动往里面加接口
#add by hyonex if devidx >0 it means the config has modified the exit
	[ 0 -lt $devidx ] && return 0

13.让内核自动启动WIFI模块及AP模块

解决:
以上述第8点为基础,修改bootnet文件为.
#!/bin/sh /etc/rc.common
START=100
start() {
    rm /etc/resolv.conf
    ln -s /tmp/resolv.conf.auto /etc/resolv.conf
    wmt_loader
    stp_uart_launcher -p /etc/firmware &
    sleep 6
    echo 1 >/dev/wmtWifi
    echo A >/dev/wmtWifi
    ifconfig wlan0 up
    ifconfig ap0 up
}
重新编译内核,写入启动后自动增加启动WIFI及AP

14.使能管理页面启动AP及WIFI
以下ap和wifi会互斥
hostapd -P /var/run/wifi-phy0.pid -B /var/run/hostapd-phy0.conf 
wpa_supplicant -B -P /var/run/wpa_supplicant-wlan0.pid -D nl80211 -i wlan0 -c /var/run/wpa_supplicant-wlan0.conf -C /var/run/wpa_supplicant -H /var/run/hostapd/ap0
ap和wifi共存
hostapd -P /var/run/wifi-phy0.pid -B /var/run/hostapd-phy0.conf 
wpa_supplicant -B -P /var/run/wpa_supplicant-wlan0.pid -D nl80211 -i wlan0 -c /var/run/wpa_supplicant-wlan0.conf -C /var/run/wpa_supplicant

解决:修改luci对应的源码
源码在如下目录

/lib/netifd/netifd-wireless.sh的_wdev_handler函数是总入口
所有的bug,导致到wifi的ap配置不生效.
/lib/netifd/wireless/mac80211.sh
里的mac80211_prepare_vif函数中
在case "$mode" in中几乎所有都出现直接return
而没有json_select ..退出本级json节点导致到后面的流程所有的json
都不能正常
修正ap与client冲突问题.
mac80211_setup_supplicant中
直接return 的逻辑相反了不用||应该用&&
wpa_supplicant_run "$ifname" ${hostapd_ctrl:+-H $hostapd_ctrl}把后面${hostapd_ctrl:+-H $hostapd_ctrl}去掉否则ap会被杀掉
对应源码目录:
build_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/root-mediatek/lib/netifd/wireless

15.git CA出错时候

解决:
输入关闭CA检查
export GIT_SSL_NO_VERIFY=1











