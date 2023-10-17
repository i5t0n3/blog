### 问题描述

在使用 `apt-get install -y tree` 的时候，发现一堆报错，无法进行安装

```shell
root@Kylin:~# apt-get install -y tree
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
下列软件包是自动安装的并且现在不需要了：
  cifs-utils libtalloc2 libwbclient0 localechooser-data samba-common user-setup
使用'apt autoremove'来卸载它(它们)。
下列【新】软件包将被安装：
  tree
升级了 0 个软件包，新安装了 1 个软件包，要卸载 0 个软件包，有 148 个软件包未被升级。
有 3 个软件包没有被完全安装或卸载。
需要下载 40.6 kB 的归档。
解压缩后会消耗 138 kB 的额外空间。
获取:1 https://mirrors.ustc.edu.cn/ubuntu xenial/universe amd64 tree amd64 1.7.0-3 [40.6 kB]
已下载 40.6 kB，耗时 0秒 (79.1 kB/s)
dpkg: 依赖关系问题使得 libstdc++6:amd64 的配置工作不能继续：
 libstdc++6:amd64 依赖于 gcc-5-base (= 5.4.0-6ubuntu1~16.04.12)；然而：
系统中 gcc-5-base:amd64 的版本为 5.4.0-6kord1~16.04.12。

dpkg: 处理软件包 libstdc++6:amd64 (--configure)时出错：
 依赖关系问题 - 仍未被配置
在处理时有错误发生：
 libstdc++6:amd64
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

先看看系统信息

```shell
root@Kylin:~# uname -a
Linux Kylin 4.11.0-14-generic #20~16.04.1kord0k1-Ubuntu SMP Wed Oct 18 00:56:13 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
root@Kylin:~# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.7 LTS
Release:	16.04
Codename:	xenial
```

尝试直接安装指定版本 gcc 失败

```shell
root@Kylin:~# apt-get install gcc-5=5.4.0-6ubuntu1~16.04.12
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
下列软件包是自动安装的并且现在不需要了：
  cifs-utils libtalloc2 libwbclient0 localechooser-data samba-common user-setup
使用'apt autoremove'来卸载它(它们)。
建议安装：
  gcc-5-multilib gcc-5-doc gcc-5-locales libgcc1-dbg libgomp1-dbg libitm1-dbg libatomic1-dbg libasan2-dbg liblsan0-dbg libtsan0-dbg libubsan0-dbg libcilkrts5-dbg libmpx0-dbg libquadmath0-dbg
下列软件包将被升级：
  gcc-5
升级了 1 个软件包，新安装了 0 个软件包，要卸载 0 个软件包，有 147 个软件包未被升级。
有 3 个软件包没有被完全安装或卸载。
需要下载 0 B/8,612 kB 的归档。
解压缩后将会空出 8,192 B 的空间。
dpkg: 依赖关系问题使得 libstdc++6:amd64 的配置工作不能继续：
 libstdc++6:amd64 依赖于 gcc-5-base (= 5.4.0-6ubuntu1~16.04.12)；然而：
系统中 gcc-5-base:amd64 的版本为 5.4.0-6kord1~16.04.12。

dpkg: 处理软件包 libstdc++6:amd64 (--configure)时出错：
 依赖关系问题 - 仍未被配置
在处理时有错误发生：
 libstdc++6:amd64
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

尝试卸载 gcc 失败

```shell
root@Kylin:~# apt-get remove gcc
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
下列软件包是自动安装的并且现在不需要了：
  cifs-utils g++-5 libstdc++-5-dev libtalloc2 libwbclient0 localechooser-data samba-common user-setup
使用'apt autoremove'来卸载它(它们)。
下列软件包将被【卸载】：
  build-essential g++ gcc
升级了 0 个软件包，新安装了 0 个软件包，要卸载 3 个软件包，有 146 个软件包未被升级。
有 3 个软件包没有被完全安装或卸载。
解压缩后将会空出 81.9 kB 的空间。
您希望继续执行吗？ [Y/n] y
dpkg: 依赖关系问题使得 libstdc++6:amd64 的配置工作不能继续：
 libstdc++6:amd64 依赖于 gcc-5-base (= 5.4.0-6ubuntu1~16.04.12)；然而：
系统中 gcc-5-base:amd64 的版本为 5.4.0-6kord1~16.04.12。

dpkg: 处理软件包 libstdc++6:amd64 (--configure)时出错：
 依赖关系问题 - 仍未被配置
在处理时有错误发生：
 libstdc++6:amd64
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

手动安装 gcc-5

```shell
wget http://security.ubuntu.com/ubuntu/pool/main/g/gcc-5/gcc-5-base_5.4.0-6ubuntu1~16.04.12_amd64.deb
dpkg -i gcc-5-base_5.4.0-6ubuntu1~16.04.12_amd64.deb
```

再次执行 `apt-get install -y libnih1` 不提示 gcc 的问题了，转为 libnih-dbus1 了

```shell
root@Kylin:/tmp# apt-get install -y libnih1
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
libnih1 已经是最新版 (1.0.3-4.3ubuntu1)。
下列软件包是自动安装的并且现在不需要了：
  cifs-utils libtalloc2 libwbclient0 localechooser-data samba-common user-setup
