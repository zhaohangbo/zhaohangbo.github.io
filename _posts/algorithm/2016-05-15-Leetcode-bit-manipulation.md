---
layout: post
title: Leetcode xxx & 201 & 318
category: 算法
tags: [Leetcode,Bit Manipulation]
keywords: 算法
---

### Problem

- Idea
- Idea

```java
```

### Problem
- Idea

```java
```

### Problem 201. Bitwise AND of Numbers Range

```java
//The idea is to use a mask to find the leftmost common digits of m and n.
//Example: m=1110001, n=1110111,
//and you just need to find 1110000 and it will be the answer.
public int rangeBitwiseAnd1(int m, int n) {
			int r=Integer.MAX_VALUE;
			while((m&r)!=(n&r))  r=r<<1;
			return n&r;
}

//Mine
public  static int rangeBitwiseAnd(int m, int n) {
	int res =0;
	int mask = ~(-1>>>1);//10000000000000000000000000000000
	int lastMostLeftOfM=0;
	int lastMostLeftOfN=0;

	int curMostLeftOfM=0;
	int curMostLeftOfN=0;
	for(int i=1;i<32;i++){//loop 31 times, not 32
		m<<=1;
		n<<=1;
		curMostLeftOfM= (m&mask)>>>31;
		curMostLeftOfN= (n&mask)>>>31;
			int a= 0;
		if(lastMostLeftOfM==0 && lastMostLeftOfN==1){
			a=0;
		}
		else{
			a=curMostLeftOfM&curMostLeftOfN;
			lastMostLeftOfM = curMostLeftOfM;
			lastMostLeftOfN = curMostLeftOfN;
		}
		res <<=1;
		res += a;
	}
	return res;
}
```

## Problem 318. Maximum Product of Word Lengths

```java
```
