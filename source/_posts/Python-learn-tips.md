---
title: Python-learn-tips
date: 2021-10-30 10:25:22
tags:
- Python
categories: 
- Python
cover: https://gitee.com/cd-yang/pic/raw/master/img/202110301042953.png
---

# *号

[Python中的*（星号）和**(双星号）完全详解_微鉴道长的博客-CSDN博客_python 星号](https://blog.csdn.net/zkk9527/article/details/88675129)

1\. nn.Sequential(\*layers)
---------------------------

> 类似于torch7中的Sequential，将每一个模块按照他们的顺序送入到nn.Sequential中 ,输入可以是一些列有顺序的模块

    conv1=nn.FractionalMaxPool2d(2,output_ratio=(scaled,scaled))
    conv2=nn.Conv2d(D,D,kernel_size= 3,stride=1,padding=1,bias=True)
    conv3=nn.Upsample(size=(inputRes,inputRes),mode='bilinear')
    return nn.Sequential(conv1,conv2,conv3)

> 输入也可以是一个orderDict

    # Example of using Sequential with OrderedDict
    model = nn.Sequential(OrderedDict([           #orderdict按照建造时候的顺序进行存储
              ('conv1', nn.Conv2d(1,20,5)),
              ('relu1', nn.ReLU()),
              ('conv2', nn.Conv2d(20,64,5)),
              ('relu2', nn.ReLU())
            ]))

orderdict示例

    #orderdict部分源代码来自https://www.cnblogs.com/gide/p/6370082.html，稍有改动
    import collections
    print "Regular dictionary"
    d={}
    d['a']='A'
    d['b']='B'
    d['c']='C'
    for k,v in d.items():
        print k,v
    
    print "\nOrder dictionary"
    d1 = collections.OrderedDict()
    d1['a'] = 'A'
    d1['b'] = 'B'
    d1['c'] = 'C'
    d1['2'] = '2'
    d1['1'] = '1'
    for k,v in d1.items():
        print k,v

![](https://img-blog.csdn.net/20180512204317537?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1NDg1Njg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

> 输入也可以是list,然后输入的时候用\*来引用

     layers = []
     layers.append(block(inplanes, outplanes,inputRes,baseWidth=9,cardinality=4,stride=1,preact=preact))
     return nn.Sequential(*layers)  # 不加*号，会报错 TypeError: list is not a Module subclass

因为从nn.Sequential的定义来看  
![](https://img-blog.csdn.net/20180512210046158?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1NDg1Njg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
输入要么事orderdict,要么事一系列的模型，遇到上述的list，必须用\*号进行转化

Sequential好处是啥呢？

        def _make_fc(self, inplanes, outplanes):
            bn = nn.BatchNorm2d(inplanes)
            conv = nn.Conv2d(inplanes, outplanes, kernel_size=1, bias=True)
            return nn.Sequential(
                    conv,
                    bn,
                    self.relu,
                )

在前传forward的时候，一步完成，而不需要conv,bn，relu在forward函数里都写一遍

2\. torch.nn.ModuleList
-----------------------

> 存储一系列模型的module list，操作类似于标准的python list

    class MyModule(nn.Module):
        def __init__(self):
            super(MyModule, self).__init__()
            self.linears = nn.ModuleList([nn.Linear(10, 10) for i in range(10)])
    
        def forward(self, x):
            # ModuleList can act as an iterable, or be indexed using ints
            for i, l in enumerate(self.linears):
                x = self.linears[i // 2](x) + l(x)
            return x

代码中的`[nn.Linear(10, 10) for i in range(10)]`是一个python列表，必须要把它转换成一个module list列表才可以被pytorch使用，否则在运行的时候会报错，什么错呢？`RuntimeError: Input type (CUDAFloatTensor) and weight type (CPUFloatTensor) should be the same`,如果这部分不转成modulelist（gpu）,那么只是pythonlist(cpu) 来承载这些模型，定造成cpu和gpu的冲突。

3 python \*号
------------

> 单个星号代表这个位置接收任意多个非关键字参数，并转化成元表。也就是_b 会接受除了a之外的剩下的非关键字参数，需要注意的是_加在形参面前代表的是收集参数，如果\*号加在了是实参上(例如第十四行)，代表的是将输入迭代器拆成一个个元素

    d1 = collections.OrderedDict()
    d1['a'] = 'A'
    d1['b'] = 'B'
    d1['c'] = 'C'
    d1['2'] = '2'
    d1['1'] = '1'
    
    def one(a,*b):
        print(b)
    def two(*b):
        print(b)
    c = [6,7,8,9]
    one(1,2,3,4,5,6)
    one(*c)    #传入实参的时候，加上*号，可以将列表中的元素拆成一个个的元素
    one(*d1)   #传入实参的时候，加上*号，可以将字典中的元素拆成一个个的元素
    one(c)
    one(d1)
    two(c)
    two(d1)


![](https://gitee.com/cd-yang/pic/raw/master/img/202110301037699.png)

> \*\* 双星号代表这个位置接收任意多个关键字参数，并按照关键字转化成字典  
> 用双星号传入实参的时候，一定是所有的实参必须带有关键字

    def three1(**b):
        print(b)
    
    three(a=1,b=2,c=3,d=4,e=5,f=6)

4\. torch CMul CAdd在pytorch中消失了，代以\* , +
----------------------------------------

`CMul` : component-wise multiplication

`CAdd`: component-wise addition



# [魔术方法](https://zhuanlan.zhihu.com/p/329962624)

## **什么是魔术方法？**

在Python中，所有以双下划线`__`包起来的方法，统称为**Magic Method（魔术方法）**，它是一种的特殊方法，普通方法需要调用，而魔术方法不需要调用就可以自动执行。

魔术方法在类或对象的某些事件出发后会自动执行，让类具有神奇的“魔力”。如果希望根据自己的程序定制自己特殊功能的类，那么就需要对这些方法进行重写。

Python中常用的运算符、for循环、以及类操作等都是运行在魔术方法之上的。

**魔术方法`__init__`、`__new__`、`__del__`的应用**

    class People(object):
        # 创建对象
        def __new__(cls, *args, **kwargs):
            print("触发了构造方法")
            ret = super().__new__(cls) # 调用父类的__new__()方法创建对象
            return ret ## 将对象返
        # 实例化对象
        def __init__(self, name, age):
            self.name = name
            self.age = age
            print("初始化方法")
        #  删除对象
        #   del 对象名或者程序执行结束之后
        def __del__(self):
            print("析构方法，删除对象")


​    
    if __name__ == '__main__':
        p1 = People('xiaoming', 16)
    
    输出：
    触发了构造方法
    初始化方法
    析构方法，删除对象

**使用`__call__`方法实现斐波那契数列**

    # 斐波那契数列指的是这样一个数列 0, 1, 1, 2, 3, 5, 8, 13
    # 特别指出：第0项是0，第1项是第一个1。从第三项开始，每一项都等于前两项之和。
    class Fib(object):
        def __init__(self):
            pass
        def __call__(self,num):
            a,b = 0,1;
            self.l=[]
    
            for i in range (num):
                self.l.append(a)
                a,b= b,a+b
            return self.l
        def __str__(self):
            return str(self.l)
        __rept__=__str__
    
    f = Fib()
    print(f(10))
    输出：
    [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

## 常用的魔术方法

### 1.初始化方法`__init__`

    触发机制：实例化对象之后立即触发
    参数：至少有一个self，接收当前对象，其他参数根据需要进行定义
    返回值：无
    作用：初始化对象的成员

### 2.构造方法`__new__`

    触发时机： 实例化对象时自动触发（在__init__之前触发）
    参数：至少一个cls 接收当前类，其他参数根据初始化方法参数决定
    返回值：必须返回一个对象实例，没有返回值，则实例化对象的结果为None
    作用：实例化对象
    注意：实例化对象是Object类底层实现，其他类继承了Object的__new__才能够实现实例化对象。

### 3.析构方法`__del__`

    触发时机：当该类对象被销毁时，自动触发
    参数：一个self，接受当前对象
    返回值：无
    作用：关闭或释放对象创建时资源
    注意：del不一定会触发当前方法，只有当前对象没有任何变量引用时才会触发

### 4.`__call__`

    调用对象的魔术方法
    触发时机:将对象当作函数调用时触发,方式： 对象()
    参数:至少一个self接收对象，其余根据调用时参数决定
    返回值：根据情况而定
    作用：可以将复杂的步骤进行合并操作，减少调用的步骤，方便使用
    注意：无

### 5.`__len__`

    触发时机：使用len(对象) 的时候触发
    参数：一个参数self
    返回值：必须是一个整型
    作用：可以设置为检测对象成员个数，但是也可以进行其他任意操作
    注意：返回值必须必须是整数，否则语法报错，另外该要求是格式要求。

### 6.`__str__`

    触发时机:使用print(对象)或者str(对象)的时候触发
    参数：一个self接收对象
    返回值：必须是字符串类型
    作用：print（对象时）进行操作，得到字符串，通常用于快捷操作
    注意：无

### 7.`__repr__`

    触发时机:在使用repr(对象)的时候触发
    参数：一个self接收对象
    返回值：必须是字符串
    作用：将对象转使用repr化为字符串时使用，也可以用于快捷操作

### 8.`__bool__`

    触发时机: 使用bool(对象)的时候触发
    参数：一个self接收对象
    返回值：必须是布尔值
    作用：根据实际情况决定，可以作为快捷方式使用
    注意:仅适合于返回布尔值的操作

### 9.`__format__`

    触发时机：使用字符串.format(对象)时候触发
    参数：一个self接收对象，一个参数接收format的{}中的格式，例如:>5
    返回值:必须是字符串
    作用：设置对象可以作为format的参数，并且自定义对象格式化的规则
    注意：无

## 与属性操作相关的魔术方法

* * *

### 1.`__getattr__`

    触发时机：获取不存在的对象成员时触发
    参数：一个是接收当前对象的self，一个是获取成员名称的字符串
    返回值：必须有值
    作用:为访问不存在的属性设置值
    注意：getattribute无论何时都会在getattr之前触发，触发了getattribute就不会在触发getattr了

### 2.`__setattr__`

    触发时机:设置对象成员值的时候触发
    参数:1个当前对象的self,一个是要设置的成员名称字符串,一个是要设置的值
    返回值:无 过程操作
    作用:接管设置操作,可以在设置前之前进行判断验证等行为
    注意:在当前方法中无法使用成员=值的方式直接设置成员，否则会无限递归，必须借助object的设置方法来完成
    
    object.__setattr__（参数1，参数2，参数3）

### 3.`__delattr__`

    触发时机：删除对象成员时触发
    参数：一个当前对象的self
    返回值：无
    作用:可以在删除成员时进行验证。

### 4.`__getattribute__`

    触发时机：使用对象成员时触发，无论成员是否存在
    参数：1个接收当前对象self，一个是获取的成员的名称字符串
    返回值：必须有
    作用：在具有封装操作（私有化时），为程序开部分访问权限使用

### 5.`__dir__`

    触发时机：dir（对象）的时候触发
    参数:1个接收当前对象self
    返回值：必须为序列类型（列表，元组，集合等，）
    作用：可以自定义成员列表的返回值

## 其他魔术方法

### 比较运算相关魔术方法

    __ lt__(self, other)：
    定义小于号的行为：x < y 调用 x.lt(y)
    __ le__(self, other)：
    定义小于等于号的行为：x <= y 调用 x.le(y)
    __ eq__(self, other) ：
    定义等于号的行为：x == y 调用 x.eq(y)
    __ ne__(self, other)：
    定义不等号的行为：x != y 调用 x.ne(y)
    __ gt__(self, other)：
    定义大于号的行为：x > y 调用 x.**gt(y)
    __ ge__(self, other) ：
    定义大于等于号的行为：x >= y 调用 x.ge(y)

### 算术运算相关魔术方法

    __add__(self, other)           定义加法的行为：+
    __sub__(self, other)           定义减法的行为：-
    __mul__(self, other)           定义乘法的行为：*
    __truediv__(self, other)       定义真除法的行为：/
    __floordiv__(self, other)      定义整数除法的行为：//
    __mod__(self, other)           定义取模算法的行为：%
    __divmod__(self, other)        定义当被 divmod() 调用时的行为
    __pow__(self, other[, modulo]) 定义当被 power() 调用或 ** 运算时的行为
    __lshift__(self, other)        定义按位左移位的行为：<<
    __rshift__(self, other)        定义按位右移位的行为：>>
    __and__(self, other)           定义按位与操作的行为：&
    __xor__(self, other)           定义按位异或操作的行为：^
    __or__(self, other)            定义按位或操作的行为：|

### 赋值运算相关魔术方法

    __iadd__(self, other)             定义赋值加法的行为：+=
    __isub__(self, other)             定义赋值减法的行为：-=
    __imul__(self, other)             定义赋值乘法的行为：=
    __itruediv__(self, other)         定义赋值真除法的行为：/=
    __ifloordiv__(self, other)        定义赋值整数除法的行为：//=
    __imod__(self, other)             定义赋值取模算法的行为：%=
    __ipow__(self, other[, modulo])   定义赋值幂运算的行为：**=
    __ilshift__(self, other)          定义赋值按位左移位的行为：<<=
    __irshift__(self, other)          定义赋值按位右移位的行为：>>=
    __iand__(self, other)             定义赋值按位与操作的行为：&=
    __ixor__(self, other)             定义赋值按位异或操作的行为：^=
    __ior__(self, other)              定义赋值按位或操作的行为：|=

### 一元运算相关魔术方法

    __pos__(self)      定义正号的行为：+x
    __neg__(self)      定义负号的行为：-x
    __abs__(self)      定义当被 abs() 调用时的行为
    __invert__(self)   定义按位求反的行为：~x

### 类型转换相关魔术方法

    __complex__(self)      定义当被 complex() 调用时的行为（需要返回恰当的值）
    __int__(self)          定义当被 int() 调用时的行为（需要返回恰当的值）
    __float__(self)        定义当被 float() 调用时的行为（需要返回恰当的值）
    __round__(self[, n])   定义当被 round() 调用时的行为（需要返回恰当的值）
    __index(self)__        1. 当对象是被应用在切片表达式中时，实现整形强制转换
                           2. 如果你定义了一个可能在切片时用到的定制的数值型,你应该定义 index
                           3. 如果 index 被定义，则 int 也需要被定义，且返回相同的值

### 上下文管理相关魔术方法(with)

`__enter__` 和 `__exit__`

    __enter__(self)
        1. 定义当使用 with 语句时的初始化行为
        2. enter 的返回值被 with 语句的目标或者 as 后的名字绑定
    
    __exit__(self, exctype, excvalue, traceback)
        1. 定义当一个代码块被执行或者终止后上下文管理器应该做什么
        2. 一般被用来处理异常，清除工作或者做一些代码块执行完毕之后的日常工作

### 容器类型相关魔术方法

    __len__(self)                  定义当被 len() 调用时的行为（返回容器中元素的个数）
    __getitem__(self, key)         定义获取容器中指定元素的行为，相当于 self[key]
    __setitem__(self, key, value)  定义设置容器中指定元素的行为，相当于 self[key] = value
    __delitem__(self, key)         定义删除容器中指定元素的行为，相当于 del self[key]
    __iter__(self)                 定义当迭代容器中的元素的行为
    __reversed__(self)             定义当被 reversed() 调用时的行为
    __contains__(self, item)       定义当使用成员测试运算符（in 或 not in）时的行为
    
    关于python魔术方法的知识掌握这么多基本就够用了，这里给大家推荐一个评价不错的python课程，希望对大家有所帮助。

仅限100名！3.9元入门python。游戏闯关式教学，小白也能轻松学会！

已失效 

点击跳转至第三方