使用'apt autoremove'来卸载它(它们)。
升级了 0 个软件包，新安装了 0 个软件包，要卸载 0 个软件包，有 149 个软件包未被升级。
有 2 个软件包没有被完全安装或卸载。
解压缩后会消耗 0 B 的额外空间。
dpkg: 依赖关系问题使得 libnih-dbus1:amd64 的配置工作不能继续：
 libnih-dbus1:amd64 依赖于 libnih1 (= 1.0.3-4.3ubuntu1)；然而：
系统中 libnih1:amd64 的版本为 1.0.3-4.3kord1。

dpkg: 处理软件包 libnih-dbus1:amd64 (--configure)时出错：
 依赖关系问题 - 仍未被配置
dpkg: 依赖关系问题使得 upstart 的配置工作不能继续：
 upstart 依赖于 libnih-dbus1 (>= 1.0.0)；然而：
  软件包 libnih-dbus1:amd64 尚未配置。

dpkg: 处理软件包 upstart (--configure)时出错：
 依赖关系问题 - 仍未被配置
在处理时有错误发生：
 libnih-dbus1:amd64
 upstart
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

### 解决方案

**卸载 `libstdc++6`，重新安装 `apt`**

为什么这么搞也是巧合，有几台克隆的虚拟机情况都一样，安装什么都提示最开始的错误信息，然后在一开始提示 `dpkg: 处理软件包 libstdc++6:amd64 (--configure)时出错：` 的时候我就直接给卸载了，接着发现 `apt` 没了，死马当活马医，去 `ubuntu` 的网站 https://www.ubuntuupdates.org/ 搜索包然后下载安装一通就可以了，然后我在第二台虚拟机里尝试挨个安装指定版本的时候，发现其他错误信息又出来了，后面干脆和第一台步骤一致再尝试一下，结果依旧没问题，莫名其妙的。

> 下载包的时候需要注意系统版本，我在 `uname -a` 中 `Codename: xenial`，所以我找的都是 `xenial` 的。

```shell
root@Kylin:/tmp# apt-get remove libstdc++6:amd64
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
下列软件包是自动安装的并且现在不需要了：
  checkpolicy cifs-utils crda dmeventd dpkg-dev fontconfig fontconfig-config fonts-dejavu-core gir1.2-glib-2.0 iso-codes iw libapol4 libasan2 libatomic1 libcairo2 libcap-ng0 libcgmanager0 libdatrie1 libdbus-glib-1-2 libdevmapper-event1.02.1 libedit2 libfontconfig1 libfreetype6 libfuse2 libgirepository-1.0-1 libglib2.0-0 libgomp1
  libgraphite2-3 libharfbuzz0b libitm1 liblsan0 liblvm2app2.2 liblvm2cmd2.02 libmpx0 libnl-3-200 libnl-genl-3-200 libpango-1.0-0 libpangocairo-1.0-0 libpangoft2-1.0-0 libpcre16-3 libpcre32-3 libpipeline1 libpixman-1-0 libqpol1 libquadmath0 libtalloc2 libthai-data libthai0 libtsan0 libwbclient0 libwrap0 libx11-6 libx11-data
  libxau6 libxcb-render0 libxcb-shm0 libxcb1 libxdmcp6 libxext6 libxrender1 localechooser-data os-prober pkg-config python-apt-common python-ipy python-selinux python-semanage python3-dbus python3-gi python3-pycurl samba-common selinux-utils ttf-ubuntu-font-family wireless-regdb
使用'apt autoremove'来卸载它(它们)。
将会同时安装下列软件：
  anacron libfuse2 logrotate plymouth
建议安装：
  default-mta | mail-transport-agent powermgmt-base fuse mailx desktop-base plymouth-themes
推荐安装：
  cron | cron-daemon rsyslog | system-log-daemon plymouth-theme-ubuntu-text | plymouth-theme
下列软件包将被【卸载】：
  adduser apt apt-transport-https apt-utils build-essential cron dbus fuse g++ g++-5 gcc gcc-5 gettext-base groff-base grub-common grub-gfxpayload-lists grub-pc grub-pc-bin grub2-common ifupdown init initramfs-tools initramfs-tools-core kylin-standard-server libapt-inst2.0 libapt-pkg5.0 libasprintf0v5 libcc1-0 libcilkrts5
  libgcc-5-dev libicu55 libpam-systemd libpcre3-dev libpcrecpp0v5 libstdc++-5-dev libstdc++6 libubsan0 libxml2 linux-image-4.11.0-14-generic linux-image-extra-4.11.0-14-generic lvm2 lzma man-db ntfs-3g openssh-client openssh-server openssh-sftp-server p7zip plymouth-label policycoreutils python-sepolgen python-sepolicy
  python-setools python3-apt python3-software-properties rsyslog software-properties-common ssh systemd systemd-sysv tasksel tasksel-data udev unrar user-setup uuid-runtime vsftpd
下列【新】软件包将被安装：
  anacron
下列软件包将被升级：
  libfuse2 logrotate plymouth
【警告】：下列基础软件包将被卸载。
请勿尝试，除非您确实知道您在做什么！
  apt libapt-pkg5.0 (是由于 apt) libstdc++6 (是由于 apt) adduser (是由于 apt) init systemd-sysv (是由于 init)
升级了 3 个软件包，新安装了 1 个软件包，要卸载 67 个软件包，有 119 个软件包未被升级。
有 1 个软件包没有被完全安装或卸载。
需要下载 27.1 kB/262 kB 的归档。
解压缩后将会空出 428 MB 的空间。
您的操作有潜在的危害性。
若要继续，请输入下面的短句“是，按我说的做！”
 ?] 
```

