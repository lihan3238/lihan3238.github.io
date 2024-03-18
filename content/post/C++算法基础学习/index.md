---
title: C++算法基础学习
description: C++算法基础学习,为蓝桥杯什么的程序竞赛学点基础算法
slug: cpp_algorithm_basic_study
date: 2024-03-14 18:42:00+0800
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
  - title: 
    description: 
    website: 
    image: 

---

# 基础算法学习

## 前缀和与差分

```
问题引入：给出一个长度为n的数组a：a[1], a[2], ..., a[n]
有m次询问
每次询问会给出一个区间[l, r]
请输出：a[l] + a[l+1] + ... + a[r]
```

如果使用暴力算法，时间复杂度为O(m*(r-l+1)),约为O(mn),算法如下:
    
```cpp
while(m--){
    int l, r;
    cin >> l >> r;
    int sum = 0;
    for(int i = l; i <= r; i++){
        sum += a[i];
    }
    cout << sum << endl;
}
```

### 一维前缀和

#### 定义

`一维前缀和`：
已知数组a：a[1], a[2], ..., a[n]
其前缀和数组Sum定义为：
Sum[1] = a[1]
Sum[2] = a[1] + a[2]
...
Sum[L-1] = a[1] + a[2] + ... + a[L-1]  
...
Sum[R] = a[1] + ... + a[L] + ... + a[R]
...
Sum[n] = a[1] + a[2] + ... + a[n]

查询数组a的区间[L, R]和：
a[L] + a[L+1] + ... + a[R] = Sum[R] – Sum[L-1]

#### 例题与代码

这样，提前处理出前缀和数组Sum，每次询问就可以在O(1)的时间复杂度内得到结果，算法如下:

```cpp

int a[maxn]; // 原始数组
int s[maxn]; // 前缀和数组

int main(){
    int n, m;
    cin >> n >> m;
    for(int i = 1; i <= n; i++){
        cin >> a[i];
        s[i] = s[i-1] + a[i];
    }
    while(m--){
        int l, r;
        cin >> l >> r;
        cout << s[r] - s[l-1] << endl;
    }
    return 0;
}//时间复杂度为O(max(n,m))
```

`例题`:
![exp_1](imgs/exp_1.png)

```cpp
#include<bits/stdc++.h>
#define MAXN 200005

int n, a[MAXN];
long long sum[MAXN], ans;

int main()
{
    std::cin >> n;
    for(int i = 1; i <= n; i++)
    {
        std::cin >> a[i];
        sum[i] = sum[i - 1] + a[i];
    }
    for(int i = 1; i <= n - 1; i++)
        ans += a[i] *(sum[n] - sum[i]);
    std::cout << ans << std::endl;
    return 0;
}
```
### 一维差分

#### 定义

`一维差分`

一维前缀和的逆运算

数组a：a[1], a[2], ..., a[n]
数组b：b[1], b[2], ..., b[n]

定义：a[i] = b[1] + b[2] + ... + b[i]
则：
- a为b的前缀和数组
- b为a的差分数组


b[1] = a[1]
b[2] = a[2] - a[1]
b[3] = a[3] - a[2]
...
b[n] = a[n] - a[n-1]


#### 例题与代码

`例题`:

问题引入：
给出一个长度为n的数组a：a[1], a[2], ..., a[n]
有m次询问
每次询问会给出一个区间[l, r]和一个数c
请让数组a中的[l, r]的每个数都加上c

```cpp
//暴力算法
while(m--){
    int l, r, c;
    cin >> l >> r >> c;
    for(int i = l; i <= r; i++){
        a[i] += c;
    }
}//时间复杂度为O(m*(r-l+1))->O(mn)
```

a是b的前缀和数组，b是a的差分数组
先求出差分数组b，然后对b进行处理，最后再反过来求前缀和求出a数组

若b[2] =  b[2] + c

则a[1] = b[1]
a[2] = b[1] + b[2] + c
a[3] = b[1] + b[2] + b[3] + c
...
a[n] = b[1] + b[2] + ... + b[n] + c 

可见，只要对b[l]加c，a[l]到a[n]都会加c；
所以只要对b[l]加c，b[r+1]减c即可。

