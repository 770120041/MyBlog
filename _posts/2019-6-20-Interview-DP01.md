---
layout: post
title:  "Leetcode DP01"
categories: Interview
---
 
 Using Simple Array to DP
## 55. Jump Game
Starts at 0, reaches at n(the length of array), note **not n-1**
```
bool canJump(vector<int>& nums) {
    int N = nums.size();
    int i=0;
    for(int reach=0;i<N && i<=reach;i++){
        reach  = max(reach,i+nums[i]);
    }
    return i==N;
}
```

<hr>


## 309. Best Time to Buy and Sell Stock with Cooldown
