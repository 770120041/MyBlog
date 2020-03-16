---
layout: post
title:  Python - Stack & priority Queue
categories: Interview
---
```py
class MinStack:
    def __init__(self):
        self.s = []
        self.min = 9999999999

    def push(self, x: int) -> None:
        if x <= self.min:
            self.s.append(self.min)
            self.min = x
        self.s.append(x)
        # print(self.s)

    def pop(self) -> None:
        ret = self.s.pop()
        if ret == self.min:
            self.min = self.s.pop()
        return ret

    def top(self) -> int:
        return self.s[-1]

    def getMin(self) -> int:
        return self.min


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

## 232. Implement Queue using Stacks
```py
class MyQueue:
    def __init__(self):
        self.s1 = []
        self.s2 = []
        
    # pop the elment to another stack, so the head will be in top of stack.
    def push(self, x: int) -> None:
        self.s2.append(x)
        
    def pop(self) -> int:        
        self.peek()
        return self.s1.pop()    
        return s1.pop()
        
    def peek(self) -> int:
        if not self.s1:
            while self.s2:
                self.s1.append(self.s2.pop())
        return self.s1[-1]
        
    def empty(self) -> bool:
        return len(self.s1)+len(self.s2) == 0

# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

## 225. Implement Stack using Queues

```cpp
class MyStack {
public:
    queue<int> q1,q2;
    /** Initialize your data structure here. */
    MyStack() {
        
    }
    
    /** Push element x onto stack. */
    void push(int x) {
        q2.push(x);
        while(!q1.empty()){
            q2.push(q1.front()); q1.pop();
        }
        q1 = q2;
        q2 = queue<int>();  
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int ret = q1.front(); q1.pop();
        return ret;
    }
    
    /** Get the top element. */
    int top() {
        return q1.front();
    }
    
    /** Returns whether the stack is empty. */
    bool empty() {
        return q1.empty();
    }
};
```

```py
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.q1 = []
        self.q2 = []

    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.q1.append(x)

    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        for i in range(len(self.q1) - 1):
            self.q2.append(self.q1.pop(0))
        res = self.q1.pop(0)
        self.q1 = self.q2
        self.q2 = []
        return res
        

    def top(self) -> int:
        """
        Get the top element.
        """
        return self.q1[-1]
        

    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return len(self.q1) == 0
        
```

## 150. Evaluate Reverse Polish Notation
```py
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        s = []
        for token in tokens:
            if ("0"<=token[0]<="9") or (token[0] == "-" and len(token) != 1):
                s.append(int(token))
            else:
                num2 = s.pop()
                num1 = s.pop()
                if token == "+": s.append(num1+num2)  
                elif token == "-": s.append(num1-num2)
                elif token == "*": s.append(num1*num2)
                elif token == "/": 
                    if num1*num2 < 0:
                        s.append(-(abs(num1)//abs(num2)))
                    else:
                        s.append(num1//num2)
        return s[-1]
```

## 71. Simplify Path
```py
class Solution:
    def simplifyPath(self, path: str) -> str:
        tokens = path.split("/")
        s = []
        for token in tokens:
            if not token or token == ".": continue
            elif token == "..":
                if s: s.pop()
            else:
                s.append(token)
        return "/"+"/".join(s)
```

## 388. Longest Absolute File Path
```py
class Solution:
    def lengthLongestPath(self, input):
        maxlen = 0
        pathlen = {0: 0}
        for line in input.splitlines():
            name = line.lstrip('\t')
            # \t的个数就是depth
            depth = len(line) - len(name)
            if '.' in name:
            # 文件夹的深度加文件的长度
                maxlen = max(maxlen, pathlen[depth] + len(name))
            else:
            # 设置文件夹的深度
                pathlen[depth + 1] = pathlen[depth] + len(name) + 1
        return maxlen
```

## 341. Flatten Nested List Iterator
https://www.cnblogs.com/grandyang/p/5358793.html

注意这个从后往前压入的操作以及hasNext中要用while self.s而不是if self.s，这样才能保证栈顶是integer
```py

class NestedIterator(object):

    def __init__(self, nestedList):
        self.s = []
        for i in range(len(nestedList)-1,-1,-1):
            self.s.append(nestedList[i])
        
    # @return {int} the next element in the iteration
    def next(self):
        # Write your code here
        return self.s.pop().getInteger()
    # @return {boolean} true if the iteration has more element or false
    def hasNext(self):
        while self.s:
            if self.s[-1].isInteger():
                return True
            l = self.s.pop().getList()
            for i in range(len(l)-1,-1,-1):
                self.s.append(l[i])
        return False

```

## 385. Mini Parser
https://www.cnblogs.com/grandyang/p/5771434.html

cnt用来记录深度，如果是同一深度的话再到下一层parse， 复杂度O(n^2)

