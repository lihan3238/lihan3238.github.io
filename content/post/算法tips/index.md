---
title: 算法学习tips
description: 记录算法学习中的一些tips
slug: algorithm_tips
date: 2024-03-26 00:00:00+0800
image: imgs/avatar.png
categories:
    - techStudy
tags:
    - tech
    - C++
    - algorithm
# math: 
# license: 
hidden: false
comments: true

draft: false

links:
  - title: C++算法基础学习
    description: C++算法基础学习,为蓝桥杯什么的程序竞赛学点基础算法
    website: https://lihan3238.github.io/p/cpp_algorithm_basic_study/
    image: cpp.png

---

## 速度优化

- 1. cin好像比scanf慢
- 2. memset(f, -1, sizeof(int)*41); 用于初始化数组

```cpp
void *memset(void *s, int ch, size_t n);
```

- 3. 大问题拆成小问题，可以通过提前把小问题分别算出来，最后再合成大问题的解，相比直接将每次的大问题拆成小问题计算更好

![1](imgs/1.png)

```cpp

#include <iostream>
#include <algorithm>
using namespace std;

int a[200005];
long long sum[200005];
long long sum1[200005];
long long sum2[200005];

int main()
{
	int n;
	long long min=0,max=0;
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		scanf("%lld",&a[i]);
		sum[i]=sum[i-1]+a[i];
	}
	min=0;
	sum1[1]=min;
	for(int i=2;i<=n;i++)
	{
		if(sum[i-1]<min)
			min=sum[i-1];
		sum1[i]=min;
	//	cout<<"min"<<min;
	}
	
	max=sum[n];
	sum2[n]=max;
	for(int i=n-1;i>=1;i--)
	{	
		if(sum[i]>max)
			max=sum[i];
		sum2[i]=max;
	//	cout<<"max"<<max; 
	}
	
	for(int i=1;i<=n;i++)
	{
		printf("%lld ",sum2[i]-sum1[i]);
	}
	
	return 0;
 } 

 
 

```

- 4. 记忆化搜索:搜索时创建数组或者其他数据结构，用来存储已经计算过的结果或已经搜索过的，避免重复计算

## 崩溃bug

- 1. 有+1 -1 数组大小记得多设点数组大小，别g了，不差那点
- 2. 大数组的定义，不要定义在main函数内部，会导致栈溢出
- 3. 二分查找——二分答案很多时候题目会让你去求解一个 ans ，这个 ans 有以下一些特征。比如求一个满足条件 A 的最大值/最 小值；或者求一个最大的最小值/最小的最大值，并且我们直接求解答案 A 非常困难，并且 ans 满足单调性，那么我们不妨直接二分查找最后的答案，测试它是不是满足条件。这种直接二分最后答案的算法就叫二分答案 。 

![2](imgs/2.png)

```cpp

#include <iostream>
#include <algorithm>
using namespace std;

int xp[1005][1005];
bool xpp[1005][1005];
int p[6]={0,-1,0,1,0};
int q[6]={0,0,-1,0,1};
int m,n;
void func(int y,int x);
int main()
{
	int f=0;
	char t;
	scanf("%d %d",&n,&m);
	getchar();
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=m;j++)
		{
			t=getchar();
			if(t=='*')
			{
				xp[i][j]=1;
			}	
			else
			{
				xp[i][j]=0;				
			}	
		}	
		getchar();	
	}
	
	
	func(0,0);
	
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=m;j++)
		{
			if(xp[i][j]==1&&xpp[i][j]!=true)
			{
				printf("*");
			}	
			else
			{
				printf(".");		
			}	
		}
		printf("\n");		
	}	
	
	
	return 0;
 } 
 
 void func(int y,int x)
 {
	if(x<0||y<0||x>m+1||y>n+1||xpp[y][x]==true)
	{
		return;
	}
 	if(xp[y][x]==1)
 	{
 		xpp[y][x]=true;
 		return;
	 }
	 xpp[y][x]=true;

	 for(int i=1;i<=4;i++)
	 {
	 	func(y+p[i],x+q[i]);

	 }
	return;
 }
```

