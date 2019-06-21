---
layout: post
title:  "Leetcode Arrays 01"
categories: Interview
---
##  189. Rotate Array
```
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        if(nums.size() == 0 or k == 0){
            return;
        }
        k = k%(nums.size());
        reverse(nums.end()-k,nums.end());
        reverse(nums.begin(),nums.end()-k);
        reverse(nums.begin(),nums.end());
    }
};
```
<hr>

## 80. Remove Duplicates from Sorted Array II
Similair to just keep one for each number, just judge each time if there already exist two same number.

If this array is not duplicate, the time complexity is also O(N), because we can use an
`unordered_map<int,int>` to record the numebr of times for each number. And the insert and serach for hash map is O(1)
```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n=nums.size();
        if(n<=2){
            return n;
        }
        // pre is the index of last record number
        // pre+1 is the total length becasue including 0
        int pre=1,cur=2;
        while( cur < n ){
           if(nums[cur] == nums[pre]){
               if(nums[pre-1] == nums[pre]){
                   cur++;
                   continue;
               }
               nums[++pre] = nums[cur];
           }    
           else{
               nums[++pre] = nums[cur];
           }
           cur++;
        }
        return pre+1;
    }
};
```

<hr>

## 277	Find the Celebrity 
Time complexity O(n),because each time one node will be excluded!
```
int findCelebrity(int n) {
    vector<bool> isCelebrity(n,true);
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            if(isCelebrity[i] && i!=j){
                if(knows(i,j) or !knows(j,i)){
                    isCelebrity[i] = false;
                }
                else{
                    isCelebrity[j] = false;
                }
            }
        }
        if(isCelebrity[i]){
                return i;
        }
    }
    return -1;
    // Write your code here
}
```

<hr>

##  274. H-Index
```
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int sum=0;
        sort(citations.begin(),citations.end(),greater<int>());
        int i;
        for(i=0;i<citations.size();i++){
            // if less, then return last successful
            if(citations[i] >= i+1){;}
            else{
                return i;
            }
        }
        return i;
    }
};
```

<hr>

## 275 H-Index 2
```
int hIndex(vector<int>& citations) {
    reverse(citations.begin(),citations.end());
    int i;
    for(i=0;i<citations.size();i++){
        // if less, then return last successful
        if(citations[i] <i+1){
            return i;
        }
    }
    return i;
}
```
<hr>

## 220. Contains Duplicate III
First idea: find from nums[i]-t to nums[i]+t for each position; Time complexity would be O(n*t)

If t is large, then too slow

If we scan postion max(0,i-k) and min(n-1,i+k), then time complexity is O(n*k)

```
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        map<long long ,int> uMap;
        int j=0;
        for(int i=0;i<nums.size();i++){
            if(i-j >k) uMap.erase(nums[j++]);
            auto it = uMap.lower_bound((long long)nums[i]-t);
            if(it != uMap.end() && abs(it->first - nums[i]) <= t) return true;
            uMap[nums[i]] = i;
        }
        
        return false;
    }
};
```

Using lowerBound, time coplexity is O(n*logn) because BST search is O(logn)

##  122. Best Time to Buy and Sell Stock II
We can consider that how to get every possible profit.  Once we encouter `nums[i]<nums[i-1]`, we 
begin to consider the `nums[i-1]` as the new buying price. The reason can be easily see when drawing a graph.

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int totalSum = 0;
        int lastSum = 0;
        if(prices.size()<=1){
            return 0;
        }
        
        int lastMin = prices[0];
        for(int i=1;i<prices.size();i++){
            if(prices[i] < prices[i-1]){
                totalSum += lastSum;
                lastSum = 0;
                lastMin = prices[i];
            } 
            else{
                lastSum = max(lastSum,prices[i]-lastMin);
            }
        }
    
        return totalSum+lastSum;
    }
};
```