---
title: prime number
date: 2023-06-04 00:47:30
tags: Python, Algorithm
categories: Algorithm
cover: https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/1665918397.jpg
---

# Miller Rabin 

Miller Rabin素性检验是一种素数判定的法则，由CMU的教授Miller首次提出，并由希大的Rabin教授作出修改，变成了今天竞赛人广泛使用的一种算法，故称Miller Rabin素性检验。

该算法本质上是一种随机化算法，能在 $O(k\log_3 n)$ 的时间复杂度下快速判断出一个数是否是素数，但具有一定的错误概率。不过在一定数据范围内，通过一些技巧可以使其不出错。

## 原理

以下是Miller-Rabin算法的详细解析：

1. 输入：待判断的数n和测试次数k。
2. 如果n是2或3，直接返回True，因为2和3是素数。
3. 如果n是偶数或小于2，直接返回False。
4. 将n-1分解为2^s * d，其中d是一个奇数。
5. 进行k次测试： a. 选择一个随机数a，满足1 < a < n-1。 b. 计算x = a^d mod n。 c. 如果x等于1或x等于n-1，进入下一次测试。 d. 重复s次：
   - 计算x = x^2 mod n。
   - 如果x等于1，返回False，n是合数。
   - 如果x等于n-1，进入下一次测试。 e. 返回False，n是合数。
6. 返回True，n可能是素数。

Miller-Rabin算法的核心思想是进行多次随机测试，每次测试都会选择一个不同的随机数a，并检查a的幂次对n取模的结果。如果其中一次测试表明n是合数，那么n就肯定是合数。如果所有的测试都通过，那么n很有可能是素数，但并不是确定的。

需要注意的是，增加测试次数k可以提高Miller-Rabin算法的准确性，但同时也增加了运行时间。



## 代码

```python
import random

def is_prime(n, k=20):
    """判断一个整数是否是素数"""
    if n < 2:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False
    
    # 将n-1分解为2^s * d的形式
    s = 0
    d = n - 1
    while d % 2 == 0:
        s += 1
        d //= 2
    
    # 进行k次测试
    for i in range(k):
        a = random.randint(2, n-2) # 在[2, n-2]范围内选择一个随机整数a
        x = pow(a, d, n) # 计算a^d mod n的值
        if x == 1 or x == n-1: # 如果x=1或x=n-1，继续下一次测试
            continue
        for j in range(s-1):
            x = pow(x, 2, n) # 计算x^2 mod n的值
            if x == n-1: # 如果x=n-1，继续下一次测试,出现一个n-1，后面都是1，直接跳出
                break
        else: # 如果所有测试都没有退出，n不是素数，返回False
            return False
    
    return True # 所有测试都通过，n可能是素数，返回True

```



![image-20230604134732737](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/image-20230604134732737.png)



## 参考

[【朝夕的ACM笔记】数论-Miller Rabin素数判定 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/349360074)