注意`res.add(self.deserialize(s[start:i]))`,这里第i个位置是","或者“]”,所以最后一个位置不取

```py
class Solution:
    def deserialize(self, s: str) -> NestedInteger:
        if not s: return NestedInteger()
        if s[0] != "[": return NestedInteger(int(s))
        # case "[]"
        if len(s) <= 2: return NestedInteger()
        res = NestedInteger()
        start = 1
        # cnt 用于括号匹配
        cnt = 0
        for i in range(1,len(s)):
            if cnt == 0 and (i == len(s)-1 or s[i] == ","):
                res.add(self.deserialize(s[start:i]))
                start = i+1
            elif s[i] == "[": cnt += 1
            elif s[i] == "]": cnt -= 1
        return res
```

#### Non iterative
O(N)复杂度

遇到左括号加入栈，遇到逗号或者右括号，说明数字可以进栈了， parse数字，然后如果是右括号，说明当前list结束，把当前list加入到它的父节点中。

```
class Solution:
    def deserialize(self, s: str) -> NestedInteger:
        if not s: return NestedInteger()
        if s[0] != "[": return NestedInteger(int(s))
        st = []
        start = 1
        for i in range(len(s)):
            if s[i] == "[":
                # this is current list
                st.append(NestedInteger())
                start = i+1
            elif s[i] == "]" or s[i] == ",":
                if i > start:
                    st[-1].add(NestedInteger(int(s[start:i])))
                start = i+1
                if s[i] == "]":
                    if len(st) > 1:
                        curL = st.pop()
                        st[-1].add(curL)
        return st[-1]
```


# Binary Heap

下标：
```
如果是从0开始，
parent: (i-1)/2
left:   2*i+1
right:  2*i + 2

如果是从1开始：
parent: i/2
left:   2*i
right:  2*i+1
```

插入一个数：插入到尾部（不管有没有超过size，然后swim up)
删除一个数：头和尾部交换然后heapify(0)

## 215	Kth Largest Element in an Array
Have enought space: sorting O(Nlogn)

If we don't have enough space: heap O(Nlogn)
If we have: O(N) to worse case O(N^2) using partition of quick sort
https://www.programcreek.com/2014/05/leetcode-kth-largest-element-in-an-array-java/


Quick sort solution
```py
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        def findPartition(nums,start,end,k):
            pivot = end
            l,r = start,end
            while l < r:
                while l < r and nums[l] <= nums[pivot]: l+=1
                while l < r and nums[r] >= nums[pivot]: r-=1
                nums[l],nums[r] = nums[r],nums[l]
            nums[l],nums[pivot] = nums[pivot],nums[l]
            pivot = l
            # print(start,end,pivot)
            if pivot == k:
                return nums[pivot]
            if pivot > k:
                return findPartition(nums,start,pivot-1,k)
            else:
                return findPartition(nums,pivot+1,end,k)
        return findPartition(nums,0,len(nums)-1,len(nums)-k)
```



## 347	Top K Frequent Elements
python how to sort by values: `for k, v in sorted(x.items(), key=lambda item: item[1])`
#### Sorting
```py
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
       # 加到dict，dict排序（根据value）,注意要从大到小（最频繁的最前面）
        d = {}
        for num in nums:
            if num not in d: d[num] = 1
            else: d[num]+=1
        res = []
        cnt = 0
        for ks,vs in sorted(d.items(),key = lambda item: item[1],reverse=True):
            cnt += 1
            res.append(ks)
            if cnt == k:
                break
        return res
```

#### Heap
先map到dict，再用priority queue，和排序差不多

Idea is very straightforward:

build a counter map that maps a num to its frequency
build a heap/priority queue that keeps track of k most significant entries
iterate through the final heap and get the keys
```java
public List<Integer> topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> counterMap = new HashMap<>();
    for(int num : nums) {
        int count = counterMap.getOrDefault(num, 0);
        counterMap.put(num, count+1);
    }
    
    PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>((a, b) -> a.getValue()-b.getValue());
    for(Map.Entry<Integer, Integer> entry : counterMap.entrySet()) {
        pq.offer(entry);
        if(pq.size() > k) pq.poll();
    }
    
    List<Integer> res = new LinkedList<>();
    while(!pq.isEmpty()) {
        res.add(0, pq.poll().getKey());
    }
    return res;
}
```
#### bucket sort
频数最大只能是num[i]，所以准备一个nums[i]的数组，然后把cntmap的k,v反过来插进去
然后前k个的话就从数组里面取（注意同频率可能有很多个，所以bucket里面要在用一个数组存储
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        vector<int> res;
        if (!nums.size()) return res;
        unordered_map<int, int> cnt;
        for (auto num : nums) cnt[num]++;
        vector<vector<int>> bucket(nums.size() + 1);
        for (auto kv : cnt) {
            bucket[kv.second].push_back(kv.first);
        }

        for (int i = bucket.size() - 1; i >= 0; --i) {
            for (int j = 0; j < bucket[i].size(); ++j){
                res.push_back(bucket[i][j]);
                if (res.size() == k) return res;
            }
        }

        return res;
    }
};
```

```py
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        d = {}
        for num in nums:
            if num not in d: d[num] = 1
            else: d[num] += 1
        bucket = [[] for _ in range(len(nums)+1)]
        for key,value in d.items():
            bucket[value].append(key)
        res = []
        for i in range(len(bucket)-1,-1,-1):
            if bucket[i]:
                for j in bucket[i]:
                    res.append(j)
                    if len(res) == k: return res
        return res
