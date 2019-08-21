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
        int sum = 0;
        for(int x:citations) sum+=x;
        if(sum == 0 or citations.size() == 0) return 0;
        sort(citations.begin(),citations.end());
        for(int i=0;i<citations.size();i++){
            int h = citations.size()-i;
            if(citations[i] >= h) return h;
        }
        return 0;
    }
};
```

<hr>

## 275 H-Index 2
#### Brute Force
```
class Solution {
public:
    int hIndex(vector<int>& citations) {
        for(int i=0;i<citations.size();i++){
            int h = citations.size() - i;
            if(citations[i] >= h){
                return h;
            }
        }
        return 0;
    }
};
```
#### Binary Search
```
class Solution {
public:
    //找到恰好大于h的点
    int hIndex(vector<int>& citations) {
        int n = citations.size();
        if(n == 0) return 0;
        int l = 0, r = n-1;
        //finally l+1 == r
        while(l+1<r){
            int mid = (l+r)/2;
            if(citations[mid] >= n-mid){
                r = mid;
            }
            else{
                l = mid;
            }
        }
        if(citations[l] >= n-l) return n-l;
        if(citations[r] >= n-r) return n-r;
        return 0;
    }
};
```

<hr>

## 219. Contains Duplicate II
#### Hash 
```
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int,int> uMap;
        for(auto i = 0 ; i < nums.size() ; i++){
            auto it = uMap.find(nums[i]);
            if(it == uMap.end()){}
            else{
                if(i - it->second <= k) return true;
            }
            uMap[nums[i]] = i;
        }
        return false;
    }
};
```

<hr>


## 220. Contains Duplicate III

First idea: find from nums[i]+1 to nums[i]+t for each position; Time complexity would be O(n*t)(O(n^2))



#### Error solution
```
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        map<long long,int> m;
        for(int i = 0 ; i < nums.size() ; i++){

            auto it = m.lower_bound((long long)nums[i] - t);
            if(it != m.end() and abs(it->first-nums[i]) <= t and abs(it->second-i)<=k) return true;
            m[nums[i]] = i;
        }
        return false;
    }
};
```

Example Input
```
[3,6,0,4]
2
2
```
The reason is that lower_bound will fetch 3, not 6 as expected. So we need to exclude numbers that the index is not legal.


#### Correct solution
Using lowerBound, time coplexity is O(n*logn) because BST search is O(logn)
```
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        map<long long, int > m;
        for(int i=0,j=0;i<nums.size();i++){
            if(i-j>k){
                if(i - m[nums[j]] > k) m.erase(nums[j]);
                j++;
            }
            auto it = m.lower_bound((long long)nums[i]-t);
            if(it != m.end() and abs(it -> first - nums[i]) <= t) return true;
            m[nums[i]] = i;
        }
        return false;
    }
};
```
Be careful we shall not remove directly ` if(i-j >k) uMap.erase(nums[j++]);` because from i to j there may be same number as nums[j], so check if the `u[nums[j]]` is filled by `nums[j]` is better
```
if(i-j>k){
    if(i - m[nums[j]] > k) m.erase(nums[j]);
    j++;
}
```


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

## 287. Find the Duplicate Number
Fast slow pointer, regard this numbers as a linked list
```
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow=0,fast=0,t=0;
        while(true){
            slow = nums[slow];
            fast = nums[nums[fast]];
            if(slow == fast) break;
        }
        while (true) {
            slow = nums[slow];
            t = nums[t];
            if (slow == t) break;
        }
        return slow;
    }
};
```

## 334. Increasing Triplet Subsequence
DP is to use dp[i] as the numbers that smaller than i, O(n) to get the result of dp

Or we can use two numbers to maintain the two large elements

```
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int m1=INT_MAX,m2=INT_MAX;
        for(int i=0;i<nums.size();i++){
            if(nums[i] > m1) return true;
            else if (nums[i] > m2) m1 = nums[i];
            else{
                // m1 = INT_MAX;
                m2 = nums[i];
            }
        }
        return false;
    }
};
```
There are one thing that need to be considered, 
```
else{
    // m1 = INT_MAX;
    m2 = nums[i];
}
```
Why we don't need to update m1 = INT_MAX. The reason is that if `nums[i] > m1`, then there is some numbers before m1 that is smaller than m1(otherwise `m1 = INT_MAX`), if we update m1, we may miss some solutions.

<hr>

## 56. Merge Intervals

```
class Solution {
public:
    static bool cmp(vector<int> &a,vector<int> &b){
        return a[0]<b[0] or (a[0]==b[0] and a[1] < b[1]);
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        sort(intervals.begin(),intervals.end(),cmp);
        for(auto interval : intervals){
            if(result.empty()){
                result.push_back(interval);
            }
            else{
                auto cur = result.back();
                if(cur[1] >= interval[0]){
                    result.pop_back();
                    cur[1] = max(interval[1],cur[1]);
                    result.push_back(cur);
                }
                else{
                    result.push_back(interval);
                }
            }
        }
        return result;
    }
};
```

<hr>

## 128. Longest Consecutive Sequence
#### SET solution
O(nlogn) but passed leetcode OJ, which is not good
```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        set<int> s(nums.begin(),nums.end());
        int result = 0;
        int curSize = 1;
        bool first = true;
        int last = -1; 
        for(auto it = s.begin() ; it != s.end() ; it++){
            if(first == false and *it == last+1){
                curSize ++;
            }
            else{
                curSize = 1;
                first = false;
            }
            result = max(curSize,result);
            last = *it;
        }
        return result;
    }
};
```

#### Hash Solution
Very simple, add all to a hash set, for each number, find all its adjacent neighbors and update length. It is O(n) because all number is add to hash and pop to hash only once. **Remember to del it after one number is calculated**, Otherwise it will be O(N^2);
```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> uset;
        for(auto x : nums) uset.insert(x);
        int result = 0;
        for(auto x: nums){
            int len = 1,tmp=x+1;
            while(uset.count(tmp)){
                uset.erase(tmp);
                len++;
                tmp++;
            }
            tmp = x-1;
            while(uset.count(tmp)){
                uset.erase(tmp);
                len++;
                tmp--;
            }
            result = max(result,len);
        }
        return result;
    }
};
```

<hr>