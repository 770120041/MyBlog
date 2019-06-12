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
