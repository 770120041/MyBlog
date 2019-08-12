---
layout: post
title:  "背包问题"
categories: Interview
---
## 01背包问题 
有 N 件物品和一个容量为 V 的背包。放入第 i 件物品耗费的容量是 Ci
1，得到的价值是 Wi。求解将哪些物品装入背包可使价值总和最大。

这是最基础的背包问题，特点是：每种物品仅有一件，可以选择放或不放。

F[i,v] 代表考虑了第i件物品，容量为V的最大价值，所以最后要求的数是F[N,V],假设第i个物品的价值是w[i],总量是v[i]

那么状态转移方程是`F[i,v] = max(F[i-1,v],F[i-1,v-v[i]] + w[i])`

#### 暴力
```
//initialize to `(N+1)*(V+1)` for border cases
vector<vector<int>> F(N+1,vector<int>(V+1,0));
for(int i=1;i<=N;i++){
    // j starts from v[i] to avoid j-v[i]<0
    for(int j=v[i];j<=V;j++){
        F[i][j] = max(F[i-1][j],F[i-1][j-v[i]]+w[i]);
    }
}
return F[N][V];
```
`Space Complexity O(N*V), time complexity O(N*V)`

#### 优化
可以看到其实只使用了两层空间，所以其实可以优化space complexity为`O(2*V`)
```
//initialize to `(2)*(V+1)` for border cases
vector<vector<int>> F(2,vector<int>(V+1,0));
for(int i=1;i<=N;i++){
    for(int j=v[i];j<=V;j++){
        F[1][j] = max(F[0][j],F[0][j-v[i]]+w[i]);
    }
    for(int j=0;j<V+1;j++){
        F[0][j] = F[1][j]; 
    }
}
return F[0][V];
```
甚至可以只用一层空间，原因对于每一层考虑容量的循环，因为每个物品只能取一件，所以考虑从后面往前循环，这样就不会一次用同一个物品两次都不会改变之前的值，比如`F[5]=max(F[5],F[3]+w[i])`,所以可以只用一维数组
```
//initialize to `(V+1)` for border cases
vector<vector<int>> F(V+1,0);
for(int i=1;i<=N;i++){
    for(int j=V;j>=v[i];j--){
        F[j] = max(F[j],F[j-v[i]]+w[i]);
    }
}
return F[V];
```

#### 细节
**恰好装满背包**和不恰好装满背包

如果要恰好装满，那么除了F[0] = 0, 其他设置为-∞。这是为什么呢？可以这样理解：初始化的 F 数组事实上就是在没有任何物品可以放入背包时的合法状态。如果要求背包恰好装满，那么此时只有容量为 0 的背包可以在什么也不装且价值为 0 的情况下被“恰好装满”，其它容量的背包均没有合法的解，属于未定义的状态，应该被赋值为 -∞ 了。如果背包并非必须被装满，那么任何容量的背包都有一个合法解“什么都不装”，这个解的价值为0，所以初始时状态的值也就全部为0了。


## 完全背包问题
有 N 种物品和一个容量为 V 的背包，每种物品都有无限件可用。放入第 i 种物品
的费用是 Ci，价值是 Wi。求解：将哪些物品装入背包，可使这些物品的耗费的费用总和不超过背包容量，且价值总和最大。

#### 暴力
```
//initialize to `(V+1)` for border cases
vector<vector<int>> F(N+1,vector<int>(V+1,0));
for(int i=1;i<=N;i++){
    // j starts from v[i] to avoid j-v[i]<0
    for(int k=1;k<= j/v[i] ; k++){
        F[i][j] = max(F[i-1][j],F[i-1][j-k*v[i]]+w[i]);
    }
}
return F[N][V];
```
`Space Complexity O(N*V), time complexity O(N*V)`

#### 优化
不考虑常数级别的优化的话，考虑转化为0-1背包问题求解。

使用一维数组
```
//initialize to `(V+1)` for border cases
vector<vector<int>> F(V+1,0);
for(int i=1;i<=N;i++){
    for(int j=v[i];j<=V;j++){
        F[j] = max(F[j],F[j-v[i]]+w[i]);
    }
}
return F[V];
```

## 多重背包问题