```cpp
int a[maxn], b[maxn]; // 原始数组和差分数组

int main(){
    int n, m;
    cin >> n >> m;
    for(int i = 1; i <= n; i++){//求差分数组
        cin >> a[i];
        b[i] = a[i] - a[i-1];
    }
    while(m--){//对差分数组进行处理
        int l, r, c;
        cin >> l >> r >> c;
        b[l] += c;
        b[r+1] -= c;
    }
    for(int i = 1; i <= n; i++){//求前缀和数组
        a[i] = a[i-1] + b[i];
        cout << a[i] << " ";
    }
    return 0;
}//时间复杂度为O(n+m)
```

`例题`:
![exp_2](imgs/exp_2.png)

```cpp
#include <cstdio>
#include <algorithm>
using namespace std;
int a[5000005],diff[5000005],n,p,mina;
int main()
{
 scanf("%d%d",&n,&p);
 for(int i=1;i<=n;i++)
 {
  scanf("%d",&a[i]);
  diff[i]=a[i]-a[i-1];//求差分数组
 }
 for(int i=1;i<=p;i++)
 {
  int x,y,z;
  scanf("%d%d%d",&x,&y,&z);
  diff[x]+=z;
  diff[y+1]-=z;//对差分数组进行处理
 }
 mina=diff[1];
 for(int i=1;i<=n;i++)
 {
  a[i]=a[i-1]+diff[i];
  mina=min(a[i],mina);//求前缀和数组
 }
 printf("%d",mina);//输出
 return 0;
}
```

- - -
`tips`
- 1.cin好像比scanf慢
- 2.最后要求最小的，一个是在最后求前缀和数组的时候，记录最小的，如果下一个是最小的 就替换掉；这里让我想起来sort函数:
```cpp
#include <algorithm>
int a[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
sort(a, a+10);//默认从小到大排序

bool myfunc(int a, int b){
    return a > b;
}
sort(a, a+10, myfunc);//从大到小排序
```

- - -

### 二维前缀和与差分

问题引入:

给出一个n行m列的矩阵
有m次询问
每次询问会给出四个整数：
表示子矩阵的左上角坐标和右下角坐标
请输出子矩阵中所有数之和

n行m列的矩阵a：a[n][m]
定义前缀和矩阵为：sum[n][m]
差分矩阵为：b[n][m]


#### 定义

`二维前缀和`

sum[i][j]的含义为：(1, 1)左上角到 (i, j) 右下角的子矩阵的元素之和

![1](imgs/1.png)

二维前缀和预处理公式:

sum[i][j] = sum[i-1][j] + sum[i][j-1] - sum[i-1][j-1] + a[i][j]

`二维差分`

b[i][j] = a[i][j] - a[i-1][j] - a[i][j-1] + a[i-1][j-1]

#### 例题与代码

![2](imgs/2.png)

求子矩阵的和，就是如图四个矩阵的加减得出。

sum(x1,y1,x2,y2)=sum[x2][y2]−sum[x1−1][y2]−sum[x2][y1−1]+sum[x1−1][y1−1]

`例题`:

![exp_3](imgs/exp_3.png)

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int a[1002][1002];
int b[1002][1002];


int main()
{
	int n,m;
	int x1,y1,x2,y2;
	
	cin>>n>>m;

	
	for(int p=1;p<=m;p++)
	{
		cin>>x1>>y1>>x2>>y2;
		b[x1][y1]++;
		b[x2+1][y1]--;
		b[x1][y2+1]--;
		b[x2+1][y2+1]++;
	}
	
	
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		{
			a[i][j]=a[i-1][j]+a[i][j-1]-a[i-1][j-1]+b[i][j]; 
			cout<<a[i][j]<<" ";
		}
		cout<<endl;
	}
	return 0;
 } 
