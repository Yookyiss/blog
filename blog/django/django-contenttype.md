在某些场景下我们需要对一张表设置很多的同类型外键，例如现在有几张表，article，news，question三张表，我们根据产品需要，需要再为这三张表设置一个comment（评论表），普通的的策略是在comment中设置三个外键指向之前的三张表，这就引发了一些问题，
```
class Article(models.Model):
	body = models.CharField(max_length=255)
	
class News(models.Model):
	body = models.CharField(max_length=255)

class Question(models.Model):
	body = models.CharField(max_length=255)

class Commet(models.Model):
	article = models.ForeignKey(Article)
	news = models.ForeignKey(News)
	question = models.ForeignKey(Question)
	.....

会面临的重复造轮子，而且我们在具体的业务中叶需要再写逻辑去取外键所指的表
，具体操作起来会很麻烦

```
而django在我们初始创建项目时就为我们安装好了一个contenttype框架，此框架提供了很多的类，而且在数据库中自动生成的表 记载着我们每个app下都有哪些的model。这样我们可以利用的框架提供给我们的方便来进行操作。

```
from django.contrib.contenttypes.models import ContentType
from django.contrib.contenttypes.fields import GenericRelation, GenericForeignKey

class Comments(models.Model):
    pinglun = models.CharField(max_length=255)
    
    #此字段在数据表不存在,用于协助下面两个字段进行各类操作
    content_type = models.ForeignKey(ContentType, blank=True, null=True, on_delete=models.CASCADE)
    # 定位对象的id
    object_id = models.PositiveIntegerField(blank=True, null=True)
    # 外键所指对象的model 在django_content_type表中的id
    content_object = GenericForeignKey('content_type', 'object_id')

class Article(models.Model):
    body = models.CharField(max_length=255)
    #不会生成此字段，此字段用于协助操作
    comment = GenericRelation(Comments)

class News(models.Model):
    body = models.CharField(max_length=255)
    comment = GenericRelation(Comments)

```
这样我们就免去了重复造轮子的工作，而且在增删改查的逻辑方面也会节省很多代码。