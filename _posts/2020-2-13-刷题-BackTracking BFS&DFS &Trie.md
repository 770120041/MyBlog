---
layout: post
title:  Python BackTracking & BFS,DFS & Trie
categories: Interview
---
## 200 islands
```py
class Solution(object):
    def numIslands(self, grid):
        def sink(i, j):
            if 0 <= i < len(grid) and 0 <= j < len(grid[i]) and grid[i][j] == '1':
                grid[i][j] = '0'
                map(sink, (i+1, i-1, i, i), (j, j, j+1, j-1))
                return 1
            return 0
        return sum(sink(i, j) for i in range(len(grid)) for j in range(len(grid[i])))

```

```py
class Solution(object):
    def numIslands(self, grid):
        def helper(g,i,j):
            if i < 0 or j < 0 or i >= len(g) or j >= len(g[0]) or g[i][j] == "0": return
            g[i][j] = "0"
            helper(g,i-1,j)
            helper(g,i+1,j)
            helper(g,i,j-1)
            helper(g,i,j+1)
        cnt = 0
        for x in range(len(grid)):
            for y in range(len(grid[0])):
                if grid[x][y] == "1":
                    cnt+=1
                    helper(grid,x,y)
        return cnt
```


## 130. Surrounded Regions
虽然简单但还是要注意：
1. 首先对对每个边上的‘O’做DFS，把连接到的改成‘M'
2. 对内部的点DFS,把遇到的联通点改成'X'
3. 把'M'改成"O"
```py
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        def dfs(board,i,j,target):
            if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]) or board[i][j] == 'X' or board[i][j] == 'M': return
            board[i][j] = target
            dfs(board,i-1,j,target)
            dfs(board,i+1,j,target)
            dfs(board,i,j-1,target)
            dfs(board,i,j+1,target)
        if len(board) <= 1 or len(board[0]) <= 1: return
        for i in range(len(board)):
            for j in range(len(board[0])):
                if i == 0 or i == len(board)-1 or j==0 or j == len(board[0])-1:
                    dfs(board,i,j,'M')
        for i in range(len(board)):
            for j in range(len(board[0])):
                dfs(board,i,j,'X')
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] == 'M': board[i][j]='O'
        
```

## 663. Walls and Gates
```py
    def wallsAndGates(self, rooms):
        # find gate, for each gate, add to the queue
        # while the queue is not empty, pop one node from the queue, if its neighbors are larger than its value, add them to the queue
        def visitNeighbor(r,q,x,y,dis):
            if x<0 or x >= len(r) or y < 0 or y>=len(r[0]) or r[x][y]<=dis: return
            r[x][y] = dis+1
            q.append((x,y,dis+1))
                
            
        q = []
        for i in range(len(rooms)):
            for j in range(len(rooms[0])):
                if rooms[i][j] == 0: q.append((i,j,0))
        while q:
            cur = q.pop(0)
            visitNeighbor(rooms,q,cur[0]+1,cur[1],cur[2])
            visitNeighbor(rooms,q,cur[0]-1,cur[1],cur[2])
            visitNeighbor(rooms,q,cur[0],cur[1]+1,cur[2])
            visitNeighbor(rooms,q,cur[0],cur[1]-1,cur[2])
```

## 551. Nested List Weight Sum
```cpp
#include <queue>
class Solution {
public:
    void dfs(int&result,NestedInteger ni,int depth){
        if(ni.isInteger()){
            result += ni.getInteger() * depth;
        }
        else{
            auto l = ni.getList();
            for(auto i=0;i<l.size();i++){
                dfs(result,l[i],depth+1);
            }
        }
    }
    int depthSum(const vector<NestedInteger>& nestedList) {
       int result = 0;
       for(auto i=0;i<nestedList.size();i++){
           dfs(result,nestedList[i],1);
       }
       return result;
    }
};
```

