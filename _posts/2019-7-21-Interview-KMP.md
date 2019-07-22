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



#### Code for searching
```
int KmpSearch(char* s, char* p)
{
	int i = 0;
	int j = 0;
	int sLen = strlen(s);
	int pLen = strlen(p);
	while (i < sLen && j < pLen)
	{
		//①如果j = -1，或者当前字符匹配成功（即S[i] == P[j]），都令i++，j++    
		if (j == -1 || s[i] == p[j])
		{
			i++;
			j++;
		}
		else
		{
			//②如果j != -1，且当前字符匹配失败（即S[i] != P[j]），则令 i 不变，j = next[j]    
			//next[j]即为j所对应的next值      
			j = next[j];
		}
	}
	if (j == pLen)
		return i - j;
	else
		return -1;
}
```

## Code for finding next[i]
```
vector<int> findNext(string&p){
    vector<int> next(p.size(),0);
    p[0] = -1;
    int k=-1,j=0;
    //k is the matching size of prefix
    while(j<p.size()){
        if(k == -1 or p[k] == p[j]){
            k++;
            j++;
            next[j] = k;
        }
        else{
            k = next[k];
        }
    }
}
```


