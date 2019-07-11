---
layout: post
title:  "Cracking the code interview"
categories: Interview
---


##Chapter 1 Array
手动实现一个resizable array
手动实现一个hashtable

print all palidrome permutation

https://www.geeksforgeeks.org/print-all-palindrome-permutations-of-a-string/

Permutation of duplicated string
一次打印出所有 


 rotated matrix 要做一下

cc 1.9

## Chapter 2 Linked List
实现链表，最好能够用一个wrapper封装head，不然如果有多个人使用了这个head，但head修改了这些用户并不知道

2.1 follow up
2.3
2.4 能不开数组吗
2.6 开数组
2.7开hash

## Chapter 3 Stacks and Quques
3.2 min o（1）？
3.5 每次pop要on？

## Chapter 4 Trees and Graphs
记住什么是complete binary tree和full binary  tree， perfect binary tree
如何判断是不是complted， full 和 perfect
都是用queue做level order traversal

complete 是不是最后一层，左边空右边不空
full 是不是有两个孩子，遍历即可
perfect level order ，计算每层个数（对于队列，如果有非空有空就是wrong


bidirectional search
丛k的d次方到2倍k的d/2次方

拓扑排序
最小生成树
AVL树

4.2 queue 加进去再放进queue层序遍历
4.6 栈 模拟递归
4.7 拓扑排序 实现一下
4。9 不会
4。11 关键是怎么delete一个node，每个点加入到hashmap中，一个找一个点删掉
4。12 每个点return一个possible value 的unotdered set，维护这个set即可
时间 on 空间on

## Chapter 5 Bit
#### Bit Manipulation
Get Bit
```
bool getBit(int num, int m){
    int mask = i << m;
    return num[m]&mask == 1;
}
```
Clear Bit
```
int ClearBit(int num, int m){
    int mask = i << m;
    num &= ~mask;
}
```
Set Bit
```
int SetBit(int num,int m){
    int mask = i << m;
    num |= mask;
}
```
Update Bit
```
int UpdateBit(int num,int m,bool isOne){
    num = ClearBit(num,m);
    if(isOne) return SetBit(num,m);
    else return num;
}
```
#### Problems

## Chapter 6 Math and Puzzles

#### 2's complement, logic right shift and arthimetic right shift



#### Prime Number: gcd, lcm
Let x = 2^l0 * 3^l1 * 5^l2 ...
Let y = 2^k0 * 3^k1 * 5^k2 ...

gcd(x,y) = 2^min(k0,l0) * 3^min(k1,l1) * 5^min(k2,l2) ...
lcm(x,y) = 2^max(k0,l0) * 3^max(k1,l1) * 5^max(k2,l2) ...

So lcm(x,y) * gcd(x,y) = x * y

#### Get Prime Number: The sieve of Eratosthenes
```
vector<bool> getNumberToN(int n){
    vector<bool> result(n+1,true);
    result[1] = false;
    int prime = 2;
    int length = 2;
    while(length*length <= n){
        crossOff(result,prime,n);
        prime = getNextPrime(nums);
    }
    return result;
}
void crossOff(vectro<bool>&nums,int prime,int n){
    // the reason why it is prime* prime is :
    // all numbers from 2*prime to (prime - 1)*prime are all crossed out, because we start from 
    // 2 to prime-1, so those are all crossed out.
    for(int i=prime*prime;i<=n;i += prime){
        nums[i] = false;
    }
}
int getNextPrime(vector<bool> &nums){
    for(int i=1;i<=nums.size()-1;i++){
        if(nums[i]){
            return i;
        }
    }
}
```

#### Problems: skipped

## Chapter 7 Object Oriented Design
Skiped
## Chapter 8 Recursion and DP

#### Problems:
#### Hanuta
```
Node = {1:"A",2:"B",3:"C"}
# move from node A to C
def hanuta(n, fromNode, toNode):
    print("n:%d,from%d,toNode%d" %(n,fromNode,toNode))
    if n > 1:
        hanuta(n-1, fromNode, 6-fromNode-toNode)
    print(Node[fromNode] + "->" + Node[toNode])
    if n > 1:
        hanuta(n-1, 6-fromNode-toNode, toNode )

print("hanuta 1:")
hanuta(1,1,3)
print("hanuta 2:")
hanuta(2,1,3)
print("hanuta 3:")
hanuta(3,1,3)
print("hanuta 5:")
hanuta(5,1,3)
```

#### Problems
1. 8.1 Triple Steps
```
def tripleStep(n):
    a = [0]*n
    a[0] = 1
    steps = [1,2,3]
    for i in range(0,n):
        for j in steps:
            if i+j < n :
                a[i+j] += a[i]
    return a[n-1]
```
2. 8.3 ??
3. 8.5 Recursive multiply with +,-,<<,>>
```
def recursiveMultiply(x,y):
    if y == 1:
        return x
    if y == 0:
        return 0
    mask = 1
    # y is not divisible by 2
    return recursiveMultiply(x<<1,y>>1) + x if y & mask == 1 else recursiveMultiply(x<<1,y>>1)

def wrapper(x,y):
    print("multiply %d and %d" %(x,y))
    print(recursiveMultiply(x,y))
```
4. 8.13?
5. 8.14??