```py
class Solution(object):
    def depthSum(self, nestedList):
        self.sum = 0
        def dfs(nestedL,depth):
            for ni in nestedL:
                if ni.isInteger():
                    self.sum += ni.getInteger()*depth
                else:
                    dfs(ni.getList(),depth+1)
        dfs(nestedList,1)
        return self.sum
```

## (Lint) 905. Nested List Weight Sum II
```py
class Solution:
    # when we meet [1,7,[7,[9]],[12],[],[]], the height of [12] is actually 2, not 1, another way is we dfs the height first, then calculate
    def depthSumInverse(self, nestedList):
        self.sum = 0
        def getDepth(nl):
            h = 1
            for ni in nl:
                if ni.isInteger(): pass
                else: h = max(h,1+getDepth(ni.getList()))
            return h
        def dfs(nls,h):
            cur = 0
            for ni in nls:
                if ni.isInteger():
                    cur += ni.getInteger()
                else:
                    dfs(ni.getList(),h-1)
            self.sum += cur * h
        h = getDepth(nestedList)
        dfs(nestedList,h)
        return self.sum
```

## 51. N-Queens
```py
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        self.resRaws = []
        def check(cur,target):
            if not cur: return True
            for i in range(len(cur)):
                # i row number, cur[i] col number len(cur):new row, target new col
                if target == cur[i] or  (len(cur)-i) == abs(target-cur[i]):
                    return False
            return True
        def dfs(n,cur=[]):
            print(cur,n)
            if len(cur) == n:
                self.resRaws += [cur.copy()]
            for i in range(n):
                if(check(cur,i)):
                    cur.append(i)
                    dfs(n,cur)
                    cur.pop()
                
        dfs(n)
        res = []
        for resRaw in self.resRaws:
            cur = []
            template = "."*n
            for colIdx in resRaw:
                cur.append(template[:colIdx]+"Q"+template[colIdx+1:])
            res+=[cur]
        return res
```

## 52. N-Queens II
```py
class Solution:
    def totalNQueens(self, n: int) -> int:
        self.resRaws = []
        def check(cur,target):
            if not cur: return True
            for i in range(len(cur)):
                # i row number, cur[i] col number len(cur):new row, target new col
                if target == cur[i] or  (len(cur)-i) == abs(target-cur[i]):
                    return False
            return True
        def dfs(n,cur=[]):
            if len(cur) == n:
                self.resRaws += [cur.copy()]
            for i in range(n):
                if(check(cur,i)):
                    cur.append(i)
                    dfs(n,cur)
                    cur.pop()
                
        dfs(n)
        return len(self.resRaws)
```

## 127. Word Ladder
```py
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        words = set(wordList)
        visited = set()
        q = [(beginWord,1)]
        lower = [chr(i) for i in range(ord("a"),ord("z")+1)]
        while q:
            curWord,dis = q.pop(0)
            # print(curWord,dis)
            visited.add(curWord)
            if curWord == endWord: return dis
            for i in range(len(curWord)):
                for j in lower:
                    nextWord = curWord[:i]+j+curWord[i+1:]
                    # print(nextWord)
                    if nextWord in words and nextWord not in visited:
                        q.append((nextWord,dis+1))
        return 0
```

## 126. Word Ladder II
#### Brute force
TLE
```py
class Solution(object):
    def findLadders(self, beginWord, endWord, wordList):
        words = set(wordList)
        visited = set()
        q = [(beginWord,[beginWord])]
        lower = [chr(i) for i in range(ord("a"),ord("z")+1)]
        res = []
        while q:
            sz = len(q)
            for i in range(sz):
                curWord,curPath = q.pop(0)
                visited.add(curWord)
                # print(curWord,curPath)
                if curWord == endWord:
                    res += [curPath]
                for j in range(len(curWord)):
                    for k in lower:
                        nextWord = curWord[:j]+k+curWord[j+1:]
                        if nextWord not in visited and nextWord in words:
                            q.append((nextWord,curPath + [nextWord]))
            if res: break
        return res
```


