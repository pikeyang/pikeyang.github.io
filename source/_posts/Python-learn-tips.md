---
title: Python-learn-tips
date: 2021-10-30 10:25:22
tags:
- Python
categories: 
- Python
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
