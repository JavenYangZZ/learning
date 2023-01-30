#python教程
##字符串与编码
1.记事本存储逻辑
![记事本存储逻辑](https://www.liaoxuefeng.com/files/attachments/923923787018816/0)
2.浏览器存储逻辑
![浏览器存储逻辑](https://www.liaoxuefeng.com/files/attachments/923923759189600/0)
3.对于单个字符：Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符：
```
ord('A')
65
ord('中')
20013
chr(66)
'B'
chr(25991)
'文'
```
4.对于多个字符：关于decode&encode  

由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes[^notes]。
[^notes]:要注意区分'ABC'和b'ABC'，前者是str，后者虽然内容显示得和前者一样，但bytes的每个字符都只占用一个字节。

以Unicode表示的str通过encode()方法可以编码为指定的bytes
反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes。要把bytes变为str，就需要用decode()方法：
编写的应该加上
```
# -*- coding: utf-8 -*-
```
5.字符串格式化
*   方法1：占位符

占位符|替换内容
:---:|:---:|
%d	|整数
%f	|浮点数
%s	|字符串
%x	|十六进制整数
如果你不太确定应该用什么，%s永远起作用，它会把任何数据类型转换为字符串：
*   方法2：f
```
r = 2.5
s = 3.14 * r ** 2
print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62
```
##list[]
*   append
```
classmates.append('Adam')
['Michael', 'Bob', 'Tracy', 'Adam']
```
*   insert（i） i是索引位置
```
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```
*   pop（i）  i是索引位置
```
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```
*   注：list可以嵌套

##tuple()
tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！
```
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```
##dict{}
*   判断key是否存在：
    >一是通过in判断key是否存在：
        ```
        'Thomas' in d
        ```

    >二是通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value：
        ```
        d.get('Thomas')
        d.get('Thomas', -1)
        -1
        ```
    

*   要删除一个key，用pop(key)方法，对应的value也会从dict中删除：
    > 
    ```
    d.pop('Bob')
    75
    d
    {'Michael': 95, 'Tracy': 85}
    ```
##set()
set
set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。
要创建一个set，需要提供一个list作为输入集合：
*   可以去重
```
s = set([1, 1, 2, 2, 3, 3])
s
{1, 2, 3}
```
* addkey()
通过add(key)方法可以添加元素到set中，可以重复添加，但不会有效果
通过remove(key)方法可以删除元素：
* 交集& 并集|
set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：


##函数
*   函数可以返回多个值，但是返回的是一个tuple
*   位置参数：
    ```
    def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
    ```
    定义默认参数要牢记一点：默认参数必须指向不变对象！不然会出现函数记忆上一个默认参数的情况。
*   可变参数：前面加*
    ```
    def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
    ```
    直接传入多参数可以
    
    ``` 
    calc(1, 2)
    5
    calc()
    0
    ```
    直接传入列表或者元组也可
    ``` 
    nums = [1, 2, 3]
    calc(*nums)
    14
*   关键字参数
可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。
而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。请看示例：
    ```
    def person(name, age, **kw):
        print('name:', name, 'age:', age, 'other:', kw)
    ```
    +    直接传入会组装成字典
     ```
    person('Adam', 45, gender='M', job='Engineer')
    name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
    ```
    +   直接传入字典会自动遍历，注意kw获得的dict是extra的一份拷贝。
    ```
    extra = {'city': 'Beijing', 'job': 'Engineer'}
    person('Jack', 24, **extra)
    name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
    ```
    +   函数关键字内部检查,但不会限制传入的参数。
    ```
    def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    print('name:', name, 'age:', age, 'other:', kw)
    ```
    +   如果要限制关键字参数的名字，就可以用命名关键字参数,命名关键字参数可以缺省。
        -   直接用*分隔开
            ```
            def person(name, age, *, city, job):
            print(name, age, city, job)
             ```    
        -   如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了：
            ```
            def person(name, age, *args, city, job):
            print(name, age, args, city, job)
            ```
    *   *args是可变参数，args接收的是一个tuple；
        **kw是关键字参数，kw接收的是一个dict。
##  递归函数
```
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```
*   尾递归调用时，如果做了优化，栈不会增长，因此，无论多少次调用也不会导致栈溢出。
##  切片
*   每几个数取数
    ```前10个数字每隔2个取一个
    L[:10:2]
    [0, 2, 4, 6, 8]
    ```
    所有数每隔五个取一个
    ```
     L[::5]
    ```

##  迭代
*   from collections.abc import Iterable用来判断对象是否可以迭代
    isinstance(x,Iterable)
*   默认情况下，dict迭代的是key。
    -   如果要迭代value，可以用for value in d.values()
    -   如果要同时迭代key和value，可以用for k, v in d.items() 
*   如果要对list实现的下标循环,返回的是0，'A'这样的
    ```
    for i, value in enumerate(['A', 'B', 'C']):
    print(i, value)
    ```

##  列表生成式
*   例子：
    ```
    [x * x for x in range(1, 11) if x % 2 == 0]
    [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    ```
*   全部迭代生成全排列
    ```
    [m + n for m in 'ABC' for n in 'XYZ']
    ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
    ```
*   列表生成式也可以多个变量迭代
    ```
    d = {'x': 'A', 'y': 'B', 'z': 'C' }
    [k + '=' + v for k, v in d.items()]
    ['y=B', 'x=A', 'z=C']
    ```
##  生成器，可迭代对象，是iterator也是iterable。

*   方法1：将列表生成式改为括号,用next获取。
    ```
     g = (x * x for x in range(10))
     ```
*   方法2：迭代器函数，包含yield
    ```
    def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
    ```
    变成generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。
    - 每次调用返回一个迭代器对象
    -   ```
        g = odd()
        next(g)
        next(g)
         ```
    -   而不应这么写
        ```
        next(odd())
        next(odd())
        ```
    -   要想获取返回值，请捕获StopIteration错误
  
##  迭代器
*   字典列表字符串是iterable但不是iterator，
    list、dict、str等Iterable变成Iterator可以使用iter(对象)函数：
*   生成器既是iterable也是是iterator，
*   凡是可作用于for循环的对象都是Iterable类型；
凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列
##  面向函数编程
*   高阶函数
    函数本身也可以赋值给变量，即：变量可以指向函数。函数的参数能接收变量。
    高阶函数，就是让函数的参数能够接收别的函数。