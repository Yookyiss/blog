今天室友部署部署项目，使用nginx做反向代理，遇到了静态文件加载不出的问题，让我帮他解决，通过查看日志·
```
2019/05/24 20:04:15 [error] 6943#6943: *14 open() 
"/root/object/YYW/travel/static/images/yyw.jpg" failed
 (13: Permission denied), client: 1.62.202.xx, 
 server: 39.106.39.xxx, request: "GET /static/images/yyw.jpg HTTP/1.1",
  host: "39.106.39.xxx", referrer: "http://39.106.39.xxx/yyw/"
```
感觉是nginx的权限太小。
解决如下：
unbuntu系统下：
在/etc/nginx/nginx.conf文件中的www-data改为root,赋予nginx管理员的权限，然后nginx -s -reload,问题解决。

nginx -s reload 意为加载配置文件重启nginx。

插曲：一般nginx出现问题通过查看日志都能解决，日志位置在/var/log/nginx 下。
