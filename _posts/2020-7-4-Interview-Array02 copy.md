---
layout: post
title:  "Cracking the code interview"
categories: Interview
---
##第一章数组
手动实现一个resizable array
手动实现一个hashtable

print all palidrome permutation

https://www.geeksforgeeks.org/print-all-palindrome-permutations-of-a-string/

Permutation of duplicated string
一次打印出所有 


 rotated matrix 要做一下

cc 1.9

## 第二章
实现链表，最好能够用一个wrapper封装head，不然如果有多个人使用了这个head，但head修改了这些用户并不知道

2.1 follow up
2.3
2.4 能不开数组吗
2.6 开数组
2.7开hash

## 第三章
3.2 min o（1）？
3.5 每次pop要on？

## 第四章
记住什么是complete binary tree和full binary  tree， perfect binary tree
如何判断是不是complted， full 和 perfect
都是用queue做level order traversal

complete 是不是最后一层，左边空右边不空
full 是不是有两个孩子，遍历即可
perfect level order ，计算每层个数（对于队列，如果有非空有空就是wrong


bidirectional search
丛k的d次方到2倍k的d/2次方

拓扑排序
最小生成树
AVL树

4.2 queue 加进去再放进queue层序遍历
4.6 栈 模拟递归
4.7 拓扑排序 实现一下
4。9 不会
4。11 关键是怎么delete一个node，每个点加入到hashmap中，一个找一个点删掉
4。12 每个点return一个possible value 的unotdered set，维护这个set即可
时间 on 空间on

## 第五章 记住bit manipulate 的 tricks
2‘scomplement 取反加一
算术右移和逻辑右移
get set update clean bit
实现全部
