---
layout:     post
title:      专题训练 ｜ 模拟与高精赛前指导
subtitle:   
date:       2020-05-03
author:     虞正皓
header-img: img/post-bg-os-metro.jpg
catalog: false
tags:
    - 算法
---
### 本文所有题目都在：<https://www.luogu.com.cn/training/9473#problems>

所谓“建模”，就是把事物进行抽象，根据实际问题来建立对应的数学模型。“抽象”并不意味着晦涩难懂；相反，它提供了大量的便利。计算机很难直接去解决实际问题，但是如果把实际问题建模成数学问题，就会大大地方便计算机来“理解”和“解决”。

举个生活中常见的例子：我们拿到了某次数学考试的成绩单，现在需要知道谁考得最好。当然不能把成绩单对着电脑晃一晃，然后问“谁考得最好？”。需要通过一种途径让计算机来理解这个问题。这个问题可以建模成：“给定数组
`score[]`，问数组内元素的最大值”。这样建模后，就能很方便的写程序解决问题了。对于这个问题，采用之前讨论过的“擂台法”，就可以给出答案。

如何把实际问题建模成数学问题，主要依靠我们的经验和直觉、当然还有你灵动的思维；而算法与数据结构，正是解决数学问题的两把利剑。从这一章开始会介绍一些程序设计竞赛中的一些常见套路算法，而下一部分会介绍基础的数据结构。如果已经认真学习完了第一部分，相信这一部分也不在话下。

这一章是语言部分的延伸，会介绍一些竞赛中会出现的“模拟题目”——这里的“模拟”不是指模拟某场比赛的模拟题，而是指让程序完整的按照题目叙述的方式执行运行得到最终答案。同时也会介绍可以计算很大整数的高精度运算方法。这一章对思维与算法设计的要求不高，但是会考验编程的基本功是否扎实
。
##什么是模拟

模拟就是按照题意建模，题目让你干什么你就照着它干什么。

模拟的思路非常简单，基本不需要思考。但模拟题有码量大，难调试等缺点，在考场写错也非常浪费时间。

## 讲一个例题：P1042 乒乓球（普及-）

<https://www.luogu.com.cn/problem/P1042>

### 思路

从题中我们得知它需要我们球11分制和21分制的比分结果，也就是华华对手或华华谁更快到达（11或21）分时，并且比分差的绝对值不小于2，就输出比分（立为一局），当然这里还有一个重要的点，就是最后一局比赛时，可能比分不会到11或21，我们直接搜索到E结尾，最后退出循环再输出一遍最后局结果即可

### 注意！

1. 本题奇大无比的数据量
2. 最后的当前比分也许是0:0，一定要输出！！！

### code
```cpp
#include <cstdio>

#include <cstring> 
char a[1000000];     //其实可以不保存，但是本人习惯
int w, l, x, n, g, t, m, i;
using namespace std;
int main(){
    int p[10000], q[10000];
    while(scanf("%c", &a[t]) && a[t]!='E'){ //读取数据，到‘E’结束
        if(a[t]=='W'){ 
            w++;   //11分制
            x++;   //21分制
        }
        if(a[t]=='L'){ //同上
            l++;
            n++;
        }
        if((w>10 && w-l>1) || (l>10 && l-w>1)){   //W或L比赛者到达11时，并且比分差不小于2，立为一局，保存一局比分
            q[m++]=w;
            q[m++]=l;
            w=0;
            l=0;
        }
        if((x>20 && x-n>1) || (n>20 && n-x>1)){   //W或L比赛者到达21时，并且比分差不小于2，立为一局，保存一局比分
            p[g++]=x;
            p[g++]=n;
            x=0;
            n=0;
        }
        t++;    
    }
    p[g++]=x;   // 当前比分
    p[g++]=n;
    q[m++]=w;
    q[m++]=l;   
    for(i=0;i<m;i+=2){  //输出11分制下比分
        if(i==0){
            printf("%d:%d", q[i], q[i+1]);
        }
        else printf("\n%d:%d", q[i], q[i+1]);       
    }
    printf("\n");
    for(i=0;i<g;i+=2)   //输出21分制下比分
        printf("\n%d:%d", p[i], p[i+1]);
    return 0;
}
```

## 高精度：背他！

1.概念

