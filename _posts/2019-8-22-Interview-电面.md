---
layout: post
title:  "电面"
categories: Interview
---
# Coupang
## Question:Lexicographically smallest array after at-most K consecutive swaps
https://www.geeksforgeeks.org/lexicographically-smallest-array-k-consecutive-swaps/
#### DFS
需要注意的是 翻转n次的最优解和n+1次的最优解没关系，不能用dp
失败：比较大小的isLarger写错了
```
#include <iostream>
#include <vector>
using namespace std;
bool isLarger(vector<int>&a,vector<int>&b){
    for(auto i=0;i<a.size();i++){
        if(a[i] > b[i]) return true;
        else if(a[i] < b[i]) return false;
    }
    return true;
}
void DFS(vector<int>&result,vector<int>&input,int k){
    if(isLarger(result,input)) result.assign(input.begin(),input.end());
    if(k==0) return;
    for(int i=0;i<input.size()-1;i++){
        swap(input[i],input[i+1]);
        DFS(result,input,k-1);
        swap(input[i],input[i+1]);
    }
}

int main()
{
    for(int n=0;n<=12;n++){
        vector<int> input{4,3,2,1,5};
        vector<int> result = input;
        DFS(result,input,n);   
        for(auto x:result){
            cout<<x<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```

#### Optimum
O(n^2),因为每次把最高位变的尽可能小就得到最小
```
#include <iostream>
#include <vector>
using namespace std;
void DFS(vector<int>&result,int k){
    for(int i=0;i<result.size()-1 and k > 0;i++){
        int pos = i;
        for(int j=i+1;j<result.size();j++){
            if(j-i>k) break;
            if(result[j] < result[pos]) pos = j;
        }
        for(int j=pos;j>i;j--){
            swap(result[j-1],result[j]);
        }
        k -= (pos-i);
    }
}

int main()
{
    for(int n=0;n<=15;n++){
        vector<int> input{4,3,2,1,5};
        DFS(input,n);
        for(auto x:input){
            cout<<x<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```

## Cadence
聊C++，匹兹堡，Citrix实习，过

四轮Onsite 过

## OA
### DRW
#### Q1 Bike
Sort `O(N*logN)`
cal the distance in between, then the `result = (a[i]-a[i-1])/2`
#### Q2
```
class Solution {
public:
    int maxTurbulenceSize(vector<int>& A) {
        int N = A.size();
        if(N <= 1) return 1;
        vector<int> up(N,1);
        vector<int> down(N,1);
        int result=1;
        for(int i=1;i<N;i++){
            if(A[i] > A[i-1]){
                up[i] = down[i-1]+1;
            }
            else if(A[i] < A[i-1]){
                down[i] = up[i-1]+1;
            }
            result = max(result,max(up[i],down[i]));
        }
        return result;
    }
};
```

#### Q3 Zero SubArray
```

#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;
int numOfZero(vector<int>&input){
    vector<int> prefix = input;
    unordered_map<int,vector<int>> uMap;
    for(int i=0;i<prefix.size();i++){
        if(i != 0){
            prefix[i] += prefix[i-1];
        }
        auto it = uMap.find(prefix[i]);
        if(it == uMap.end()){
            vector<int> tmp{i};
            uMap[prefix[i]]=tmp;
        }
        else uMap[prefix[i]].push_back(i);
    }
    int result=0;
    for(int i=0;i<prefix.size();i++){
        if(prefix[i] == 0) result++;
        int curVal = prefix[i];
        auto it = uMap.find(curVal);
        if(it == uMap.end()){}
        else{
            for(auto x:it->second){
                if(x < i){
                    cout<<"l="<<x<<",r="<<i<<endl;
                    result++;
                }
            }
        }
    }
    return result;
}

int main()
{
    vector<int> input{0,0,0,0,0};
    cout<<numOfZero(input)<<endl;

    return 0;
}

```

#### Q4
Determine if it is possible to choose two requests that will determine an even distribution of requests among 3 works

去掉两个以后能被3个人平分： Sum%3 == 0 and 存在两个sequence和为sum/3