```

- - -
`tips`

1. 有+1 -1 数组大小记得多设点，别g了，不差那点

- - -

## 排序

- 常见排序
- - 冒泡排序    $O(n^2)$
- - 插入排序    $O(n^2)$
- - 选择排序    $O(n^2)$
- - 桶排序      $O(max(m,n))$
- - 快速排序    $O(nlogn)$
- - 归并排序    $O(nlogn)$

### 冒泡排序

#### 定义

从数组头部开始，遍历数组中的每一个数，通过相邻两数的比较交换，每一轮循环下来找出剩余未排序数中的最大数（或者最小数）并“冒泡”至数组尾部。

#### 优化

- 1.如果某一轮循环中没有发生交换，说明数组已经有序，可以提前结束循环
- 2.每一轮循环中，最后一次发生交换的位置之后的数都是有序的，可以记录下来，下一轮循环的终点就是这个位置

#### 代码

```cpp
void bubbleSort(int a[], int n){
    for(int i = 0; i < n; i++){
        bool flag = false;
        for(int j = 0; j < n-i-1; j++){
            if(a[j] > a[j+1]){
                swap(a[j], a[j+1]);
                flag = true;
            }
        }
        if(!flag) break;
    }
}
```

### 桶排序

#### 定义

若待排序的数据在一个明显有限范围内（整型）时，可设计有限个有序桶，每个桶中装入一个值（当然也可以装入若干个值，装入若干值时，桶内数据有序），顺序输出各桶的值，得到有序序列。

![3](imgs/3.png)

#### 代码

```cpp
//输入n(1≤n≤105)个0到100的整数，输出这n个数从小到大排序的结果
#include <iostream>
#include <algorithm>
#include <cstring>
#include <cstdio>
using namespace std;
int main()
{
    int n;
    int a[105];
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        int x;
        cin>>x;
        a[x]++;
    }//O(n)
    for(int i=0;i<=100;i++)
    {
        for(int j=1;j<=a[i];j++)
        {
            cout<<i<<" ";
        }
    }//O(数据范围)
    return 0;
}//时间复杂度为O(max(n，数据范围))
```
### 快速排序

#### 定义

通过一趟快速排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，直到最后各部分只有单个数为止。

#### 代码

假设数组长度为10，需要进行升序排序的数组为：
array[10]={23, 12, 43, 2, 8, 18, 73, 21, 23, 35}
首先选取一个关键数据key：通常选用数组的第一个数
然后再按此方法对这两部分数据分别进行快速排序，直到最后各部分只有单个数为止。

sort函数就是快速排序的实现

```cpp
//默认从小到大排序
int a[10]={23, 12, 43, 2, 8, 18, 73, 21, 23, 35};
sort(a, a+10);

//自定义排序
struct stdInfo{
    string name;
    int age;
    int score;
};

bool cmp(stdInfo a, stdInfo b){
    if(a.score != b.score) return a.score > b.score;
    else if(a.age != b.age) return a.age < b.age;
    else return a.name < b.name;
}
stdInfo a[10];
sort(a, a+10, cmp);
```


### 归并排序

#### 定义

- 归并操作 的概念：将两个或两个以上有序的数组，合并成一个仍然有序的数组。
- 二路归并（两两合并）、三路归并（ 三个并一个）....
- 二路归并过程：从两个有序数组的表头开始  依次比较 a[i] 和 a[j] 的大小，如果 a[i] < a[j]，则将 a[i] 复制到归并数组 r[k] 中，并且 i 和 k 都加 1 后移，否则将 a[j] 复制归并到 r[k] 中，并且 j 和 k 都加 1 后移；如此循环下去，直到其中一个有序表取完后再将另外一个有序表中剩余的元素复制到 r 中。

#### 代码

```cpp
void mergeSort(int a[], int l, int r){
    if(l >= r) return;
    int mid = l + r >> 1;//x >> 1相当于x / 2
    mergeSort(a, l, mid);
    mergeSort(a, mid+1, r);
    int i = l, j = mid+1, k = 0;
    while(i <= mid && j <= r){
        if(a[i] <= a[j]) b[k++] = a[i++];
        else b[k++] = a[j++];
    }
    while(i <= mid) b[k++] = a[i++];
    while(j <= r) b[k++] = a[j++];
    for(int i = l, j = 0; i <= r; i++, j++) a[i] = b[j];
}

int main(){
    int a[10] = {23, 12, 43, 2, 8, 18, 73, 21, 23, 35};
    mergeSort(a, 0, 9);
}
```