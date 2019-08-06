---
layout: post
title:  "Leetcode KMP"
categories: Interview
---
[blog](https://blog.csdn.net/v_july_v/article/details/7041827)
## KMP
```
Suppose we have a string S and a pattern string P
we want to find the if P is a substr of S
```
1. preprocess P, get Next[]
2. Using next[] to search

The meaning of `Next[j]`: 代表当前字符之前的字符串中，有多大长度的相同前缀后缀。例如如果next [j] = k，代表j **之前**的字符串中有最大长度为k 的相同前缀后缀。

特别注意这里的next[j]代表的是j之前的字符串，所以计算的时候要注意。

## Code

```
class Solution {
public:
    vector<int> findPrefix(const string&needle){
        vector<int> next(needle.size(),-1);
        int j = -1,i=0;
        while(i<needle.size()-1){
            if(j == -1 or needle[i] == needle[j]){
                i++; j++;
                // the reason why next[i] = j is after i++,j++ is that next[j] means string from 0 to j-1 has a prefix of length k
                next[i] = j;
            }
            else{
                j = next[j];
            }
        }
        return next;
    }
    int strStr(string haystack, string needle) {
        if(needle.size() == 0) return 0;
        if(haystack.size() == 0) return -1;
        auto next = findPrefix(needle);
        int i=0,j=0;
        int m = haystack.size(),n = needle.size();
        while( i < m and j  < n ){
            if(j == -1 or (haystack[i] == needle[j])){
                i++;j++;
            }
            else{
                j = next[j];
            }
        }
        
        if(j == needle.size()){
            return i-j;
        }
        return -1;
    }
};
```

They are many details needs to take care of:
1. border condition, `if needle.size() == 0 or haystack.size() == 0`
2. Do remember needle.size() is unsigned int, so if we compare `j<n`, j may be -1, so it will cause problem. Use a temporary to store it.
3. Be carefule about find the prefix array.
```
if(j == -1 or needle[i] == needle[j]){
                i++; j++;
                // the reason why next[i] = j is after i++,j++ is that next[j] means string from 0 to j-1 has a prefix of length k
                next[i] = j;
            }
```
