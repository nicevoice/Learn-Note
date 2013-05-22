PYTHON学习笔记
======================

一些基础
-----------------

* 运算符号:`2**3`, `pow(2, 3)`表示乘方`2的2次方`  
* `repr(1000L)`会创建一个字符串以合法的方式来表示值  
* `input()`和`raw_input()`前者会解析用户的输入表达式,而后者不会,提倡后者  
* `\`可以控制换行,但是对于目录就不好用了,加入`r`前缀可以生成原始字符串  
* python内部是用8位的ASCII码存储的, 如果需要Unicode码的话加字符前缀`u`  


列表和元组
-----------------

**列表的元组的区别就是"列表可以修改,元组不可以"**

* 列表
```python
    user = ['Rogee', 28, 'Engineer', 'Loving']
```

* 元组  
```python
    user = ('rogee', 28, 'Engineer', 'Loging')
```

### 列表的方法  
* 索引: 同数组一样从**0**开始索引, 如果索引值为负数,那么从最后一位开始索引  
* 分片, `user[2:3:4]`, 这个不多说了, 第三个参数表示的是步长,步长为负数表示从右向左提取  
* 检查一个值在不在列表中,需要使用关键字`in`,返回一个`boolean`, 如果列表是二维的,那么需要匹配完整的一个一维序列才可返回true  
```python
'p' in 'python' # return true
```
* `len`, `max`, `min`是三个方便的对序列进行查看的函数  
* `del` 用于删除序列中的一个元素, `del user[2]`  
* `list` 函数可以根据字符串来创建一个列表  
```python
list('Rogee') #['R', 'o', 'g', 'e', 'e']
```
* 分片赋值是一个强大的特性, 同时也可以使用与原序列不相等的长度将分片替换, 如果使用空值的话,那么就是删除元素  
```python
name = [1,2,3,4,5,6]
name[4:] = list('ab')
print(name) #[1,2,3,4,a,b]
```
* 列表中的一些方法:`append`, `count`, `extend`, `index`, `insert`, `pop`, `remove`, `reverse`, `sort`  

### 元组:不可变的序列
* 元组的生成有些奇特, 那就是必须在值后加一个逗号  
* `tuple` 与列表的`list` 方法相似, 生成一个元组  
* 元组一般是作为返回的值存在的  

### 字典:当索引不好用时  

**字典类似于JSON**
* 字典中的键是`唯一`的  
* `dict(items)`可以通过映射来建立字典  
```python
items = [('a', 1), ('b', 2)]
d = dict(items)
# {'a':1, 'b':2}
```  
* 字典中的格式化字符串,使用圆括号括起来,再加上其它说明元素  
```python
phonebook = {'rogee': 123, 'jim': 456, 'green':789}
format = 'jim\'s phone is %(jim)s, please call him'
print(format % phonebook)
```
* `dict.clear()`清空一个字典  
* `dict.copy()`实现一个字典的浅复制, 如果需要深度复制可以使用`deepcopy(dic)`  
* `{}.fromkeys(['name', 'age'])`, 生成一个空的字典, {'name':None, 'age':None}
* `dict.get(key[, replace])`, 可以实现不报错的情况下访问一个值, replace可以返回替换的返回值  
* `dict.has_key('key')`, 可以确定是否含有一个键值  
* 迭代器`items`与`iteritems`和`keys`与`iterkeys` 和 `values`与`itervalues`  
* `dict.pop('key')`, `dict.popitem()`, 实现对字典中键值对的弹出
* `dict.setdefault('key', defaultvalue)`, 当存在key时返回键值, 当不存在键时, 返回default并更新键值为default值  
* `dict.update(dict2)`, 利用dict2中的键值来更新dict1中的键值  

## 字符串  
```python
format = 'helo, %s .%s enoough for yar??'
values = ('world', 'hot') #只有元组或字典可以在这里表示
print format % values
```
* `string.find(a, [start, [end] ])` 返回查找到的0开始的最左值, 没找到返回 `-1`, 接收查找起点与终点配置    
* `seperator.join(list)`可以使用`seperator`连接`list`形成一个字符串,是`string.split(seprator)`的逆方法  
* `string.lower()`转化小写  
* `string.title()`首字母大写  
* `string.replace(a, b)`, 把所有的a值替换为b值  
* `string.strip([a])`去除空格或者a值中的元素  
* `string.translate(table)`在使用这个函数前需要先使用`string`包里的`maketrans(a, b)`来生成一个字符转化table, 虽然麻烦, 但是这个比`replace`快  


## 语句   

* print  
```python
print(a,b,c)
```
* import  
```python
import string
from string import split, title, replace
import string as string1
from string import split as split1
```
* 赋值  
```python
x, y, z = 1, 2, 3 #多个赋值
x, y = y, x #交换

# 中转赋值
value = 1,2,3
x, y , z = value

#字典赋值
dict = {'name':123, 'age':123}
a = dict.popitem();
key, value = a
```

* boolean 除了下面的几个都是true, `False, None, 0, "", (), [], {}`  
* if语句
```python
if name.endswith('abc'):
    print 'hello'
elif num>0:
    print 'world'
else:
    print 'fuck'
```
* `if`除了普通的判断还有`is`, `is not`, `in`, `not in`  
* 断言`assets`,会引起崩溃, 但是这对早期程序设计是有帮助的  
* 循环
```python
#while
while true:
    print '1'

# for 
for word in words
    print word

for key, value in dict.items()
    print key, value
# 列表推导式
[x*x for x in range(10)]ｘ
[(x, y) for x in range(3) for y in range (3)]
```

## 抽象  
* `callable(function)`可以判定函数是否可以调用, **3.0**后不可用, 需要使用`hasattr(func, __call__)`来替代     
* 函数的样子如下, 使用`function.__doc__`来查看函数的文档   
```python
def hello(name):
    "here is the doc of the function"
    return 'hello' + name
```
* 关键字收集  
```python
def show(x, y, z,*param, **key):
    print(x,y,x)
    print param
    print key
# *param用于收集值参数, **用于收集关键字参数
show(1,2,3,4,5,6,a=1,b=4,c=4)
```
* 关键字收集的反转过程  
```python
def show_1(name, age):
    print '%s is %d years old'%(name, age)

user = ('rogee', 12)
show_1(*user)

def show_2(name, age):
    print '%s is %d years old'%(name, age)

info = {'name':'rogee', 'age':12}
show_2(**info)
```
* 