## Chapter 9. System Design and Scalability
Skip

## Chapter 10. Sorting and Searching
#### MergeSort
```
void mergesort(vector<int>& array){
    vector<int> helper(array.size(),0);
    mergeSort(array,helper,0,array.size()-1);
}
void mergeSort(vector<int>&arr,vector<int>&helper,int low,int high){
    if(low >= high) return;
    int mid=(low+hight)/2;
    mergeSort(arr,helper,low,mid);
    mergeSort(arr,helper,mid+1,,high);
    mergeHelper(arr,helper,low,mid,hight);
}
void mergeHelper(vector<int>& arr,vector<int>&helper,int low,int mid,int hight){
    for(int i = low ; i <= hight ; i++){
        helper[i] = arr[i];
    }
    int first = low,second = mid+1;
    int cur = low;
    while(first<= mid or second <= right){
        if(first <= mid and second <= right){
            if(helper[first] < helper[second]){
                arr[cur] = helper[first];
                first++;
            }
            else{
                arr[cur] = helper[second];
                second++;
            }
        }
        else if( first <= mid){
            arr[cur] = helper[first];
            first++;
        }
        else{
            arr[cur] = helper[second];
            second++;
        }
        cur++;
    }
}
```

#### QuickSort
```
//contains left & right
void quickSort(vector<int>&arr, int left, int right){
    if( left >= right) return;
    int index = partition(arr,left, right);
    // index is already in the right place
    if( index - 1 > left){
        quickSort(arr,left,index-1);
    }
    if( index + 1 < right){
        quickSort(arr,index+1,right);
    }
}

void partition(vector<int>&arr, int left,int right){
    int mid = (left+right)/2;
    int pivot = arr[mid];
    int l = left,r = right;
    while(l <= r){
        while(arr[left] < pivot){
            l++;
        }
        while(arr[r] > pivot){
            r--;
        }
        if(l<=r){
            swap(arr[l],arr[r]);
            l++;
            r--;
        }
    }
    return l;
}
```

## Chapter 11. Test
Skipped


## Chapter 12. C++
Skipped

## Chapter 13 Java
Skipped


## Chapter 14. Databases
Skipped

## Chapter 15. Threads and Locks
Skipped

## Chapter 16. Moderate
* 16.1 Swap Number with XOR: a = a^b b = a^b a = a^b
* 16.2 Hash??
* 16.3 Intersection??
* 16.4 Tic Tac Win??
* 16.5 计算5的数量
* 16.6 Smallest Difference, Sort and compute diff
* 16.7 Number Max:??
* 16.8 English Int可以试试
* 16.9 Operations 试试
* 16.10 Living People 建立一个100的数组，count
* 16.11 ？？
* 16.12 不会
* 16.13 不会
* 16.14 不会
* 16.15 leetcode guess原题
* 16.16 原题
* 16.17 O(n)在线更新
* 16.18 双射
* 16.21 target is known: so sort both first, then for each one binary search, O(nlogn)

* 16.23 rand7 from rand5
This is equivalent to Adam Rosenfield's solution, but may be a bit more clear for some readers. It assumes rand5() is a function that returns a statistically random integer in the range 1 through 5 inclusive.
```
int rand7()
{
    int vals[5][5] = {
        { 1, 2, 3, 4, 5 },
        { 6, 7, 1, 2, 3 },
        { 4, 5, 6, 7, 1 },
        { 2, 3, 4, 5, 6 },
        { 7, 0, 0, 0, 0 }
    };

    int result = 0;
    while (result == 0)
    {
        int i = rand5();
        int j = rand5();
        result = vals[i-1][j-1];
    }
    return result;
}
```
How does it work? Think of it like this: imagine printing out this double-dimension array on paper, tacking it up to a dart board and randomly throwing darts at it. If you hit a non-zero value, it's a statistically random value between 1 and 7, since there are an equal number of non-zero values to choose from. If you hit a zero, just keep throwing the dart until you hit a non-zero. That's what this code is doing: the i and j indexes randomly select a location on the dart board, and if we don't get a good result, we keep throwing darts.

Like Adam said, this can run forever in the worst case, but statistically the worst case never happens. 

```
int i;
do
{
  i = 5 * (rand5() - 1) + rand5();  // i is now uniformly random between 1 and 25
} while(i > 21);
// i is now uniformly random between 1 and 21
return i % 7 + 1;  // result is now uniformly random between 1 and 7
```

* 16.24 二分搜索
* 16.25 LRU cache
* 16.26 计算器