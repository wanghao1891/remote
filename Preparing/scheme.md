# 5、2015-07-13
* vicare安装
  * prepare
    apt-get install autoconf libgmp-dev texinfo
  * download
    wget https://bbuseruploads.s3.amazonaws.com/marcomaggi/vicare-scheme/downloads/vicare-scheme-0.4d0pre1.tar.xz?Signature=S7GH97hoY5G15oaeUCBm5jrhyQM%3D&Expires=1436802556&AWSAccessKeyId=0EMWEFSGA12Z1HF1TZ82&response-content-disposition=attachment%3B%20filename%3D%22vicare-scheme-0.4d0pre1.tar.xz%22
  * compile
    tar -zxvf vicare-scheme-0.4d0pre1.tar.xz
    cd vicare-scheme-0.4d0
    mkdir buile
    cd build/
    ../configure --prefix=/root/workspace/bin/vicare-scheme-0.4d0
    make
    make install

# 4、2015-07-04
* 用guile执行脚本，默认编译为字节码的文件位置

  /root/.cache/guile/ccache/2.0-LE-8-2.0/root/workspace/src/mine/backend-guile/base.ss.go

  脚本位置：
  /root/workspace/src/mine/backend-guile/base.ss

* A simple http client
  (use-modules (ice-9 popen))
  (use-modules (ice-9 rdelim))
  (let ((s (socket PF_INET SOCK_STREAM 0)))
    (connect s AF_INET (inet-pton AF_INET "127.0.0.1") 80)
    (display "GET / HTTP/1.0\r\n\r\n" s)

    (do ((line (read-line s) (read-line s)))
        ((eof-object? line))
      (display line)
      (newline)))

  > socket http client

* guile repl 配置文件

  vi .guile
  <<
  (use-modules (ice-9 readline))
  (activate-readline)
  >>

* /usr/local/lib/guile/2.0/ccache 这个目录下的.go文件应该是.scm编译后生成的

  对应的源代码路径：/usr/local/share/guile/2.0/

* GNU Readline library
  To make it easier for you to repeat and vary previously entered expressions, or to edit the expression that you’re typing in, Guile can use the GNU Readline library. This is not enabled by default because of licensing reasons, but all you need to activate Readline is the following pair of lines.

  scheme@(guile-user)> (use-modules (ice-9 readline))
  scheme@(guile-user)> (activate-readline)
  It’s a good idea to put these two lines (without the scheme@(guile-user)> prompts) in your .guile file. See Init File, for more on .guile.

  When I excute the expression, I got the error:
  While compiling expression:
  ERROR: no code for module (ice-9 readline)

  Later, I run the command:
  apt-get install readline-common libreadline6 libreadline6-dev

  And then, I got the same error.

  And then, I found that there isn't the file of readline.scm in /usr/local/share/guile/2.0/ice-9/.

  So I thought that the reason should be not installing the module.

  I found the source file in guile-2.0.11/guile-readline/ice-9.

  I thought the reason of no readline installed is lack of the readline dev file.

  And then, I reconfig the makefile, `./configure`, and `make` `make install`

  Finally, I can use this module.

# 3、2015-06-26
* How to install GNU Artanis

  <<
  guile-dbi：
  Get error when compile guile-dbi-2.1.5:
  /usr/bin/ld: cannot find -lz

  apt-cache search libz-dev
  result: zlib1g-dev - compression library - development

  apt-get install zlib1g-dev

  guile-dbi-2.1.5
        prefix:      /usr/local
        compiler:    gcc  -pthread -I/usr/local/include/guile/2.0  
        link:        -L/usr/local/lib -lguile-2.0 -lgc

  依赖：
  apt-get install g++

  install guile-2.0.11 in ubuntu of docker.
  apt-get install libunistring-dev
  apt-get install libffi-dev

  apt-get install libltdl-dev
  apt-get install pkg-config

  apt-get install libgc-dev

  apt-get install libgmp-dev
  >>

  full:
  apt-get install zlib1g-dev g++ libunistring-dev libffi-dev libltdl-dev pkg-config libgc-dev libgmp-dev

  本来想在其它目录编译安装，结果发现在编译artanis的时候，需要寻找guile的路径，我不知道怎么设置，所以最终还是得用默认的/usr/local

  所以接下来到其它环境安装，需要将/usr/local下的bin、lib、include、share等打包，然后解压到/usr/local目录

  -- 2015-06-27 update
  需要依赖的其它库包括：
  cp  /mnt/tools/guile-artanis/lib/libgc.so.1.0.3 .
  ln -s libgc.so.1.0.3 libgc.so.1

  cp /mnt/tools/guile-artanis/lib/libunistring.so.0.1.2 .
  ln -s libunistring.so.0.1.2 libunistring.so.0

  cp /mnt/tools/guile-artanis/lib/libgmp.so.10.1.3 .
  ln -s libgmp.so.10.1.3 libgmp.so.10

  cp /mnt/tools/guile-artanis/lib/libltdl.so.7.3.0 .
  ln -s libltdl.so.7.3.0 libltdl.so.7

  这些也一并放到/usr/local/lib下

  需要在/etc/profile中增加：
  export LD_LIBRARY_PATH=/usr/local/lib/

  或者：（用于docker）

  vi /etc/ld.so.conf
  <<
  /usr/local/lib/
  >>
  /sbin/ldconfig

  需要复制/etc/artanis配置文件

  接下来需要写一个安装脚本

# 2、2015-04-23
* [How to use and extend BiwaScheme](https://jcubic.wordpress.com/2010/05/17/how-to-use-biwascheme/)
* [BiwaScheme Blackboard](http://blackboard.biwascheme.org/)

  BiwaScheme Blackboard is a sandbox for BiwaScheme, a R6RS Scheme interpreter written in JavaScript.
  You can edit, run and save Scheme code in your browser.

* [WeScheme](http://www.wescheme.org/)

  WeScheme: The Browser is Your Programming Environment
  makes use of...

  [moby](http://github.com/dyoo/moby-scheme) : a Scheme compiler which allows Scheme programs to be run as "native" Android apps, or as javascript programs which run in a web browser.
  [AppEngine](http://appspot.com/) : Google's "cloud" of computing infrastructure. WeScheme uses AppEngine to handle user accounts, serverside compilation, and file management
  [ExCanvas](http://excanvas.sourceforge.net/) : Google's javascript library that provides a Canvas implementation on browsers which do not support it (IE).
  [Closure](http://code.google.com/closure/) : Google's javascript compiler, library and template system.
  [CodeMirror](http://codemirror.net/) : Marijn Haverbeke's library for in-browser editing.
  [EasyXDM](http://easyxdm.net/wp/) : a cross-domain messaging library.
  The source to WeScheme itself is [available for download](https://github.com/dyoo/WeScheme), under the [GNU LGPL](http://www.gnu.org/copyleft/) license.
  For more information about WeScheme, you can [watch the video](http://www.vidiowiki.com/watch/cydr9yk/#) or [read the paper](http://www.cs.brown.edu/~sk/Publications/Papers/Published/yskf-wescheme/).
  注：scheme的在线编写、运行环境，用google帐号登录，数据应该是保存到google的云存储上

# 1、2015-04-21
* [Scheme Working Groups](http://trac.sacrideo.us/wg/wiki#no1)
* [BiwaScheme](http://www.biwascheme.org/index.html)

  BiwaScheme is a Scheme interpreter written in JavaScript.
