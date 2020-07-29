---
title: '【题解】[NOI Online 3 入门组] 买表'
copyright: true
date: 2020-06-08 12:22:47
excerpt: 【题解】[NOI Online 3 入门组] 买表
top: 
tags: 题解
categories: 题解
---

提供一个不用背包的做法。  

先假设所有钱的数量无限 , 令 $\texttt{dp[x]}$ 表示价格为 $x$ 的手表能否购买。若 $\texttt{dp[x]=1}$ 则可以购买 , 反之则不行。  
显然 $\texttt{dp[0]=1}$ , 并且如果 $\texttt{dp[x-f]=1}$ , 那么 $\texttt{dp[x]=1}$ ( $f$ 代表一种币值) 。  
那么如果钱的数量有限制 , 我们便在 $\texttt{dp}$ 数组中存储币值的使用次数 , 即 $\texttt{dp[x]=dp[x-f]+1}$ ( $f$ 代表一种币值) , 并在转移的时候进行判断 , 判断 $\texttt{dp[x]}$ 是否大于这种钱的数量 , 如果大于就不转移。  
由于总共使用多少张钱是没有限制的 , 有限制的只是使用某一种钱的数量 , 那么我们每换一种币值 , 就要把所有可以达到的价格 (即所有大于 $1$ 的 $\texttt{dp[x]}$ ) 都重新赋值为 $1$ 后再进行新一轮的转移。  

代码如下 :
```cpp
#include <cstdio>

const int N=5e5,M=2e2;

int dp[N+10]={1};

struct Node {int k,a;} e[M+10];

int main() {
	int n,m; scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) scanf("%d%d",&e[i].k,&e[i].a);
	for(int i=1;i<=n;i++)
		for(int j=0;j<=N;j++) {
			if(dp[j]) dp[j]=1;
			if(j-e[i].k>=0&&dp[j-e[i].k]&&dp[j-e[i].k]<=e[i].a) dp[j]=dp[j-e[i].k]+1;
		}
	while(m--) {
		int x; scanf("%d",&x);
		puts(dp[x]?"Yes":"No");
	}
	return 0;
}
```