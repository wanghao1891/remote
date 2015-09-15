# 3、2015-06-23
* [Inside NGINX: How We Designed for Performance & Scale](http://nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/?from=timeline&isappinstalled=0#rd)

# 2、2015-06-05
* nginx启动报错

  getgrnam("nobody") failed

  发现/etc/passwd中有nobody用户，但是group中没有nobody组

  创建nobody组，并将nobody用户加入该组中
  groupadd nobody  
  gpasswd -a nobody nobody

# 1、2015-04-16
* [Nginx 战斗准备：优化指南](http://linux.cn/article-5265-weibo.html)