复制 `是，按我说的做！` 输入进去按回车就行。

然后你就能发现 `apt` 没了~接下来就是下载一堆包，挨个安装吧

```shell
wget http://security.ubuntu.com/ubuntu/pool/main/a/apt/apt_1.2.35_amd64.deb
wget http://security.ubuntu.com/ubuntu/pool/main/a/apt/libapt-pkg5.0_1.2.35_amd64.deb
wget http://security.ubuntu.com/ubuntu/pool/main/g/gcc-5/libstdc++6_5.4.0-6ubuntu1~16.04.12_amd64.deb
wget http://security.ubuntu.com/ubuntu/pool/main/a/adduser/adduser_3.113+nmu3ubuntu4_all.deb
wget http://security.ubuntu.com/ubuntu/pool/main/g/gcc-5/gcc-5-base_5.4.0-6ubuntu1~16.04.12_amd64.deb
dpkg -i adduser_3.113+nmu3ubuntu4_all.deb 
dpkg -i libstdc++6_5.4.0-6ubuntu1~16.04.12_amd64.deb 
dpkg -i gcc-5-base_5.4.0-6ubuntu1~16.04.12_amd64.deb 
dpkg -i libstdc++6_5.4.0-6ubuntu1~16.04.12_amd64.deb 
dpkg -i libapt-pkg5.0_1.2.35_amd64.deb 
dpkg -i apt_1.2.35_amd64.deb 
```

所需依赖装好后，再次尝试安装 `tree`

```shell
root@Kylin:/tmp# apt-get install -y tree
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
tree 已经是最新版 (1.7.0-3)。
您可能需要运行“apt-get -f install”来纠正下列错误：
下列软件包有未满足的依赖关系：
 cpp-5 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
 dpkg : 破坏: ufw (< 0.35-0ubuntu2~) 但是 0.35-0kord2 正要被安装
 libasan2 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
 libatomic1 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
 libgomp1 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
 libitm1 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
 liblsan0 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
 libmpx0 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
 libnih-dbus1 : 依赖: libnih1 (= 1.0.3-4.3ubuntu1) 但是 1.0.3-4.3kord1 正要被安装
 libquadmath0 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
 libtsan0 : 依赖: gcc-5-base (= 5.4.0-6kord1~16.04.12) 但是 5.4.0-6ubuntu1~16.04.12 正要被安装
E: 有未能满足的依赖关系。请尝试不指明软件包的名字来运行“apt-get -f install”(也可以指定一个解决办法)。
```

可以看到可以正确的使用 apt 安装软件了，但还有一些提示需要解决。

执行 `apt-get -f install` 来解决依赖，接着执行 `apt autoremove` 来卸载一些用不到的安装包
