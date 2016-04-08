---
layout: post
title: Union and Find
category: 算法
tags: 算法
keywords: 算法,Algorithm
---

[Algorithms BookSite](http://algs4.cs.princeton.edu/15uf/)

### Sample Problem

```
0  1  2

3  4  5

union(3,0)
union(3,4)
union(2,5)
union(2,1)

0  1--2
|     |
3--4  5

connected(0,5) --> false
connected(0,3) --> true

union(1,4)

0  1--2
|  |  |
3--4  5

connected(0,5) --> true
```

### Union and Find(并查集)的应用领域

- Pixels in a digital photo
- Computer network connectivity
- Friends in social network
- Transistor in a computer chip

`An example here:`

![union-find-example](/assets/img/posts/algorithm/2016-04-08/union-find-example.png)

### Union-Find data type(API) in Java

```
public class UF(){
  UF(int N)
  void union(int p, int q)
  boolean connected(int p, int q)
  int find(int p)
  int count()
}
```

### Quick-Find Implementation

![quick-find-1](/assets/img/posts/algorithm/2016-04-08/quick-find-1.png)
![quick-find-2](/assets/img/posts/algorithm/2016-04-08/quick-find-2.png)
![quick-find-3](/assets/img/posts/algorithm/2016-04-08/quick-find-3.png)

### Quick-Union Implementation

![quick-union-1](/assets/img/posts/algorithm/2016-04-08/quick-union-1.png)
![quick-union-2](/assets/img/posts/algorithm/2016-04-08/quick-union-2.png)

### Weighted-Quick-Union

![Weighted-quick-union-1.png](/assets/img/posts/algorithm/2016-04-08/Weighted-quick-union-1.png)
![Weighted-quick-union-2.png](/assets/img/posts/algorithm/2016-04-08/Weighted-quick-union-2.png)
![Weighted-quick-union-3.png](/assets/img/posts/algorithm/2016-04-08/Weighted-quick-union-3.png)
![Weighted-quick-union-4.png](/assets/img/posts/algorithm/2016-04-08/Weighted-quick-union-4.png)

### Summary

![Union-Find-summary](/assets/img/posts/algorithm/2016-04-08/summary.png)

### Reference

[BookSite-Princeton-Algorithms from Coursera](http://algs4.cs.princeton.edu/15uf/)
