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

The meaning of `Next[j]`: 代表当前字符之前的字符串中，有多大长度的相同前缀后缀。例如如果next [j] = k，代表j 之前的字符串中有最大长度为k 的相同前缀后缀。



#### Code for searching(Need to test)

```
void calNextP(vector<int>&next,string&p){
	int j = 0;
	int k = -1;
	while(j < p.size()){
		if(k == -1 or p[k] == p[j]){
			next[j] = k
			k++;
			j++;
		}
		else{
			k = next[j];
		}
	}
}
int KMP(string s,string p){
	vector<int> next(p,0);
	calNextP(next,p);
	for(int i=0,j=0;i<s.size() and j < p.size() ; >{
		if(j == -1 or s[i] == s[j]){
			i++; j++;
		}
		else{
			j = next[j];
		}
	}
	if(j == p.size()){
		return i-j;
	}
	return -1;
}
```
