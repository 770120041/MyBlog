---
layout: post
title:  "Search Sorted Matrix"
categories: Interview
---
## 74. Search a 2D Matrix
Binary search 

#### Iterative Solution
```
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if matrix == [[]] :
            return False
        left,right = 0, len(matrix)-1
        Pivot = -1
        while left <= right:
            mid = (left+right)/2
            if target >= matrix[mid][0]:
                if mid == len(matrix)-1 or target < matrix[mid+1][0]:
                    Pivot = mid
                    break
                # target > matrix[mid+1]
                left = mid+1
            else:
                right = mid-1
        # print Pivot
        if Pivot == -1:
            return False
        
        left,right = 0,len(matrix[Pivot])-1
        while left <= right:
            mid = (left+right)/2
            if target == matrix[Pivot][mid]:
                return True
            if target >= matrix[Pivot][mid]:
                left = mid+1
            else:
                right = mid-1
        return False
```

## 240. Search a 2D Matrix II
The properity makes it easy to reduce search ranges. Also by the properities, we can know the matirx's diagonal is always increasingn. This time, it is an N*M matrix

But we can't use diagonal to minimize range, because like shape, 15 is smaller than 17 but in last column.     
```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
 
Actually, for example, if our target is 15, then we firstly look at 9, 15 is larger than 9, so the result would be in , so actually each miss will reduce the left upper part of the matrix. We can use recursive method to Solve this problem
```
[              LeftM     MidM    RightM
leftN           [X,   X,  X, X, X],
                [X,   X,  X, X, X],
MidN            [X,   X,  Mid, 16, 22],
                [X,   X,  14, 17, 24],
RightN          [X,   X,  23, 26, 30]
]
```
We use 4 variables to track search area leftN, rightN,leftM,rightM. leftN and rightN are used to track horizontal range, rightV and leftV are used to track Vertical range. Those are both inclusive.

For example, if the Mid value is larger than target, then we need to find all elements marked with X. We have known the corrdinate of Mid, so we can make this search as two seperate parts.
`Matrix[leftN:rightN][leftM:midM-1] and Matrix[leftN:midN-1][midM,rightM]`

If Mid value is smaller than the target, we need to find all elements makred with X. This means we need to find from `Matrix[midN+1:rightN][leftM:rightM] and Matrix[leftN:midN][midM+1,rightM]`
```
[
  [1,   4,  7, X, X],
  [2,   5,  8, X, X],
  [3,   6,  Mid, X, X],
  [X, X, X, X, X]
  [X, X, X, X, X]
]


```
class Solution(object):
    
    # l1,r1 horizontal, l2,r2, vertical
    def helper(self,matrix,target,leftN,rightN,leftM,rightM):
        
        if leftN>rightN or leftM>rightM:
            return False
        midN, midM = (leftN + rightN)/2, (leftM + rightM)/2
        # print("midN=%d,midM=%d",midN,midM)
        if target == matrix[midN][midM]:
            return True
        if target < matrix[midN][midM]:
            return self.helper(matrix,target,leftN,rightN,leftM,midM-1) or self.helper(matrix,target,leftN,midN-1,midM,rightM)
        else:
            return self.helper(matrix,target,midN+1,rightN,leftM,rightM) or self.helper(matrix,target,leftN,midN,midM+1,rightM)

        
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if matrix == []:
            return False
        if matrix == [[]]:
            return False
        n = len(matrix)
        m = len(matrix[0])
        return self.helper(matrix,target,0,n-1,0,m-1)
```
