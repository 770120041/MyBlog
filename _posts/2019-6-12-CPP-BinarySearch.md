---
layout: post
title:  "Binary Search"
categories: CPP
---

## Template
This template can easily access edges

```
int BinarySearch(vector<int> &nums, int target){
  int left=0,right=nums.size()-1;
  while(left+1<right){
    int mid = (left+right)/2;
    if(mid == target) return mid;
    else if(mid > target) left = mid;
    else right = mid;
  }
  if(left == target) return left;
  if(right == target) return right;
  return -1;
}

```

#### Follow up
If left+right is very large, just using Python to avoid overflow

<hr>


## STL
binary_search(startaddress, endaddress, valuetofind)
```
if (binary_search(a, a + 10, 2)) 
        cout << "\nElement found in the array"; 
    else
        cout << "\nElement not found in the array";
```

## Resource
[Here](https://github.com/yuzhoujr/leetcode/issues/8) is good summary of Leetcode Binary Search stategies 

## LeetCode 162
Left的落脚点一定是上升段，Right的落脚点一定是下降段，所以最后一定搜索到一个Peak
```
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        if(nums.size()<2) return 0;
        int n = nums.size();
     
        deque<int> tmp_nums(nums.begin(),nums.end());
        tmp_nums.push_front(INT_MIN);
        tmp_nums.push_back(INT_MIN);
        
        int left=1,right = n;
        while(left +1 < right){
            int mid = (left+right)/2;
            if(tmp_nums[mid]>tmp_nums[mid-1]){
                if(tmp_nums[mid] > tmp_nums[mid+1]){
                    return mid-1;
                }
                left = mid;
            }
            else{
                right = mid; 
            }
        }
        if(tmp_nums[left] > tmp_nums[left-1] && tmp_nums[left]>tmp_nums[left+1]){
            return left-1;
        }
        return right-1;
    }
};
```
