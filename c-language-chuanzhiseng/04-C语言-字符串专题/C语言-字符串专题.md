# C语言——字符串专题




<h2 id="0">目录</h2>
<br/>

* [1　字符串的初始化](#1字符串的初始化)
    * [1.1　通过字符数组初始化字符串](#11通过字符数组初始化字符串)
    * [1.2　通过字符串常量初始化字符串](#12通过字符串常量初始化字符串)
* [2　字符串的打印输出](#2字符串的打印输出)
* [3　常用字符/字符串函数](#3常用字符字符串函数)
* [4　字符串作函数参数传递时的注意要点](#4字符串作函数参数传递时的注意要点)
* [5　字符串拷贝时的技术要点](#5字符串拷贝时的技术要点)

<br/>




## 1　字符串的初始化
[回到目录](#0)

- **C语言中没有字符串，字符串以'\0'作为字符串结尾符**
- **字符串的内存分配可在堆区、栈区、全局区上分配**


### 1.1　通过字符数组初始化字符串
[回到目录](#0)
```c
#include <stdio.h>

void main()
{
	char buf1[] = {'a', 'b', 'c', 'd', 'e', '\0'};   
	//自动分配数组长度：6  此种初始化字符串正确
	char buf2[] = {'a', 'b', 'c', 'd', 'e'};         
	//自动分配数组长度：5  此种初始化字符串错误
	
	char buf3[6] = {'a', 'b', 'c', 'd', 'e', '\0'};  
	char buf4[6] = {'a', 'b', 'c', 'd', 'e'}; 
	char buf5[5] = {'a', 'b', 'c', 'd', 'e'};   //此种初始化字符串错误 
	
	printf_s("buf1:%s   buf2:%s \n", buf1, buf2);
	printf_s("buf3:%s   buf4:%s   buf5:%s \n", buf3, buf4, buf5);

	return;
}

/*
在Microsoft Visual Studio中的运行结果是：
---------------------------------------------
buf1:abcde   buf2:abcde烫烫烫烫烫蘟bcde
buf3:abcde   buf4:abcde   buf5:abcde烫烫烫烫烫蘟bcde
---------------------------------------------
要点：
对于未指定字符数组长度情况，使用字符数组初始化字符串时，需加上'\0'，否则输出将出现乱码，
不加上'\0'的话，就算不上字符串，后续就无法使用字符串函数；
对于指定字符数组长度的情况，使用字符数组初始化字符串时，对于字符串结尾符'\0'，可加可不加，
但前提是字符数组的长度必须大于所需初始化字符串的长度，否则输出出现乱码。
故能成功初始化成字符串的有buf1,buf3,buf4，后续可使用各种字符串函数。
```

### 1.2　通过字符串常量初始化字符串
[回到目录](#0)

```c
#include <stdio.h>

void main()
{
	char buf1[] = "abcde";     //自动分配字符数组的长度：6
	char buf2[8] = "abcde";    //字符数组的长度至少为6
	char *buf3 = "abcde";
	printf_s("buf1:%s   buf2:%s   buf3:%s \n", buf1, buf2, buf3);

	return;
}

/*
在Microsoft Visual Studio中的运行结果是：
---------------------------------------------
buf1:abcde   buf2:abcde   buf3:abcde
---------------------------------------------
要点：
```

- **总结**：以上种种字符串初始化方式，建议使用以下两种`char buf1[] = "abcde"`和`char *buf3 = "abcde"`。

<br/>



## 2　字符串的打印输出

[回到目录](#0)
```c
#include <stdio.h>

void main()
{

    char buf[] = "abcde";              
    char * p = "abcde";

    int i = 0;
    printf_s("buf:");
    for (i=0; i<6; i++)   //打印方式1（逐个打印）
    {
        printf_s("%c", *(buf+i));
        //错误写法：   buf = buf + i;  printf_s("%c", *buf);
        //buf是一个常量指针不能修改，这样设计是保证析构内存时，buf所指向的连续内存空间安全释放
    }
    printf_s("\n");

    printf_s("p:");
    for (i=0; i<6; i++)   //打印方式2（逐个打印）
    {
        printf_s("%c", *(p+i));
        //这样写也是一种正确写法：（p并非常量指针，可进行改变）   
        //p = p +i;  printf_s("%c", *p);
    }
    printf_s("\n");

    printf_s("buf:%s \n", buf);  //打印方式3（整体打印）
    printf_s("p:%s \n", p);     //打印方式4（整体打印）

    return;
}
/*
在Microsoft Visual Studio中的运行结果是：
---------------------------------------------
buf:abcde
p:abcde
buf:abcde
p:abcde
---------------------------------------------
要点：字符串打印方式主要分为逐个打印和整体打印。推荐使用整体打印法
```

<br/>



## 3　常用字符/字符串函数

[回到目录](#0)
- **使用字符常用函数需添加前置声明：**`#include <ctype.h>`
- **使用字符串常用函数需添加前置声明：**`#include <string.h>`

**复制**
- **strcpy**  `char * strcpy ( char * destination, const char * source )`
- **strncpy**  `char * strncpy ( char * destination, const char * source, size_t num )`

**连接**
- **strcat**  `char * strcat ( char * destination, const char * source )`
- **strncat**  `char * strncat ( char * destination, char * source, size_t num )`

**比较**
- **strcmp**  `int strcmp ( const char * str1, const char * str2 )`
- **strnncmp**  `int strncmp ( const char * str1, const char * str2, size_t num )`

**搜索**
- **strchr**  `char * strchr ( char * str, int character )`
- **strrchr**  `char * strrchr ( char * str, int character )`
- **strpbrk**  `char * strpbrk ( char * str1, const char * str2 )`
- **strcspn**  `size_t strcspn ( const char * str1, const char * str2 )`
- **strspn**  `size_t strspn ( const char * str1, const char * str2 )`
- **strstr**  `char * strstr ( char * str1, const char * str2 );`
- **strtok**  `char * strtok ( char * str, const char * delimiters );`

**其他**
- **strlen**  `size_t strlen ( const char * str );`

**以上函数的具体使用说明要点见C语言函数手册**

<br/>



## 4　字符串作函数参数传递时的注意要点

[回到目录](#0)
我们以实现字符串拷贝为例，来说明字符串作函数参数传递时需注意的要点。
```c
#include <stdio.h>

int copy_str(char *from, char *to)
{
	//注意要点1：函数调用时形参接收的值别随意改变
	char  *tempfrom, *tempto;
	int ret = 0;

	//注意要点2：用来防止随意改写不能改写的空间
	if (from == NULL || to == NULL)
	{
		ret = -1;
		return ret;
	}
	//设定两个内部缓冲指针变量tempfrom, tempto用以接收函数形参from, to的值
	tempfrom = from;
	tempto = to;
	while (*tempto++ = *tempfrom++)  
	{}
	
	printf_s("from:%s     to:%s\n", from, to);

	return ret;
}

void main()
{
	char *from = "abcde";
	char to[8];
	int ret;

	ret = copy_str(from, to);
	if (ret != 0)
	{
		printf_s("function copy_str() error\n");
	}
	printf_s("from:%s     to:%s\n", from, to);

	return;
}

/*
在Microsoft Visual Studio中的运行结果是：
---------------------------------------------
from:abcde     to:abcde
from:abcde     to:abcde
---------------------------------------------
要点：以上两个注意要点指得牢记
```
- **关于注意要点1**  
　　对于函数形参from, to的所接收的值需尽量别去随便改变，而是用两个内部缓冲指针变量tempfrom, tempto用以接收函数形参from, to的值。如果直接用函数形参from, to直接进行操作，其指针指向在一系列操作后早已改变，在子函数最后字符串打印输出`printf_s("from:%s     to:%s\n", from, to)`将无法准确打印输出字符串。故函数形参所接收的值别去随便改变，其所需操作通过函数内部缓冲变量用以接收形参值代替其操作。这是为了防止在子函数内部其他一些场合需要用到函数形参的初始接收值时而无法使用的情况。

- **关于注意要点2**  
　　对于字符串作函数参数传递时，其传递的一般是字符串首个字符的地址`copy_str(from, to)`，然而在一些特殊情况下脑残传递了类似NULL此类受保护（不允许改写）的内存空间给形参去函数内部操作`copy_str(NULL, to)`，将会出现程序宕机的情况。为了避免此类情况出现程序宕机，在操作函数内部需加入此类预防机制。
```c
//子函数部分
if (from == NULL || to == NULL)
{
	ret = -1;
	return ret;   //终止函数进一步操作并返回错误报告
}

//main函数部分
ret = copy_str(NULL, to);
if (ret != 0)
{
	printf_s("function copy_str() error\n");
}
```

<br/>



## 5　字符串拷贝时的技术要点

[回到目录](#0)  
对于前一部分字符串作函数参数传递时，子函数内实现了字符串拷贝，那么我们对字符串拷贝的技术要点进行讲解一下。字符串结束的标志符是'\0'，那么我们进行字符串拷贝时拷贝到哪里结束，必然以检测到字符串结尾标志符'\0'为标准的。比如：

```c
#include <stdio.h>

void copy_str01(char *from, char *to)
{
	while (*from != '\0')
	{
		*to = *from;
		from++;
		to++;
	}
	//循环结束时，from指向字符串"abcde"结尾标志符'\0'
	*to = *from;   //循环结束检测到字符串结尾标志符'\0'，手动添加字符串结束符

	return;
}

void main()
{
	char *from = "abcde";
	char to[8];

	copy_str01(from, to);
	printf_s("from:%s   to:%s \n", from, to);

	return;
}

/*
在Microsoft Visual Studio中的运行结果是：
---------------------------------------------
from:abcde   to:abcde
---------------------------------------------
要点：
```
如以上程序所示，我们检测到字符串结尾标志符'\0'时需进行手动添加。然而，我们是否可以**免去手动添加**这个过程呢？当然有！前面我们之所以需要手动添加，原因在于我们先检测所拷贝的字符是否为'\0'，然后符合要求后进行拷贝。然而我们先进行拷贝再检测所拷贝的字符是否为'\0'呢？
```c
void copy_str02(char *from, char *to)
{
	while ( (*to=*from) != '\0' )
	{
		from++;
		to++;
	}
	//循环结束时，from指向字符串"abcde"结尾标志符'\0'

	return;
}
```
将代码进一步精简压缩
```c
void copy_str03(char *from, char *to)
{
	while ( (*to++=*from++) != '\0' )
	{
		/*括号内操作等价如下：
		*to = *from;
		from++;
		to++;
		判断表达式"*to = *from"是否为零*/
	}
	//循环结束时，from指向字符串"abcde"结尾标志符'\0'之后的一个字节空间的地址
	//即此处的from的值比前一种方法的from的值大1，这点一定要注意

	return;
}
```
还可以将代码再进一步精简压缩
```c
void copy_str04(char *from, char *to)
{
	while ( *to++ = *from++ )
	{}
	//循环结束时，from指向字符串"abcde"结尾标志符'\0'之后的一个字节空间的地址
	//即此处的方法与前一种方法效果相同
	
	return;
}
```


