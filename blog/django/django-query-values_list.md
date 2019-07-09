values()方法返回包含字典的QuerySet：
<QuerySet [{'comment': 1}, {'comment': 2}]>

该values_list()方法返回一个包含元组的QuerySet：
<QuerySet [(1,), (2,)]>
如果你使用values_list()单个字段，则可以使用flat=True返回单个值的QuerySet而不是1个元组：
<QuerySet [1, 2]>
