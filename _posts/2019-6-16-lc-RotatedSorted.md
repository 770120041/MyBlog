---
layout: post
title:  "Rotated Binary sorted Array"
categories: lc
---

## 33. Search in Rotated Sorted Array
The key to this problem is, how to determine which state the mid is. For nomral binary search, we compare the `nums[mid]` with target, while for this problem, we compare `nums[mid] with nums[begin] and nums[end]`

#### State
[solution example](http://bangbingsyb.blogspot.com/2014/11/leetcode-search-in-rotated-sorted-array.html)

There are many situations: first is array is not rotated, second is that it is rotated, then we need to find out the sorted part.

1. `If nums[mid] == target`. Done
2. `If nums[mid] < nums[end]`, then the part from `[mid,end]` is all sorted. If `target > nums[mid] && target < nums[end]`, then the answer is in `[mid,end]` part, otherwise in left part. 
3. The same for  `If nums[mid] > nums[end]`, this means that left part of the array is sorted.

This method reduces the range of search by half each time, Thus the time complexity would be O(N)

#### Recursive Solution

```
int helper(vector<int> &nums,int start,int end,int target){
    if(start > end) return -1;
    int mid = (start+end/2);
    if(nums[mid] == target) return mid;
    // if end part is sorted
    if(nums[mid] < nums[end]){
        // lies in end part
        if(target>nums[mid] && target <= nums[end]){
            return helper(nums,mid+1,end,target);
        }
        else{
            return helper(nums,start,mid-1,target);
        }
    }
    else{
        if(target>=nums[start] && target<nums[mid]){
            return helper(nums,start,mid-1,target);
        }
        else{
            return helper(nums,mid+1,end,traget);
        }

    }
}
int search(vector<int> &nums,int target){
    return helper(nums,0,nums.size()-1,target);
}
```

#### Iterative Solution
```
 int search(vector<int> &nums,int target){
    int left = 0, right =nums.size()-1;
    if(left>right) return -1;
    while(left <= right){
        int mid=(left+right)/2;
        if(nums[mid] == target){
            return mid;
        }
        if(nums[mid] < nums[right]){
            if(target > nums[mid] && target <= nums[right]){
                left = mid+1;
            }
            else{
                right = mid-1;
            }
        }
        else{
            if(target >= nums[left] && target < nums[mid]){
                right = mid-1;
            }
            else{
                left = mid+1;
            }
        }
    }
    return -1;
}
```

<hr>