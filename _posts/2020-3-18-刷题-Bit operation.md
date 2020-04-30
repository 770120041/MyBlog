---
layout: post
title:  Python Bit Op
categories: Interview
---


## Bitwise AND of Numbers Range
结果是左边的公共部分

https://www.cnblogs.com/grandyang/p/4431646.html
```py
class Solution:
    def rangeBitwiseAnd(self, m: int, n: int) -> int:
        # n is the upper bound. m is lower bound.
        i = 0
        while m != n:
            m = m >> 1
            n = n >> 1
            i += 1
        return m << i
```