## 代码模板

### 1.随机数

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>

using namespace std;

int main() {
	int a=1000;
	srand(time(0));
	while(a--)
	{
	cout<<rand()%10<<endl;//生成0-9的随机数
	 } 

    return 0;
}

```

### 字符串处理

```cpp
#include <string>

//字符串构造

string s1 = "hello";
string s2 = s1; // s2 = "hello"
string s2(10, 'c'); // s2 = "cccccccccc"
string s3(s1); // s3 = "hello"
string s4(s1, 1, 3); // s4 = "ell"


//获取字符串长度

string s = "hello";
int len = s.size(); // len = 5
int len2 = s.length(); // len2 = 5

//字符串拼接

string s1 = "hello";
string s2 = "world";
string s3 = s1 + s2; // s3 = "helloworld"
string s4 = s1 + " " + s2; // s4 = "hello world"
s1 += s2; // s1 = "helloworld"
s1.append(s2); // s1 = "helloworld"
s1.append("world"); // s1 = "helloworldworld"
s1.append(3, 'c'); // s1 = "helloworldccc"

//字符串比较
str1 == str2 //判断两个字符串是否相等
str1 > str2 //判断str1中第一个不匹配的字符比str2中对应字符大,或者所有字符都匹配但是比较字符串比源字符串长。

//获取子串
string str("Hello,World!");
string subStr = str.substr(3,5); // subStr = "lo,Wo"

//访问字符串中的字符
string str("Hello,World!");
char c = str[0]; // c = 'H'
char c2 = str.at(0); // c2 = 'H'

//查找字符串

find()函数返回字符串中第一个匹配的位置，如果没有找到则返回string::npos
rfind()函数返回字符串中最后一个匹配的位置，如果没有找到则返回string::npos

string str("Hello,World!");	
int pos = str.find("World"); // pos = 6
int pos2 = str.find("World", 7); // 从第7个字符开始查找，pos2 = string::npos
int pos3 = str.find("World", 7, 3); // 从第7个字符开始查找，最多查找3个字符，pos3 = string::npos

find_first_of()函数返回字符串中第一个匹配的位置，如果没有找到则返回string::npos
find_last_of()函数返回字符串中最后一个匹配的位置，如果没有找到则返回string::npos
find_first_not_of()函数返回字符串中第一个不匹配的位置，如果没有找到则返回string::npos
find_last_not_of()函数返回字符串中最后一个不匹配的位置，如果没有找到则返回string::npos


//插入删除字符串

string&insert(size_t pos，const string&str);　　　// 在位置 pos 处插入字符串 str
string&insert(size_t pos，const string&str，size_t subpos，size_t sublen);　// 在位置 pos 处插入字符串 str 的从位置 subpos 处开始的 sublen 个字符
string&insert(size_t pos，const char * s);　　　　// 在位置 pos 处插入字符串 s
string&insert(size_t pos，const char * s，size_t n);　// 在位置 pos 处插入字符串 s 的前 n 个字符
string&insert(size_t pos，size_t n，char c);　　　　　 // 在位置 pos 处插入 n 个字符 c
iterator insert(const_iterator p, size_t n, char c);　// 在 p 处插入 n 个字符 c，并返回插入后迭代器的位置
iterator insert (const_iterator p, char c);　　　　　　 // 在 p 处插入字符 c，并返回插入后迭代器的位置

string& erase (size_t pos = 0, size_t len = npos);　　　// 删除从 pos 处开始的 n 个字符
 
iterator erase (const_iterator p);　　　　　　　　　　　　// 删除 p 处的一个字符，并返回删除后迭代器的位置
 
iterator erase (const_iterator first, const_iterator last);　// 删除从 first 到 last 之间的字符，并返回删除后迭代器的位置

// 其他

getline(cin, s); // 从输入流中读取一行字符串
stoi(s); // 将字符串转换为整数
str.empty(); // 判断字符串是否为空
str.swap(str2); // 交换两个字符串的内容





