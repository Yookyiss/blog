今天在django下使用celery，为了配置方便，直接使用了djcelery，写了一些定时任务也没问题，不过运到一个比较奇怪的现象，就是我在配置文件中把定时任务注释掉了，他还是会执行，通过查看文档还有其他途径，发现djcelery是先把配置文件中的定时任务写入数据库，在运行celery时，他会直接去数据库中拿，而不是找配置文件。把配置文件注释掉，djcelery会认为没有对这个定时任务做修改，所以数据库的任务信息也不会变。

官方文档应该是有对于取消某个定时任务的解释，不过我还没有找到，所以只能在数据库相应的表中更改，如果想取消某个定时任务的解释，在djcelery_periodicttask这张记录着任务信息的表中，把相应的任务中的enabled设为0即可。

我用的启动方式是：
python manage.py celery beat --loglevel=info