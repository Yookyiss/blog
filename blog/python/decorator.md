北京今天天儿不错，在家写点博客吧。。。。
常用的装饰器如下

```
from functools import wraps
def decorator(func):
	"""
	使用系统内置的warps装饰器能够使被装饰的函数保留他原始的信息，
	直观一点就是在你print(func.__name__)的时候这个函数的名字仍然是func而不是new_func.
	"""
	@warps 
	def new_func(*args,**kwargs):
		print('添加装饰')
		return func(*args,**kwargs)
	return new_func
"""
只需在需要装饰的函数前@你写好的装饰器即可，装饰器格式如上
"""
@decorator
def func():
	print('原函数')
```
但是有的时候我们需要更加多样功能的装饰器，譬如可以根据接收参数不同而进行不同的装饰，举个栗子。
```
@decorator(method='get')
def func():
	pass
```
这就是一个典型的可以接收参数的装饰器，那我们如何实现的，看代码。
```
def decorator(method):
	def decorat(func):
		def new_func(*args,**kwargs):
			if method == "get":
				print('进行get装饰')
				return func(*args,**kwargs)
			elif method == "post":
				print('进行post装饰')
				return func(*args,**kwargs)
			else:
				method_copy = method				
				print('进行其他类型装饰'+method_copy[0:1])
				return func(*args,**kwargs)
		return new_func
	return decorator

@decorator(method='get')
def func():
	pass
"""
这样就能根据不同的参数进行不同的装饰啦，但是有一点需要注意，如果需要对传入的型参进行更改,
那么就需要从新再新建一个变量来继承传入型参的值，在此装饰器中就是method_copy
"""
				
```
睡了，睡了，雯雯的脚还受伤了，难受。
