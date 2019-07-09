## 看了很多网上的csrf机制的文章，百分之99解释的都是错误的。

**错误说法1：CsrfViewMiddleware中间件会把form表单的csrfmiddlewaretoken值与cookie的csrftoken字段值作比较，如果一样就表示合法。**

what？？？我试验了一下，我不断地刷新带有form表单的页面，发现cookie的csrftoken在一段时间内并不会变，而表单的csrfmiddlewaretoken会发生变化，而且他俩的值根本不相等，这表名同一个csrftoken可以跟不同的csrfmiddlewaretoken完成合法提交。

**错误说法2：django会把csrftoken的值存储到后台，当前台提交时，django会把这个值拿来跟表单中的值作比较，相同即合法。**

这更不可能，django中根本没有这张表的存在。


***个人觉得正确的说法是，CsrfViewMiddleware中间件会把form表单的csrfmiddlewaretoken值与cookie的csrftoken字段值送入一个计算函数，在函数中会对对两个值加salt进行hash计算（由于我没太过仔细的研究中间件源码，只能把大概思想说出来），用一些的算法做一些计算来得出是否合法。***

下面这篇文章的老哥说的不错，但是难度较大，可以细细品味
https://blog.csdn.net/qq_27952549/article/details/82392790
