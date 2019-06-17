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

## 81. Search in Rotated Sorted Array II
The key is remove duplicate ones, also ,how to judge which part is sorted.

For my implementation, if we remove duplicate ones from right, then if `nums[mid] <= nums[right]`, we can make sure right part is sorted! 


#### Resurcive Solution
```
bool helper(vector<int> &nums,int start,int end,int target){
    if(left > right) return false;
    while(nums[left] == nums[right] && left<right){
        right -- ;
    }
    int mid = (left+right)/2;
    if(nums[mid] == target) return mid;
    if(nums[mid] <= nums[right]){
        if(target > nums[mid] && target <= nums[right]){
            helper(vector<int> &nums, mid+1,right, target);
        }
        else{
            helper(vector<int> &nums, left,mid-1, target);
        }
    }
    else{
        if(target >= nums[start] && target < nums[mid]){
           helper(vector<int> &nums, left,mid-1, target);
        }
        else{
            helper(vector<int> &nums, mid+1,right, target);
        }
    }
}
bool search(vector<int>& nums, int target) {
    return helper()
}
```
#### Iterative Solution
```
 bool search(vector<int>& nums, int target) {
    int left=0,right=nums.size()-1;
    while(left<=right){
        while(nums[left] == nums[right] && left<right){
            right--;
        }
        int mid = (left+right)/2;
        // printf("left=%d,right=%d,mid=%d\n",left,right,mid);
        if(nums[mid] == target) return true;
        // right part is sorted, if nums[right]==nums[mid], it can't be in right part because nums[mid] != target
        if(nums[mid] <= nums[right]){
            if(target > nums[mid] && target <= nums[right]){
                left=mid+1;
            }
            else{
                right=mid-1;
            }
        }
        else{
            // printf("left is sorted\n");
            if(target >= nums[left] && target < nums[mid]){
                right = mid-1;
            }
            else{
                left = mid+1;
            }
        }
    }
    return false;
}
```
<hr>

## 153. Find Minimum in Rotated Sorted Array
The key is that minimum is either in `nums[0] or nums[nums.size()-1]` and less than all others, or it is less than its neighbors.

Because we need to solve edge condition, like `nums[0] or nums[nums.size()-1]` are minimum, so using Python would be easier to write. For C++, we can add dummy element to postion 0 and n, and right shift the element, which would also solve this problem. For C, just creating array staring in postion 1.

#### Iterative Solution
```
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        if n < 2: return nums[0]
        left,right = 0, n-1
        while left+1<right:
            mid = (left+right)//2
            # print "left=",left," right=",right," mid=",mid
            if nums[mid] < nums[mid-1] and mid < nums[mid+1]:
                return nums[mid]
            if nums[mid] < nums[right]:
                right=mid
            else:
                left=mid
        if nums[left]<nums[left+1] and nums[left] < nums[left-1]:
            return nums[left]
        return nums[right]
```

#### Recursive Solution
```
class Solution(object):
    def helper(self,nums,left,right):
        # print "left=%d,right=%d" %(left,right)
        if left+1 >= right:
            if nums[left] < nums[right]: 
                return nums[left]
            return nums[right]
       
        mid = (left+right)//2
        
        # print "mid=%d" %(mid)
        if nums[mid] < nums[mid-1] and mid < nums[mid+1]:
            return nums[mid]
        
        if nums[mid] < nums[right]:
            return self.helper(nums,left,mid)
        else:
            return self.helper(nums,mid,right) 
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        return self.helper(nums,0,len(nums)-1)
        
```

<hr>

## 154. Find Minimum in Rotated Sorted Array II
The array may contain **duplicates**.

Using same strategy to handle duplicates

Do remember that we can using the way to find pivot to decrese the size of search by half.

If we find the increasing part, then find its counterpart

#### Iterative Solution
```
def findMin(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    n = len(nums)
    if n < 2: return nums[0]
    result = nums[0]
    left,right = 0, n-1
    
    while left+1<right:
        while nums[left] == nums[right] and left < right:
            right -= 1
        mid = (left+right)/2
        result = min(result,nums[mid])
        
        # right part is incresing
        if nums[mid] <= nums[right]:
            right = mid
        else:
            left = mid
    result = min(result,nums[left],nums[right])
    
    return result
        
```
Recursive Solution
```
class Solution(object):
    def helper(self,nums,left,right,result):
        if left+1 >= right:
            result = min(result,nums[left],nums[right])
            return result
        
        while nums[left] == nums[right] and left<right:
            right -= 1
        mid = (left+right)/2
        result = min(nums[mid],result)
        if nums[mid] <= nums[right]:
            return self.helper(nums,left,mid,result)
        else:
            return self.helper(nums,mid,right,result)
        
    def findMin(self, nums):
        n = len(nums)
        return self.helper(nums,0,n-1,nums[0])
```