## 78. Subsets
```py
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        self.res = []
        def dfs(n,idx=0,cur=[]):
            self.res += [cur.copy()]
            if idx >= len(n): return
            for i in range(idx,len(n)):
                cur += [n[i]]
                dfs(n,i+1,cur)
                cur.pop()
        dfs(nums)
        return self.res
```

## 90 Subsets II
```py
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        self.res = []
        def dfs(n,idx=0,cur=[]):
            self.res += [cur.copy()]
            if idx >= len(n): return
            i = idx
            while i < len(n):
                cur += [n[i]]
                dfs(n,i+1,cur)
                cur.pop()
                i+=1
                while i < len(n) and n[i] == n[i-1]: i+=1
        nums.sort()
        dfs(nums)
        return self.res
```

## 77. Combinations 
```py
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        self.res = []
        def dfs(n,k,idx=1,cur=[]):
            if len(cur) == k:
                self.res += [cur.copy()]
                return
            for i in range(idx,n+1):
                cur.append(i)
                dfs(n,k,i+1,cur)
                cur.pop()
        dfs(n,k)
        return self.res
```

## 39. Combination Sum
```py
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(num,target,cur=[],curSum=0,idx=0):
            if curSum == target:
                self.res += [cur.copy()]
                return
            if curSum > target or idx >= len(num): return
            for i in range(idx,len(num)):
                nextL = cur.copy()
                for j in range(1,(target-curSum)//num[i]+1):
                    nextL.append(num[i])
                    dfs(num,target,nextL,curSum+j*num[i],i+1)
        self.res = []
        dfs(candidates,target)
        return self.res
```

## 40. Combination Sum II
```py
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(num,target,cur=[],curSum=0,idx=0):
            if curSum == target:
                self.res += [cur.copy()]
                return
            if curSum > target or idx >= len(num): return
            i = idx
            while i < len(num):
                cur.append(num[i])
                dfs(num,target,cur,curSum+num[i],i+1)
                cur.pop()
                i+=1
                while i<len(num) and num[i] == num[i-1]: i+=1
                
        candidates.sort()
        self.res = []
        dfs(candidates,target)
        return self.res
```

## 216. Combination Sum III
```py
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        def dfs(k,n,cur=[],idx=1,curSum=0):
            if curSum == n and len(cur) == k:
                self.res += [cur.copy()]
                return
            if idx >= 10 or curSum > n or len(cur) >= k: return
            for i in range(idx,10):
                cur.append(i)
                dfs(k,n,cur,i+1,curSum+i)
                cur.pop()
        self.res=[]
        dfs(k,n)
        return self.res
```

## 1308. Factor Combinations
```py
class Solution:
    def getFactors(self, n):
        self.res = []
        def dfs(curN,start=2,curL=[]):
            if curN == 1:
                if len(curL) > 1:
                    self.res += [curL.copy()]
                return
            for i in range(start,curN+1):
                if curN%i==0:
                    curL.append(i)
                    dfs(curN//i,i,curL)
                    curL.pop()
        dfs(n)
        return self.res
```

#### Using prime numbers
```cpp
class Solution {
public:
    vector<vector<int>> getFactors(int n) {
        vector<vector<int>> res;
        helper(n, 2, {}, res);
        return res;
    }
    void helper(int n, int start, vector<int> out, vector<vector<int>> &res) {
        for (int i = start; i <= sqrt(n); ++i) {
            if (n % i == 0) {
                vector<int> new_out = out;
                new_out.push_back(i);
                helper(n / i, i, new_out, res);
                new_out.push_back(n / i);
                res.push_back(new_out);
            }
        }
    }
};
```

```py
import math
class Solution:
    def getFactors(self, n):
        self.res = []
        def dfs(curN,start=2,curL=[]):
            if curN == 1: return
            for i in range(start,min(math.floor(math.sqrt(curN))+1,curN)):
                if curN % i == 0:
                    pendingL = curL.copy()
                    pendingL.append(i)
                    nextL = pendingL.copy()
                    dfs(curN//i,i,nextL)
                    pendingL.append(curN//i)
                    self.res += [pendingL.copy()]
        dfs(n)
        return self.res
```

