# 13、2015-07-18
* 使用debootstrap制作chroot环境，可以创建docker镜像，32位系统

# 12、2015-07-09
* ssh tunnel

  ssh -N -f -D 0.0.0.0:1080 -p 5381 116.50.32.212

  然后输入密码，这样就可以建立加密通道了

  在浏览器中使用时，要注意选择"SOCKS 代理"，协议要选择"SOCKS v5"

  在chrome中用的是SwitchySharp扩展插件

# 11、2015-07-08
* pptpd使用的端口
  tcp 1723
  raw 47

* 打开pptpd的日志

  vi /etc/pptpd.conf
  <<
    # 将debug前的# 去掉或加上debug
    debug
  >>

  centos下会记录到/var/log/message中

* 安装pptpd
  yum install -y perl ppp iptables
  rpm -q ppp
  strings '/usr/sbin/pppd' |grep -i mppe | wc --lines
  rpm -Uvh http://poptop.sourceforge.net/yum/stable/rhel6/pptp-release-current.noarch.rpm
  yum install pptpd
  cp /etc/ppp/options.pptpd /etc/ppp/options.pptpd.bak
  vi /etc/ppp/options.pptpd
  <<
  ms-dns 8.8.8.8
  ms-dns 8.8.4.4
  >>
  cp /etc/ppp/chap-secrets /etc/ppp/chap-secrets.bak
  vi /etc/ppp/chap-secrets
  <<
  dongfong pptpd Test@2015 *
  >>
  cp /etc/pptpd.conf /etc/pptpd.conf.bak
  vi /etc/pptpd.conf
  <<
  localip 192.168.9.1
  remoteip 192.168.9.11-30
  >>
  vi /etc/sysctl.conf
  <<
  net.ipv4.ip_forward = 1
  >>
  /sbin/sysctl -p
  /sbin/service pptpd start
  /sbin/service iptables start
  /sbin/iptables -t nat -A POSTROUTING -o eth0 -s 192.168.9.0/24 -j MASQUERADE
  /etc/init.d/iptables save
  /sbin/service iptables restart

# 10、2015-06-28
* anyplex ust new pptpd
ksharpdabu pptpd sky *
username: ksharpdabu
password: sky
ip: 122.152.173.184

root/orbitc206d-touch3

/sbin/iptables -t nat -A POSTROUTING -o em1 -s 192.168.9.0/24 -j MASQUERADE

# 9、2015-06-11
* [Linux下高并发socket最大连接数所受的各种限制](http://blog.csdn.net/guowake/article/details/6615728)

  在Linux平台上，无论编写客户端程序还是服务端程序，在进行高并发TCP连接处理时，最高的并发数量都要受到系统对用户单一进程同时可打开文件数量的限制(这是因为系统为每个TCP连接都要创建一个socket句柄，每个socket句柄同时也是一个文件句柄)。

# 8、2015-06-05
* id命令查看用户uid和gid

  id root

* 查看文件的完整时间

  ls --full-time

* 用ssh连接另外一台服务器时报如下错误

no matching cipher found: client blowfish-cbc,aes128-cbc,3des-cbc,cast128-cbc,arcfour,aes192-cbc,aes256-cbc server aes128-ctr,aes192-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com

  另外一台服务器连就没有问题，这两台的系统版本是一样的，对比了下openssh-client的版本
  出错那台的有：
  OpenSSL 0x1000105f

  后来用yum install openssh-client后，升级了下程序，再连就可以了

* 修改ubuntu 14.04.1的本地源为国内

      cd /etc/apt/
      cp sources.list sources.list-20150605

      > sources.list
      vi sources.list
      <<

      >>  
      deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
      deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
      deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
      deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
      deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
      deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
      deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
      deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
      deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
      deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse

      deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
      deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
      deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
      deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
      deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse
      deb-src http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
      deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
      deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
      deb-src http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
      deb-src http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse

      deb http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse
      deb http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse
      deb http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse
      deb http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse
      deb http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse
      deb-src http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse
      deb-src http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse
      deb-src http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse
      deb-src http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse
      deb-src http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse
      >>

# 7、2015-06-04
* [LC_ALL=C的含义](http://blog.chinaunix.net/xmlrpc.php?r=blog/article&uid=29312110&id=4480485)

  在很多的shell脚本中，我们经常会看见某一句命令的前面有一句“LC_ALL=C”  
  SAR_CMD="LC_ALL=C sar -u -b 1 5 | grep -i average "  
  这到底是什么意思？  
  LC_ALL=C 是为了去除所有本地化的设置，让命令能正确执行。

* 通过文件名获取包名

  apt-get install apt-file

  apt-file search filename

  centos:
  yum provides filename

# 6、2015-05-30
* linux关机命令

  halt = shutdown -h now

# 5、2015-05-26
* [Linux Used内存到底哪里去了？](http://blog.yufeng.info/archives/2456)

  前几天 纯上 同学问了一个问题：

  > 我ps aux看到的RSS内存只有不到30M，但是free看到内存却已经使用了7,8G了，已经开始swap了，请问ps aux的实际物理内存统计是不是漏了哪些内存没算？我有什么办法确定free中used的内存都去哪儿了呢？

  这个问题不止一个同学遇到过了，之前子嘉同学也遇到这个问题，内存的计算总是一个迷糊账。 我们今天来把它算个清楚下!

  keywords: linux memory

* [一次linux内存问题排查-slab](http://bhsc881114.github.io/2015/04/19/%E4%B8%80%E6%AC%A1linux%E5%86%85%E5%AD%98%E9%97%AE%E9%A2%98%E6%8E%92%E6%9F%A5-slab/)

  一次内存告警的排查过程，linux内存占用分析

  > keywords: linux memory slab

# 4、2015-05-25
* 给其他登陆用户发送消息

  通过last命令，得到pts/0，然后用write发送，root表示用户名

  write root pts/0

# 3、2015-05-24
* [Linux Kernel not passing through multicast UDP packets](http://serverfault.com/questions/163244/linux-kernel-not-passing-through-multicast-udp-packets)

  Recently I've set up a new Ubuntu Server 10.04 and noticed my UDP server is no longer able to see any multicast data sent to the interface, even after joining the multicast group. I've got the exact same set up on two other Ubuntu 8.04.4 LTS machines and there is no problem receiving data after joining the same multicast group.

  > keywords: linux multicast rp_filter

# 2、2015-05-19
* 查看文件系统类型

      root@docker:~# file -s /dev/sda1
      /dev/sda1: Linux rev 1.0 ext4 filesystem data, UUID=8166c701-9637-4ffe-8895-20c9b0b4afc6 (needs journal recovery) (extents) (large files) (huge files)

# 1、2015-04-16
* 使用`ldd`查看程序依赖的so库文件