```

## 263. Ugly Number
```py
class Solution:
    def isUgly(self, num: int) -> bool:
        primes = [2,3,5]
        for prime in primes:
            while num >= prime and num % prime == 0:
                num = num // prime
        return num == 1
``` 

## 264. Ugly Number II 丑陋数之二
https://www.cnblogs.com/grandyang/p/4743837.html

这个题目的思想很有意思，从3个list里面merge最小的。
因为每个ugly number 都是从前面的ugly number乘以prime得到的，所以每个prime都能生成一个list.

为了防止merge 重复的数，加入list之前检查是不是和尾部相同
#### Math solution
```py
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        res = [1]
        i2,i3,i5 = 0,0,0
        while len(res) < n:
            m2,m3,m5 = res[i2]*2,res[i3]*3,res[i5]*5
            m = min(m2,m3,m5)
            if m2 == m: i2+=1
            elif m3 == m: i3+=1
            else: i5 += 1
            if m != res[-1]:
                res.append(m)
        print(res)
        return res[n-1]
```
#### Heap solution
我们也可以使用最小堆来做，首先放进去一个1，然后从1遍历到n，每次取出堆顶元素，为了确保没有重复数字，进行一次 while 循环，将此时和堆顶元素相同的都取出。然后分别将这个取出的数字乘以 2，3，5，并分别加入最小堆。这样最终 for 循环退出后，堆顶元素就是所求的第n个丑陋数，参见代码如下：


```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> result;
        priority_queue<long long,vector<long long >,greater<long long >> pq;
        pq.push(1);
        for(int i=1;i<n;i++){
            auto topNum = pq.top(); pq.pop();
            while(pq.size() > 0 and pq.top() == topNum){
                pq.pop();
            }
            pq.push(topNum*2);
            pq.push(topNum*3);
            pq.push(topNum*5);
        }
        return pq.top();
    }
};
```

## 1201. Ugly Number III
和ugly number II不一样，ugly number是能够被a或b，或c整除的
#### Brute force 穷举（TLE）
```cpp
class Solution:
    def nthUglyNumber(self, n: int, a: int, b: int, c: int) -> int:
        cur = min(a,b,c)
        for i in range(n):
            while cur % a != 0 and cur % b != 0 and cur%c != 0:
                cur += 1
            if i == n-1:
                return cur
            cur+=1    
```

#### binary search
https://leetcode.com/problems/ugly-number-iii/discuss/387539/cpp-Binary-Search-with-picture-and-Binary-Search-Template

This problem can easily solved by binary-search by defining the predicate: "Given N, how many numbers upto N are divisible by A or B or C"

```
No. of numbers upto N divisible by A = N/A;
No. of numbers upto N divisible by B = N/B;
No. of numbers upto N divisible by C = N/C;

No. of numbers upto N divisible by both A and B = N / lcm(A, B);
No. of numbers upto N divisible by both B and C = N / lcm(B, C);
No. of numbers upto N divisible by both A and C = N / lcm(A, C);