## 46. Permutations
```py
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        self.res = []
        def dfs(nums,cur=[],visited=set()):
            if len(cur) == len(nums):
                self.res += [cur.copy()]
                return
            for num in nums:
                if num not in visited:
                    visited.add(num)
                    cur.append(num)
                    dfs(nums,cur,visited)
                    cur.pop()
                    visited.remove(num)
        dfs(nums)
        return self.res
```

## 47. Permutations II
```py
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        self.res = []
        d = {}
        visited = {}
        for num in nums:
            if num not in d: d[num]=1
            else: d[num]+=1
        for n in d.keys():
            visited[n] = 0
        def dfs(d,visited,cur=[]):
            if len(cur) == len(nums):
                self.res += [cur.copy()]
                return
            for n in d.keys():
                if visited[n] < d[n]:
                    visited[n]+=1
                    cur.append(n)
                    dfs(d,visited,cur)
                    cur.pop()
                    visited[n]-=1
        dfs(d,visited)
        return self.res
```

## 31. Next Permutation
```py
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        # 1. get the first numebr to swap from back to front where nums[i] < nums[i-1], 
        # if i == 0 and nums[0] < nums[1]: reverse the number
        # 2. from rear to i(it is increasing), find the first k when nums[i] < nums[k] (use binary search)
        # 3. swap nums[i] and nums[k], then make nums from k to rear increasing, just revers nums[k+1:]
        if len(nums) <= 1: return
        i = len(nums)-2
        while i >= 0 and nums[i] >= nums[i+1]: i-=1
        if i < 0:
            nums.reverse()
            return 
        right = len(nums)-1
        for j in range(len(nums)-1,i,-1):
            if nums[j]>nums[i]: 
                right = j
                break
        nums[i],nums[right] = nums[right],nums[i]
        # print(i,right)
        # print(nums)
        nums[i+1:] = nums[i+1:][::-1]
        # print(nums)
```