高精度运算,是指参与运算的数(加数,减数,因子……）范围大大超出了标准数据类型（整型，实型）能表示的范围的运算。例如，求两个200位的数的和。这时，就要用到高精度算法了

高精度使用数组来存储整数，模拟手算进行四则运算

2.高精度运算涉及到的问题

（1） 数据的输入

（2） 数据的存储

（3）数据的运算：进位和借位

（4）结果的输出：小数点的位置和处于多余的0 

3.高精度加法

```cpp
#include<iostream>

#include<cstdio>

#include<cstring>
using namespace std;
int main()
{
	char a1[100],b1[100];
	int a[100],b[100],c[100];
	int lena,lenb,lenc,i,x; 注意x是Int型
	memset(a,0,sizeof(a)); 
	memset(b,0,sizeof(b));
	 memset(c,0,sizeof(c));
	 gets(a1); 
	 gets(b1); //输入加数与被加数 
	 lena=strlen(a1); 
	 lenb=strlen(b1); 
	 for (i=0;i<=lena-1;i++)
	    a[lena-i]=a1[i]-48; //加数放入a数组 　 
	 for (i=0;i<=lenb-1;i++) 
	    b[lenb-i]=b1[i]-48; //加数放入b数组 
	 lenc =1; x=0; 
	 while (lenc <=lena||lenc <=lenb) 
	 { 　 
	  c[lenc]=a[lenc]+b[lenc]+x; //两数相加 　
	  x=c[lenc]/10; 　
	  c[lenc]%=10; 
	  lenc++; 
	}
    c[lenc]=x; 
	if (c[lenc]==0)//此时最高位上的加法 例如9+3 = 12 要溢出一位 
   		lenc--; //处理最高进位 
	for (i=lenc;i>=1;i--)
        cout<<c[i]; //输出结果 cout<<endl;
	return 0;
 }
```

4.高精度减法

  
  和高精度加法相比，减法在差为负数时处理的细节更多一点：当被减数小于减数时，差为负数，差的绝对值是减数减去被减数；在程序实现上用一个变量来存储符号位，用另一个数组存差的绝对值。

算法流程：

- 读入被减数S1，S2（字符串）；

- 置符号位：判断被减数是否大于减数：大则将符号位置为空；小则将符号位置为“-”，交换减数与被减数；

- 被减数与减数处理成数值，放在数组中；

- 运算：
	- 取数；
    - 判断是否需要借位；
    - 减，将运算结果放到差数组相应位中；
    - 判断是否运算完成：是，转5；不是，转A；
- 打印结果：符号位，第1位，循环处理第2到最后一位；

```cpp
#include<stdio.h>

#include<string.h>
    
void exch_str(char*s1, char*s2)//当减数大于被减数 交换二者 eg:2 -3 减数为3 
{
	char tmp[1001];
    strcpy(tmp,s1);
    strcpy(s1,s2);
    strcpy(s2,tmp);
}
    
int a1[1001],b1[1001],s[1001]; 

int main()
{
    char a[1001], b[1001];
    int sign=1;
    int len_a, len_b, i, j, k=0, t;
    scanf("%s %s", a, b);
    len_a = strlen(a);
    len_b = strlen(b);
    if(len_a < len_b)
    {
        sign = -1;//符号位置负
        exch_str(a,b);//交换被减数与减数
        t = len_a;
        len_a = len_b;
        len_b = t;//交换他们的长度
    }
    else if(len_a == len_b)
      for(i = 0; i < len_a; ++i)
        if(a[i] > b[i])
        {
          sign = 1;
          break;
        }
        else if(a[i] < b[i])//两者长度相同 差仍未负数 
        {
          sign = -1;
          exch_str(a,b);
          break;
        }
        else
          sign = 1;
   for(i = 0; i < len_a; ++i)
       a1[i] = a[i] - '0';
   for(j = 0; j < len_b; ++j)
       b1[j] = b[j] - '0';
   while(i>=0 && j>=0)
      {
    s[k] = a1[i] - b1[j];//s[0]=0,因为a1[len_a]和b1[len_b]为0
    if(s[k] < 0)
    {
      a1[i-1] -= 1;//借位
      s[k]+=10;
    }
    k++;
    i--;
    j--;
   }
    while(i >= 0)
    {
     s[k] = a1[i];
     k++;
     i--;
    }
    if(sign<0)
       printf("-");
    while(s[k]==0&&k>0)
      k--;
    if(k == 0)
       printf("0");
    while(k > 0)
    {
      printf("%d", s[k]);
      k--;
    }
  return 0;
}
```

5.高精度乘法

```cpp
#include<string.h> 
#include<stdio.h>

char n[255],m[255];

int n1[255],m1[255],s[510];
int main()
{

	int i,j, k=0, t, x=0, dig;
	int lenn, lenm;
	scanf("%s %s", &n, &m);
	lenn = strlen(n);
	lenm = strlen(m);
	for(i = 0; i < lenn; i++)
	   n1[i] = n[i] - 48;
	for(j = 0;j < lenm; j++)
	   m1[j] = m[j] - 48;
	for(j = lenm-1; j>=0; j--)
	{
		t=k;
		for(i = lenn-1; i >= 0; i--)
		{
		  s[t]+=n1[i]*m1[j];
		  t++;
	    }
	    ++k;
	    dig=t;
	}
	for(i = 0;i < dig; i++)
	   while(s[i] >= 10)
          {
	     s[i] -= 10;
	     ++s[i+1];
	   }
   if(s[dig] != 0)
     for(i=dig;i>=0;i--)
        printf("%d",s[i]);
          else
     for(i=dig-1;i>=0;i--)
           printf("%d",s[i]);
	return 0;
}

```

6.高精度除法
```cpp
#include<iostream>  
#include<cstring>  
using namespace std;  
int main()  
{  
  string str1,str2;  
  int a[250],b[250],c[500],len;    //250位以内的两个数相乘  
  int i,j;  
  memset(a,0,sizeof(a));  
  memset(b,0,sizeof(b));  
  cin>>str1>>str2;  
  a[0]=str1.length();  
  for(i=1;i<=a[0];i++)  
    a[i]=str1[a[0]-i]-'0';  
  b[0]=str2.length();  
  for(i=1;i<=b[0];i++)  
    b[i]=str2[b[0]-i]-'0';  
  memset(c,0,sizeof(c));  
  for(i=1;i<=a[0];i++)   //做按位乘法同时处理进位，注意循环内语句的写法。  
    for(j=1;j<=b[0];j++)  
    {  
    c[i+j-1]+=a[i]*b[j];  
    c[i+j]+=c[i+j-1]/10;  
    c[i+j-1]%=10;     
    }  
  len=a[0]+b[0]+1;  //去掉最高位的0，然后输出  
  while((c[len]==0)&&(len>1)) len--;   //为什么此处要len>1??  
  for(i=len;i>=1;i--)  
    cout<<c[i];  
  return 0;   
}
```