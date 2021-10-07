---
title: CPP-Array
date: 2021-10-07 23:46:52
tags: 
- Array
- C++
category: C++
cover: https://gitee.com/cd-yang/pic/raw/master/img/202110080100485.png
---

# 数组的声明

​		C++中，最经典的数组声明方法是，`type name [elements]`。例如：

```cpp
int foo [5];
```

# 数组的初始化

> 在局部作用域下（函数中）声明的数组一般是不会进行初始化操作的。

​		初始化的几种方式，直接上代码。

## 初始化数组中的每一个值

```cpp
int foo [5] = { 16, 2, 77, 40, 12071 };
```

![img](https://www.cplusplus.com/doc/tutorial/arrays/arrays2.png)

## 初始化数组的一部分

```cpp
int bar [5] = { 10, 20, 30 };
```

![img](https://www.cplusplus.com/doc/tutorial/arrays/arrays3.png)



## 默认值初始化

```cpp
int baz [5] = { }; 
```

![img](https://www.cplusplus.com/doc/tutorial/arrays/arrays4.png)

## 不必声明数组的长度的情况

​		越来越简洁。

```cpp
int foo[] = { 10, 20, 30 };
int foo[] { 10, 20, 30 }; 
```

# 字符串数组

​		C++中的string类已经非常好用了，但是字符串在本质上来讲，就是字符的序列，直接使用字符串数组来存储自由度更大（性能，操作方式等）。

​		C++在字符串数组中规定了一种公约，即在字符串结束后紧随一个'\0'用来标定字符串的结束。我认为一个是结束的标志，另一个也是维持字符串本身意义所需，字符串不想数字的序列有着它本身的意义，用结束符来标定比较具有完备性，自我的一点想法，设计者是不是这么考量的有待考证。

## 字符串数组的初始化

```cpp
char myword[] = { 'H', 'e', 'l', 'l', 'o', '\0' };
char myword[] = "Hello"; 
```

​		==需要注意的是字符串数组被初始化完成后，上面的操作不可以用来改变`myword`的值了！！==

```cpp
myword = "Bye";
myword[] = "Bye"; 
```

​		都是不允许的,但是可以像下面这样的操作。

```cpp
myword[0] = 'B';
myword[1] = 'y';
myword[2] = 'e';
myword[3] = '\0';
```

## string和c_string

```cpp
char myntcs[] = "some text";
string mystring = myntcs;  // convert c-string to string
cout << mystring;          // printed as a library string
cout << mystring.c_str();  // printed as a c-string
```

# 参考

https://www.cplusplus.com/doc/tutorial/arrays/（摘录）
