今天在mac上使用navicat连接mysql报错弄了一下午，各种查询踩坑，总算解决了。
即从mysql5.7版本之后，默认采用了caching_sha2_password验证方式，我用的mysql8.0，于是就遇到了这个问题。
解决办法：
mysql>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root账户密码';  意为使用旧的认证方式。