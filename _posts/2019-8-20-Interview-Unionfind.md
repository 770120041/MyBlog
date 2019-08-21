---
layout: post
title:  "Leetcode UnionFind"
categories: Interview
---
# 并查集 Union Find
在计算机科学中，并查集是一种树型的数据结构，用于处理一些不交集（Disjoint Sets）的合并及查询问题。有一个联合-查找算法（union-find algorithm）定义了两个用于此数据结构的操作：

Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。
Union：将两个子集合并成同一个集合。
由于支持这两种操作，一个不相交集也常被称为联合-查找数据结构（union-find data structure）或合并-查找集合（merge-find set）。其他的重要方法，MakeSet，用于创建单元素集合。有了这些方法，许多经典的划分问题可以被解决。

为了更加精确的定义这些方法，需要定义如何表示集合。一种常用的策略是为每个集合选定一个固定的元素，称为代表，以表示整个集合。接着，Find(x) 返回 x 所属集合的代表，而 Union 使用两个集合的代表作为参数。

[blog](https://blog.csdn.net/Yaokai_AssultMaster/article/details/78888846)
```
public class QuickFind {
public: 
    bool connected(int q,int p){
        return find(q) == find(p);
    }
    int find(int q){
        while(pre[q] != q){
            pre[q] = find(pre[q]);
        }
        return pre[q];
    }
    void union(int p,int q){
        int rootp = find(p);
        int rootq = find(q);
        if(rootp != rootq) pre[rootp] = pre[rootq];
    }
}
```

#### 547. Friend Circles
Template question
```
class Solution {
public:
    int find(vector<int>&pre,int p){
        if(pre[p] != p){
            p = find(pre,pre[p]);
        }
        return pre[p];
    }
    
    void unionFunction(vector<int>&pre,int p,int q){
        int rootP = find(pre,p);
        int rootQ = find(pre,q);
        pre[rootP] = rootQ;
    }
    
    int findCircleNum(vector<vector<int>>& M) {
        int n = M.size();
        unordered_set<int> uSet;
        vector<int> pre(n,0);
        for(auto i=0;i<n;i++) pre[i] = i;
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(M[i][j]) unionFunction(pre,i,j);
            }
        }
        for(auto x : pre){
            uSet.insert(find(pre,x));
        }
        return uSet.size();
    }
};
```

<hr>

## 684. Redundant Connection
Template question,注意这里是
```
int find(vector<int>&pre,int p){
        if(pre[p] != p){
            pre[p] = find(pre,pre[p]);
        }
        return pre[p];
    }
    
```
#### Union find
```
class Solution {
public:
    //最小生成树，但每条边都是1
    int find(vector<int>&pre,int p){
        if(pre[p] != p){
            pre[p] = find(pre,pre[p]);
        }
        return pre[p];
    }
    
    void unionF(vector<int>&pre,int p,int q){
        int rootP = find(pre,p);
        int rootQ = find(pre,q);
        pre[rootP] = rootQ;
    } 
    
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        vector<int> result;
        vector<int> pre(1005,0);
        for(auto i=0;i<1005;i++) pre[i] = i;
        for(auto x:edges){
            if(find(pre,x[0]) == find(pre,x[1])){
                result = x;
                break;
            }
            else{
                unionF(pre,x[0],x[1]);
            }
        }
        return result;
    }
};
```

<hr>

## 685. Redundant Connection II
If use Union find again, `[[2,1],[3,1],[4,2],[1,4]]` can't pass

[blog](https://www.cnblogs.com/grandyang/p/8445733.html)
[leetcode discussion](https://leetcode.com/problems/redundant-connection-ii/discuss/278105/topic)
同时检查入度和环，入度用一个数组记录，环用并查集记录
1. 只有入度为2：删除第二个增加入度的边
2. 只有环没有入度2：删除导致环的点
3. 有入度和环：删除导致且使得入度为2的点（另一个边不会导致环）
总结：导致环或者入度为2，则删除是不行的， 因为`[[2,1],[3,1],[4,2],[1,4]]`这种状态3的情况，如果删除第二个出现的`3,1`，会导致顶点数目不够，所以要删除即构成环又导致入度为2的点。

#### Union find
```
class Solution {
public:
    //最小生成树，但每条边都是1
    int getRoot(vector<int>&pre,int p){
        if(pre[p] != p){
            pre[p] = getRoot(pre,pre[p]);
        }
        return pre[p];
    }
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        vector<int> root(1005,0),first,second;
        //root is used as degree
        for(auto &x : edges){
            if(root[x[1]] == 0){
                root[x[1]] = x[0];
            }
            else{
                first = {root[x[1]],x[1]};
                second = x;
                x[1] = 0; //skip the checking of edge2, which is x[1]
            }
        }
        //reset root
        for(int i=0;i<1005;i++) root[i] = i;
        for(auto edge:edges){
            if(edge[1] == 0) continue;
            int x = getRoot(root, edge[0]), y = getRoot(root, edge[1]);
            if (x == y) return first.empty() ? edge : first;
            root[x] = y;
        }
        return second;
    }
};
```