No. of numbers upto N divisible by all A, B and C = N / lcm(A, B, C);
```
Furthermore LCM(A, B) can be computed as LCM(A, B) = (A*B) / GCD(A, B)

So when target is increasing, the number that divisible to a,b,c are increasing. So a logN solution is generated(N is the uppber bounds of given bound)

## 313	Super Ugly Number
```py
class Solution {
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        priority_queue<int,vector<long long>,greater<long long>> pq;
        pq.push(1);
        for(int i=1;i<n;i++){
            auto topN = pq.top(); pq.pop();
            while(pq.size() > 0 && pq.top() == topN){
                pq.pop();
            }
            for(int i=0;i<primes.size();i++){
                pq.push(topN*primes[i]);
            }
        }
        return pq.top();
    }
};
```

## 373	Find K Pairs with Smallest Sums
```py
class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        vector<vector<int>> res;
        auto comp = [](pair<int,int> a, pair<int,int> b ) { return a.first+a.second > b.first+b.second; };
        priority_queue< pair<int,int>,vector<pair<int,int>>,decltype(comp)> pq(comp);
        for(auto num1: nums1){
            for(auto num2:nums2){
                pq.push(make_pair(num1,num2));
            }
        }
        for(int i=0;i<k and !pq.empty();i++){
            vector<int> cur{pq.top().first,pq.top().second};
            res.push_back(cur);
            pq.pop();
        }
        return res;
    }
};
```

## 332	Reconstruct Itinerary
#### Recursion
这道题给我们一堆飞机票，让我们建立一个行程单，如果有多种方法，取其中字母顺序小的那种方法。这道题的本质是有向图的遍历问题，那么LeetCode关于有向图的题只有两道Course Schedule和Course Schedule II，而那两道是关于有向图的顶点的遍历的，而本题是关于有向图的边的遍历。每张机票都是有向图的一条边，我们需要找出一条经过所有边的路径，那么DFS不是我们的不二选择。先来看递归的结果，我们首先把图建立起来，通过邻接链表来建立。由于题目要求解法按字母顺序小的，那么我们考虑用multiset，可以自动排序。等我们图建立好了以后，从节点JFK开始遍历，只要当前节点映射的multiset里有节点，我们取出这个节点，将其在multiset里删掉，然后继续递归遍历这个节点，由于题目中限定了一定会有解，那么等图中所有的multiset中都没有节点的时候，我们把当前节点存入结果中，然后再一层层回溯回去，将当前节点都存入结果，那么最后我们结果中存的顺序和我们需要的相反的，我们最后再翻转一下即可，参见代码如下：

注意这里要保证dict排序，所以我们逆序排序，然后再倒过来使用这个dict。这里的这种方式可以逆序访问这张图（反过来的拓扑排序）
```py
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        # DFS
        self.res = []
        def dfs(curPos="JFK"):
            while curPos in self.d and self.d[curPos]:
                neighbor = self.d[curPos].pop()
                dfs(neighbor)
            self.res.append(curPos)

        self.d = {}
        for ticket in tickets:
            if ticket[0] not in self.d: self.d[ticket[0]] = []
            self.d[ticket[0]].append(ticket[1])
        # sort the cities
        for key in self.d.keys():
            self.d[key].sort(reverse=True)
        dfs()
        self.res.reverse()
        return self.res
```

#### Using stack
下面我们来看迭代的解法，需要借助栈来实现，来实现回溯功能。比如对下面这个例子：

tickets = [["JFK", "KUL"], ["JFK", "NRT"], ["MRT", "JFK"]]

那么建立的图如下：

JFK -> KUL, NRT

NRT -> JFK

由于multiset是按顺序存的，所有KUL会在NRT之前，那么我们起始从JFK开始遍历，先到KUL，但是KUL没有下家了，这时候图中的边并没有遍历完，此时我们需要将KUL存入栈中，然后继续往下遍历，最后再把栈里的节点存回结果即可，参见代码如下：

 
```py
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        # 用stack，那么模拟递归
        res = []
        s = ["JFK"]
        d = {}
        for ticket in tickets:
            if ticket[0] not in d: d[ticket[0]] = []
            d[ticket[0]].append(ticket[1])
        for key in d.keys():
            d[key].sort(reverse=False)
        while s:
            cur = s[-1]
            if cur not in d or len(d[cur]) == 0:
                res.insert(0,cur)
                s.pop()
            else:
                s.append(d[cur].pop(0))
        return res
```



## 218	The Skyline Problem
https://www.youtube.com/watch?v=GSBLe8cKu0s
https://www.cnblogs.com/grandyang/p/4534586.html

Each time, we record the index when the max height changes. So we use a multiset, for each building, when it starts, we push it to the multiset, when it ends, we pop if from the multiset, both operations is O(N) time, and after each opeartions, we check if the max height is changing, if so we record the index.
```cpp
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        vector<vector<int>> res;
        vector<pair<int,int>> height;
        for(auto building : buildings){
            height.push_back(make_pair(building[0],-building[2]));
            height.push_back(make_pair(building[1],building[2]));
        }
        multiset<int> m;
        sort(height.begin(), height.end());
        int pre=0,cur;
        // init height is 0, after all operations, it should go back to 0
        m.insert(0);
        for(auto h:height){
            if(h.second < 0){
                m.insert(-h.second);
            }
            else{
                m.erase(m.find(h.second));
            }
            // rbegin is the maximum value
            cur = *m.rbegin();
            if(pre != cur){
                vector<int> tmp{h.first,cur};
                res.push_back(tmp);
                pre = cur;
            }
            
        }
        return res;
    }
};
```

https://briangordon.github.io/2014/08/the-skyline-problem.html

## 84	Largest Rectangle in Histogram