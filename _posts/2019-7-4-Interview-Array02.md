---
layout: post
title:  "Leetcode Arrays 02"
categories: Interview
---
## 581. Shortest Unsorted Continuous Subarray
First solution is just sort, O(nlogn)
```
public:
    // longest increasing sequence
    int findUnsortedSubarray(vector<int>& nums) {
        vector<int> tmp(nums);
        sort(tmp.begin(),tmp.end());
        int cnt = 0 ;
        int n = nums.size();
        int left = -1;
        int right = -1;
        for(int i=0;i<n;i++){
            if(nums[i] != tmp[i]){
                left = i;
                break;
            }
            
        }
        for(int i=n-1;i>=0;i--){
            if(nums[i] != tmp[i]){
                right = i;
                break;
            }
        }
        if(left == -1 or right == -1){
            return 0l;
        }
        return right-left+1;
    }
};
```

Stack Solution, kind of tricky.
```
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        stack<int> myStack;
        int l=nums.size(),r=0;
        for(int i=0 ; i<nums.size() ; i ++){
            while(!myStack.empty() && nums[myStack.top()] > nums[i]){
                l = min(myStack.top(),l);
                myStack.pop();
            }
            myStack.push(i);
        }
        stack<int> tmp;
        swap(tmp,myStack);
        for(int j=nums.size()-1;j>=0;j--){
            while(!myStack.empty() && nums[j] > nums[myStack.top()]){
                r = max(r,myStack.top());
                myStack.pop();
            }
            myStack.push(j);
        }
        return max(r-l+1, 0);
    }
};
```
<hr>

##  41. First Missing Positive
#### Solution
```
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for(int i=0;i<n;i++){
            while(nums[i] > 0 and nums[i] <=n and nums[nums[i]-1] != nums[i]){
                swap(nums[i],nums[nums[i]-1]);
            }
        }
        for(int i=0;i<n;i++){
            if(nums[i] != i+1){
                return i+1;
            }
        }
        return n+1;
    }
};
```

<hr>

## 134. Gas Station
The key is that if total gas larger than total cost, there must be a solution.

```
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int total = 0,sum = 0,start = 0;
        for(int i=0;i<gas.size();i++){
            total += gas[i] - cost[i];
            sum += gas[i] - cost[i];
            if(sum < 0){
                sum = 0;
                start = i+1;
            }
        }
        return total < 0 ? -1 : start;
    }
};
```

<hr>

## 209. Minimum Size Subarray Sum
#### Greedy
Find all possible sum that is exactly greater or equal to s. And compare.
```
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int len = nums.size();
        int left=0,right=0;
        int sum = 0;
        int result = len+1;
        while(right < len){
            while(sum < s and right < len){
                sum += nums[right++];
            }
            while(sum >= s){
                result = min(result,right-left);
                sum -= nums[left++];
            }
        }
        return result == len+1 ? 0 : result;
    }
};
```
#### Binary
Get prefix sum first, then binary search
<hr>