## 60. Permutation Sequence
From (https://leetcode.com/problems/permutation-sequence/discuss/22507/%22Explain-like-I'm-five%22-Java-Solution-in-O(n))

I'm sure somewhere can be simplified so it'd be nice if anyone can let me know. The pattern was that:

say n = 4, you have {1, 2, 3, 4}

If you were to list out all the permutations you have

1 + (permutations of 2, 3, 4)

2 + (permutations of 1, 3, 4)

3 + (permutations of 1, 2, 4)

4 + (permutations of 1, 2, 3)


We know how to calculate the number of permutations of n numbers... n! So each of those with permutations of 3 numbers means there are 6 possible permutations. Meaning there would be a total of 24 permutations in this particular one. So if you were to look for the (k = 14) 14th permutation, it would be in the

3 + (permutations of 1, 2, 4) subset.

To programmatically get that, you take k = 13 (subtract 1 because of things always starting at 0) and divide that by the 6 we got from the factorial, which would give you the index of the number you want. In the array {1, 2, 3, 4}, k/(n-1)! = 13/(4-1)! = 13/3! = 13/6 = 2. The array {1, 2, 3, 4} has a value of 3 at index 2. So the first number is a 3.

Then the problem repeats with less numbers.

The permutations of {1, 2, 4} would be:

1 + (permutations of 2, 4)

2 + (permutations of 1, 4)

4 + (permutations of 1, 2)

But our k is no longer the 14th, because in the previous step, we've already eliminated the 12 4-number permutations starting with 1 and 2. So you subtract 12 from k.. which gives you 1. Programmatically that would be...

k = k - (index from previous) * (n-1)! = k - 2*(n-1)! = 13 - 2*(3)! = 1

In this second step, permutations of 2 numbers has only 2 possibilities, meaning each of the three permutations listed above a has two possibilities, giving a total of 6. We're looking for the first one, so that would be in the 1 + (permutations of 2, 4) subset.

Meaning: index to get number from is k / (n - 2)! = 1 / (4-2)! = 1 / 2! = 0.. from {1, 2, 4}, index 0 is 1


so the numbers we have so far is 3, 1... and then repeating without explanations.


{2, 4}

k = k - (index from pervious) * (n-2)! = k - 0 * (n - 2)! = 1 - 0 = 1;

third number's index = k / (n - 3)! = 1 / (4-3)! = 1/ 1! = 1... from {2, 4}, index 1 has 4

Third number is 4


{2}

k = k - (index from pervious) * (n - 3)! = k - 1 * (4 - 3)! = 1 - 1 = 0;

third number's index = k / (n - 4)! = 0 / (4-4)! = 0/ 1 = 0... from {2}, index 0 has 2

Fourth number is 2


Giving us 3142. If you manually list out the permutations using DFS method, it would be 3142. Done! It really was all about pattern finding.

```py
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        nums = [i+1 for i in range(n)]
        facs = [1]*(n+1)
        for i in range(1,n+1): facs[i] = facs[i-1]*(i)
        ret = ""
        k-=1
        for i in range(n):
            index = k//facs[n-1-i]
            ret = ret + str(nums[index])
            nums.pop(index)
            k = k%facs[n-1-i]
        return ret
```


## 829. Word Pattern II
```py
```py
class Solution:
    """
    @param pattern: a string,denote pattern string
    @param str: a string, denote matching string
    @return: a boolean
    """
    def wordPatternMatch(self, p, s):
        def dfs(p, s, p_s, s_p):
            if len(p) == 0:
                return len(s) == 0
            for l in range(len(s)):
                s_to_match = s[0:l + 1]
                p_to_match = p[0]
                if p_to_match not in p_s and s_to_match not in s_p:
                    p_s[p_to_match], s_p[s_to_match] = s_to_match, p_to_match
                    if dfs(p[1:], s[l+1:], p_s, s_p):
                        return True
                    del p_s[p_to_match]
                    del s_p[s_to_match]
                elif p_s.get(p_to_match, None) == s_to_match and s_p.get(s_to_match, None) == p_to_match and dfs(p[1:], s[l+1:], p_s, s_p):
                    return True
                else:
                    continue
            return False
        return dfs(p, s, {}, {})
```

## 779. Generalized Abbreviation
If there is only one abbreviation in the word, the code below works. But there might be cases light "w1o1" for word 
```py
class Solution:
    """
    @param word: the given word
    @return: the generalized abbreviations of a word
    """
    def generateAbbreviations(self, word):
        res = []
        res.append(word)
        # i : length
        for i in range(1,len(word)+1):
            # j : start pos
            for j in range(len(word)-i+1):
                target = word[:j] + str(i) + word[j+i:]
                print(target)
                res.append(target)
        return res
```

```py
class Solution:
    def generateAbbreviations(self, word):
        # for each pos, abbreviate it or not
        self.res = []
        def helper(word,curPath="",idx=0,cnt=0):
            if idx == len(word):
                if cnt == 0: self.res += [curPath]
                else: self.res += [curPath+str(cnt)]
                return
            # abbreviate current word, add cnt
            helper(word,curPath,idx+1,cnt+1)
            # not abbreviate, needs to add the cnt accumulated by previous words
            pre = "" if cnt == 0 else str(cnt)
            helper(word,curPath + pre + word[idx],idx+1,0)
            
    
        helper(word)
        return self.res
```

## 351	Android Unlock Patterns
https://www.cnblogs.com/grandyang/p/5541012.html

注意1可以直接到6，8，可以间接到3，7，9
```py
class Solution:
    # 9 positions, 1,3,7,9 are symmetric, 2,4,6,8 are symmetric
    # all keys must be distinct
    def numberOfPatterns(self, m, n):
        jump = [[0 for i in range(10)] for j in range(10)]
        jump[2][8]=jump[8][2] = jump[4][6] = jump[6][4] = 5
        jump[1][9]=jump[9][1] = jump[3][7] = jump[7][3] = 5
        jump[1][7] = jump[7][1] = 4
        jump[1][3] = jump[3][1] = 2
        jump[7][9] = jump[9][7] = 8
        jump[3][9] = jump[9][3] = 6
        res = 0
        self.tmp = 0
        def helper(pos,length,m,n,visited=set(),first = True):
            if first:
                self.tmp = 0
                first = False
            if length > n: return
            if length >=m: 
                self.tmp += 1
            visited.add(pos)
            for i in range(1,10):
                if i not in visited and (jump[pos][i] == 0 or jump[pos][i] in visited):
                    helper(i,length+1,m,n,visited,first)
            visited.remove(pos)
        helper(1,1,m,n)
        res += 4*self.tmp
        helper(2,1,m,n)
        res += 4*self.tmp
        helper(5,1,m,n)
        res += self.tmp
        return res
```

## 93. Restore IP Addresses
注意0和255的情况
```py
class Solution:
    # 从头开始，取1位，取2位，取3位，不断累积
    def restoreIpAddresses(self, s: str) -> List[str]:
        self.res = []
        def helper(s,pos=0,cur="",finished=0):
            if finished == 4 and pos == len(s):
                self.res.append(cur)
            if pos >= len(s) or finished >= 4 or len(s)-pos > (4-finished)*3: return
            for i in range(pos+1,min(pos+4,len(s)+1)):
                curNum = s[pos:i]
                if int(curNum)>255 or (len(curNum) > 1 and curNum[0] == "0"): continue
                else:
                    if not cur:
                        helper(s,i,cur+curNum,finished+1)
                    else:
                        helper(s,i,cur+"."+curNum,finished+1)
        helper(s)
        return self.res
```


## 208	Implement Trie (Prefix Tree)
```py
class TrieNode:
    def __init__(self,char,word_end = False):
        self.char = char
        self.children = []
        self.word_end = word_end
    
    def add(self,s):
        if not s: 
            self.word_end = True
            return
        exist = False
        for child in self.children:
            if child.char == s[0]:
                exist = True
                child.add(s[1:])
                break
        if not exist:
            self.children.append(TrieNode(s[0]))
            self.children[-1].add(s[1:])
        
    def search(self,s,prefix = False):
        if not s:
            if prefix: return True
            return self.word_end
        for child in self.children:
            if child.char == s[0]:
                return child.search(s[1:],prefix)
        return False
        
class Trie:

    def __init__(self):
        self.root = TrieNode("")

    def insert(self, word: str) -> None:
        self.root.add(word)
        

    def search(self, word: str) -> bool:
        return self.root.search(word)

    def startsWith(self, prefix: str) -> bool:
            return self.root.search(prefix,True)
        
```

## 211	Add and Search Word - Data structure design
One line change
```py
class TrieNode:
    def __init__(self,char,word_end = False):
        self.char = char
        self.children = []
        self.word_end = word_end
    
    def add(self,s):
        if not s: 
            self.word_end = True
            return
        exist = False
        for child in self.children:
            if child.char == s[0]:
                exist = True
                child.add(s[1:])
                break
        if not exist:
            self.children.append(TrieNode(s[0]))
            self.children[-1].add(s[1:])
        
    def search(self,s):
        if not s:
            return self.word_end
        for child in self.children:
            if child.char == s[0]:
                return child.search(s[1:])
            elif s[0] == "." and child.search(s[1:]):
                return True
        return False
    
class WordDictionary:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode("")

    def addWord(self, word: str) -> None:
        """
        Adds a word into the data structure.
        """
        self.root.add(word)

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        """
        return self.root.search(word)
        

```

## 212	Word Search II
Tricky trie tree
```py
```


## 282	Expression Add Operators    
looks Too hard

## 377. Combination Sum IV
DP


