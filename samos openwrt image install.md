**目录**

**设备说明**

1.接口说明
![图片](https://images-cdn.shimo.im/6jUXndDFoUcIoJWA/image.undefined!thumbnail)

2.设备主配置说明
| Hardware |    | 
|
| Soc | MediaTek MT7623N, Quad-code ARM Cortex-A7(32bit 1.3GHZ,4 CORES) | 
| GPU | Mali 450 MP4(支持OPENGL ES1.1,OPENCL,CUDA) | 
| SDRAM | 2GB DDR3 (shared with GPU) | 
| Power | 12V @ 2A via DC power | 
| Features |    | 
| Low-level peripherals | 40 Pins Header, 28×GPIO, some of which can be used for specific functions including UART, I2C, SPI, PWM, I2S. | 
| On board Network | 5x 10/100/1000Mbps Ethernet (MT7530) | 
| Wifi | Wifi 802.11 a/b/g/n 2.4GHz/ 5GHz(MT6625L) | 
| Bluetooth | BT4.1 BLE with MTK6625L chip | 
| On board Storage | MicroSD (TF) card, 2 SATA 6Gbps ,eMMC | 
| Display | HDMI (Type A) output with HDCP 1.4, resolutions up 1920x1200； MIPI Display Serial Interface (DSI) interface(4 data lanes) | 
| Video decoder | Multi-format FHD video decoding, including Mpeg1/2, Mpeg4, H.263, H.264, etc H.264 high profile 1080p@60fps, HEVC/H.265 1080P@60fps | 
| Video encoder | VP8 encoder: 1080p@30fps H.264 encoder: high profile 1080p@30fps | 
| Audio outputs | HDMI, I2S audio | 
| PCIE | 1 pcie interface & 1 pcie pin define interface(mini pcie) | 
| USB 3.0 | 2x USB 3.0 host | 
| USB 2.0 | 1x USB 2.0 OTG | 
| Buttons | Reset button, Power button, U-boot button | 
| Leds | Red, Green, Blue | 
| Other | IR reciever(红外) | 
|    |    | 
|    |    | 


**获取镜像**

镜像可分为写入tf卡启动镜像,emmc启动镜像及在线升级镜像.
tf卡刷镜像以sd结尾的img文件
emmc镜像以emmc结尾的img文件
在线升级镜像以sysupgrade结尾的tar文件
用户可按照以下方式获取镜像
1.自行编译
可按照<samos 镜像编译篇>自行编译生成内核
2.亦可从我们官方直接下载已生成镜像.
官方镜像已默认安装好manager及node软件并配置好默认打开了WAN口管理及所有skywire的端口,全部环境并进行了系统调优.

**安装**
镜像的安装可分为卡刷,线刷及在线升级方式.
1.卡刷
获取到img镜像
1)windows 
下载Win32DiskImager安装运行
![图片](https://images-cdn.shimo.im/BPEUOZNLr7sUotjz/image.undefined!thumbnail)
映像文件选择mtk-bpi-r2-sd.img,设备选择目标TF卡.其他必须按图中的配置
然后点击写入,等待进度条完成.把TF卡取入插入目标矿机.

浏览器打开 [http://192.168.0.2:8000](http://localhost:8000/). 默认密码是: 1234.
2)linux
输入如下命令,按照实际修改自己的目标盘
dd if=mtk-bpi-r2-sd.img of=/dev/目标盘

2.线刷(当前仅支持WINDOWS)
获取到img镜像
下载zadig2.3
下载bpi-fel-mass-storage
设备先断开电源线，直接在下图红色标注处插上USB-OTG线一头,另外一端连接电脑,按着U-Boot按键不放,然后上电.

![图片](https://images-cdn.shimo.im/AAPfq0NJx3YzV1TB/image.undefined!thumbnail)
   此时你会在“我的电脑”-右键“管理”-“设备管理器”中看到一个“unknown device”
![图片](http://bbs.elecfans.com/data/attachment/album/201705/01/130925isq4uk6mesbb9mev.jpg)
打开zadig软件，设置libusbK(v3.0.7.0)，点击Install Driver。
![图片](http://bbs.elecfans.com/data/attachment/album/201705/01/110721x6r0k9cekya0r9jy.png)
查看“设备管理器”中已经识别了libusbK USB Devices，这个设备的名字叫“Unknown Device #1”，其实也就是我们的设备
![图片](http://bbs.elecfans.com/data/attachment/album/201705/01/131521j5smf3cvavgam8ta.jpg)
挂载开发板emmc
打开软件bpi-fel-mass-storage-gui4win.exe，点击“Detect Device”。
![图片](http://bbs.elecfans.com/data/attachment/album/201705/01/110722dacajoam5h44z599.png)![图片](http://bbs.elecfans.com/data/attachment/album/201705/01/110722t02057sw5qcs55zn.png)点击“Launch”，稍等一会。
![图片](http://bbs.elecfans.com/data/attachment/album/201705/01/110723hdyohagmz6a9ldgu.png)结束之后会在“我的电脑”中看到开发板的emmc已经被当作一个“U盘”被挂载到了系统中。
格式化emmc
在“我的电脑”-右键“管理”-“磁盘管理”里看到的emmc是这样子：
![图片](http://bbs.elecfans.com/data/attachment/album/201705/01/110720elfgcdovdowodlog.jpg)
右键“删除卷”-右键“删除卷”，全部卷已删除之后，右键“新建简单卷”-一直“下一步”-“完成”。这时会在“我的电脑”中出现一个7.28G的“U盘”。
然后参考以上卡刷一节的windows卡刷方法,把EMMC镜像刷入即可.

3.在线升级
打开samos设备的管理页面,如果是连在lan口上则输入[https://192.168.0.1,](https://192.168.0.1,)
如果连在wan口则输入设备对应的地址即可进入管理页面.
设备初始化用户名密码:
user:root
password:123456
然后选择页面system->backup/flash firmware->Flash new firmware image.
点击'选择文件'按钮选择相应的在线升级文件,然后点击'flash image'按钮,系统将进行在线升级.升级过程中切勿断电,升级可能需要10到15分钟,且如果用WAN升级,升级完成后因为IP改变而看不到重启页面,请手工重启.

**设备的管理**

1.设备的管理页面
lan口上则输入[https://192.168.0.1](https://192.168.0.1%2C/)
wan口则输入设备对应的地址
2.skywire的管理页面
浏览器打开 [http://192.168.0.2:8000](http://localhost:8000/). 默认密码是: 1234.

**软件的安装与卸载**

官方的img已经默认安装了skywire的manager及node.无需另外安装.
用户自行编译的镜像需要自行安装软件.
在system->software页面下点击Available packages页,然后找到需要安装的软件包点击install即完成安装.
卸载则在同页面下选择installed packages的页面找到对应需要卸载的软件包点击remove即可完成卸载.

**QA**

1.打开ssh的远程登录root权限

解决:
可以对 openssh server进行配置
nano /etc/ssh/sshd_config
找到PermitRootLogin no一行，改为PermitRootLogin yes
若不存在该行则自行添加
然后重启！   
service sshd restart

2.git CA出错时候

解决:
输入关闭CA检查
export GIT_SSL_NO_VERIFY=1

3.所有修改完后若出现不存盘的现象

解决:
执行强制同步硬盘
sync

4.需要挂载其它USB存储设备

解决:
vi /etc/fstab
添加
/dev/sda1 /mnt/temp vfat rw 0 0
然后重启
mount -t vfat /dev/sda1 /mnt/temp

5.镜像到底是使用tf卡好还是emmc好.
官方建议使用emmc.以下是sd(tf)与内部emmc的民间性能测试报告.以供参考
![图片](http://c.51hei.com/d/forum/201609/08/192719xcapctgtv12tl2ko.png)
![图片](http://c.51hei.com/d/forum/201609/08/192719xy48zxnzme3d43xy.png)

![图片](http://c.51hei.com/d/forum/201609/08/192720obci0hq030c3gq3d.png)


