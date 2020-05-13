---
layout: post
title:  DP Pattern
categories: Interview
---
https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns

# Patterns
Minimum (Maximum) Path to Reach a Target
Distinct Ways
Merging Intervals
DP on Strings
Decision Making

# Minimum (Maximum) Path to Reach a Target
Generate problem statement for this pattern

Statement
Given a target find minimum (maximum) cost / path / sum to reach the target.

Approach
Choose minimum (maximum) path among all possible paths before the current state, then add value for the current state.

`routes[i] = min(routes[i-1], routes[i-2], ... , routes[i-k]) + cost[i]`

可以用单调队列优化
```
for (int i = 1; i <= target; ++i) {
   for (int j = 0; j < ways.size(); ++j) {
       if (ways[j] <= i) {
           dp[i] = min(dp[i], dp[i - ways[j]] + cost / path / sum) ;
       }
   }
}
return dp[target]
```

## 746. Min Cost Climbing Stairs
```py
class Solution(object):
    def minCostClimbingStairs(self, cost):
        """
        :type cost: List[int]
        :rtype: int
        """
        # dp[i] means the min cost to reach i-th stair
        # Since each cost is non-negative, the min cost the get to second 
        # starit is cost[1]
        cost.append(0)
        n = len(cost)
        dp = [0 for i in range(n+1)]
        dp[1] = cost[0]
        dp[2] = cost[1]
        for i in range(3,n+1):
            dp[i] = cost[i-1] + min(dp[i-1],dp[i-2])
        return dp[n]
```

## 64. Minimum Path Sum
#### O(N) Space
```py
class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m,n = len(grid),len(grid[0])
        dp = [0 for i in range(n+1)]
        for j in range(1,n+1):
            dp[j] = grid[0][j-1]+dp[j-1]
        dp[0] = 99999999
        for i in range(1,m):
            for j in range(n):
                dp[j+1] = min(dp[j],dp[j+1]) + grid[i][j]
        return dp[n]
```

## 70. Climbing Stairs
O(1) space
```py
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        a,b = 1,2
        c = 0
        if n == 1:
            return a
        if n == 2:
            return b
        for i in range(3,n+1):
            c = a+b 
            a = b
            b = c
        return c
```

## 322. Coin Change 换零钱
用最少的硬币换一定数量的钱。可以把总钱数看成要爬的台阶总数，硬币的面值是每一步能爬的个数
```py
class Solution(object):
    def coinChange(self, coins, amount):
        dp = [amount+500 for i in range(amount+1)]
        dp[0] = 0
        n = len(coins)
        for i in range(1,amount+1):
            for j in range(n):
                if i-coins[j] >= 0:
                    dp[i] = min(dp[i],dp[i-coins[j]]+1)
        return dp[amount]  if dp[amount] != amount+500 else -1
```

## 518. Coin Change 2
Number of ways for coin change

这里和number of ways to climb stairs 不同，因为不能重复
比如：
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

所以要想办法避免重复

为什么iterate throught columns

Well, the real reason why the answer changes because of loops is because of the change in dp definition when you try to reduce the space complexity.If we define dp[i][j] as "number of ways to get sum 'j' using 'first i' coins", then the answer doesn't change because of loop arrangement.

So why does the answer change only when we try to reduce the space complexity?

To get the correct answer, the correct dp definition should be dp[i][j]="number of ways to get sum 'j' using 'first i' coins". Now when we try to traverse the 2-d array row-wise by keeping only previous row array(to reduce space complexity), we preserve the above dp definition as dp[j]="number of ways to get sum 'j' using 'previous /first i coins' " but when we try to traverse the 2-d array column-wise by keeping only the previous column, the meaning of dp array changes to dp[j]="number of ways to get sum 'j' using 'all' coins".

```py
   class Solution {

   public int change(int amount, int[] coins) {       
             if(coins.length==0) 
                 {
                     if(amount==0) return 1;
                      return 0;
                }
    
    int dp[][]=new int[coins.length][amount+1];
    
    //Initializing first column
    for(int i=0;i<coins.length;i++)
        dp[i][0]=1;
    
    //Initializing first row.
    for(int i=1;i<=amount;i++)
    {
        if(i%coins[0]==0)
            dp[0][i]=1;
        else
            dp[0][i]=0;
    }
   
  //Check the difference between interchanging the for loops.
  //Both of them work. You can comment it out and check.  
    
    
  ////////1st one
  for(int i=1;i<coins.length;i++)
  {
    for(int j=1;j<=amount;j++)
    {
      if(j>=coins[i])
        dp[i][j]=dp[i-1][j]+dp[i][j-coins[i]];
      else
         dp[i][j]=dp[i-1][j];
    }
  }
    
    
  ///////2nd one
  for(int j=1;j<=amount;j++)
  {
    for(int i=1;i<coins.length;i++)
    {
      if(j>=coins[i])
        dp[i][j]=dp[i-1][j]+dp[i][j-coins[i]];
      else
        dp[i][j]=dp[i-1][j];
    }
  }
    
    return dp[coins.length-1][amount];
   }
  }
 }
 ```

In the below code you can see that if we are not interested in reducing the space complexity, both the loop arrangements yield the same answer.

Code:-
```py
class Solution(object):
    def change(self, amount, coins):
        dp = [0 for i in range(amount+1)]
        n = len(coins)
        for i in range(n):
            if coins[i] <= amount:
               dp[coins[i]] += 1
            for j in range(1,amount-coins[i]+1):
               dp[j+coins[i]] += dp[j]   
        return dp[amount]
```

## 931. Minimum Falling Path Sum
```
class Solution(object):
    def minFallingPathSum(self, A):
        """
        :type A: List[List[int]]
        :rtype: int
        """
        inf = 9999999999
        m,n = len(A),len(A[0])
        dp = [[inf for _ in range(n+2)] for _ in range(m+1)]
        for i in range(1,n+1):
            dp[0][i] = 0
        for i in range(1,m+1):
            for j in range(1,n+1):
                fpath = min(dp[i-1][j],dp[i-1][j-1],dp[i-1][j+1])
                dp[i][j] = fpath + A[i-1][j-1]
        return min(dp[m])
```

## 983. Minimum Cost For Tickets
和coin change思路差不多，但比较不好处理的是只是有些天出游，不管这些slots，当作连续的吗？不可以，因为中间这些天并没有出游。 所以应该只遍历到有出游的天数，如果买周票或者月票，找上一个最近的出游天数。上一个最近可以通过一个hashmap实现，这样只需要遍历一遍就可以了。

通过寻找上一次出游的天数，变成了一样的问题。
```py
class Solution(object):
    def mincostTickets(self, days, costs):
        """
        :type days: List[int]
        :type costs: List[int]
        :rtype: int
        """
        # hashmap, find the last days we visited from this day
        num_days = days[-1]
        prev = [-1 for i in range(num_days+1)]
        i = 0
        days_idx = 0
        while days_idx < len(days)-1:
            next_day = num_days+1 if days_idx == len(days)-1 else days[days_idx+1]
            if i <= days[days_idx]:
                i+= 1
            elif i <= next_day:
                prev[i] = days[days_idx]
                i+=1
            else:
                days_idx += 1
        

        dp = {}
        for i in range(len(days)):
            # insert i to dp
            dp[days[i]] = 0
            # 1 day, 7 day or 30days
            dp[days[i]] = min(
                costs[0] + (0 if prev[days[i]] == -1 else dp[prev[days[i]]]),
                costs[1] + (0 if (days[i]-7<0 or (prev[days[i]-6] == -1)) else  dp[prev[days[i]-6]]),
                costs[2] + (0 if (days[i]-30<0 or (prev[days[i]-29] == -1)) else  dp[prev[days[i]-29]]),
            )
        return dp[days[-1]]
```

#### 更简单的办法
直接跳过不出游的天数，当这天不出游的话`dp[i] = dp[i-1]`
```py
class Solution(object):
    def mincostTickets(self, days, costs):
        """
        :type days: List[int]
        :type costs: List[int]
        :rtype: int
        """
        num_days = days[-1]
        # dp[i]: the min cost for travel in 0..i days
        dp = [0 for i in range(num_days+1)]
        visit_days = set(days)
        for i in range(1,num_days+1):
            if i not in visit_days:
                    dp[i] = dp[i-1]
            else:
                dp[i] = min(costs[0]+dp[i-1], costs[1] + dp[max(0,i-7)], costs[2]+dp[max(0,i-30)])
        return dp[num_days]
```

## 650. 2 Keys Keyboard

主要难点：如何转化这个cost和paste的操作:
可以堪称coins change,每次copy相当于使用这个coin，但这个coin只能使用一次
```py
class Solution(object):
    def minSteps(self, n):
        # dp[i]: the min numbers to reach i
        # 从前往后？可以，相当于有1到i个硬币,所以dp[i+n*i] = dp[i]+n
        dp = [10*n for i in range(n+1)]
        dp[1] = 0
        for i in range(1,n+1):
            k = 1
            while i+k*i<=n:
                dp[i+k*i] = min(dp[i+k*i],dp[i]+k+1)
                k+=1          
        return dp[n]        
```

## 279. Perfect Squares

```py
class Solution(object):
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        #不能贪心，因为12=9+1+1+1 = 4+4+4
        #只能dp
        perf = []
        k = 1
        while k*k<=n:
            perf.append(k*k)
            k+=1
        dp = [n for i in range(n+1)]
        for i in range(len(perf)):
            dp[perf[i]] = 1
            for j in range(1,n+1):
                if j + perf[i] <= n:
                    dp[j+perf[i]] = min(dp[j+perf[i]],dp[j]+1)
        return dp[n]
```

## 1049. Last Stone Weight II

#### Explaining why this problem is equals to finding the difference between the sum of two groups


NOTE: I previously explained this in another post. But I'm posting it now as a standalone post to help others finding this answer easily. Link to the post where I answered this in the form of reply

Suppose you have rock a, b, c and d.
If you subtract them in the following order: b-c, then d-b-c. Then it is the same as doing d-(b+c).
Then doing [d-(b+c)]-a is the same as -a+d-(b+c), which is d-a-(b+c), which is d-[a+(b+c)], which is d-(a+b+c). (So doing things in that order will lead to this shortcut).

Lets try another order.
Suppose you have rock a, b, c and d.
If you do a-d, then b-c, then (a-d)-(b-c).
Then (a-d)-(b-c) is the same as a-d-b+c, which is the same as -d-b+a+c, which is -(d+b)+(a+c), which is (a+c)-(d+b). Another shortcut.

Then you can see that depending on the order of the subtractions, we get a different setting of difference between two groups.

#### Solution
Very classic knapsack problem solved by DP.
In this solution, I use dp to record the achievable sum of the smaller group.
dp[x] = 1 means the sum x is possible.

这里很tricky

第一点：注意当total是奇数的情况，因为我们找到的是smaller sum，所以total-2*smaller
第二点：dp的j的range要注意是从后往前for j in range(min(total,1501),stones[i]-1,-1)，原因是因为每个石头只能用一次，背包问题中如果从前往后的话每个石头可以用多次。

```py
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        dp = [False for _ in range(1501)]
        dp[0] = True
        n = len(stones)
        total = sum(stones)
        for i in range(n):
            for j in range(min(total,1501),stones[i]-1,-1):
                dp[j] = dp[j] | dp[j-stones[i]]
        for i in range(total//2,-1,-1):
            if dp[i]:
                return total-i-i
```

## 474. Ones and Zeroes
背包问题的变种，每个物品需要两种资源
#### O(N^3) 空间
注意这里的j，k必须从前往后，不能从后往前，因为dp[i][j][k]还没有值,
只有dp[i-1][j][k]有值

```py
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        arr = []
        for s in strs:
            a,b = 0,0
            for c in s:
                if c == "0": a+=1
                else: b+=1
            arr.append((a,b))
        l = len(arr)
        dp = [[[0 for _ in range(n+1)] for _ in range(m+1)] for _ in range(l+1)]
        for i in range(1,l+1):
            a,b = arr[i-1][0],arr[i-1][1]
            # 注意j和k要从零开始，因为可能没有1或者0
            for j in range(m+1):
                for k in range(n+1):
                    if j < a or k < b:
                        dp[i][j][k] = dp[i-1][j][k]
                    else:
                        dp[i][j][k] = max(dp[i-1][j-a][k-b]+1,dp[i-1][j][k])
        print(dp)
        return dp[l][m][n]
```

#### O(N^2) 空间
注意这里必须从后往前，因为每个物品只能用一次，为了节省空间我们重复利用了之前的数组。
```py
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        k = len(strs)
        arr = []
        for s in strs:
            a,b = 0,0
            for c in s:
                if c == "0": a+=1
                else: b+=1
            arr.append((a,b))
        dp = [[0 for _ in range(n+1)] for _ in range(m+1)]
        dp[0][0] = 0
        for i in range(len(arr)):
            a,b = arr[i][0],arr[i][1]
            if a <= m and b <= n:
                dp[a][b] = 1
                for j in range(m,a-1,-1):
                    for k in range(n,b-1,-1):
                        dp[j][k] = max(dp[j-a][k-b]+1,dp[j][k])
        return dp[m][n]
```

## 174. Dungeon Game


## 871. Minimum Number of Refueling Stops
https://leetcode.com/problems/minimum-number-of-refueling-stops/
#### DP
Approach 1: 1D DP, O(N^2)
dp[t] means the furthest distance that we can get with t times of refueling.

So for every station s[i],
if the current distance dp[t] >= s[i][0], we can refuel:
dp[t + 1] = max(dp[t + 1], dp[t] + s[i][1])

In the end, we'll return the first t with dp[i] >= target,
otherwise we'll return -1.

每次使用一个station，使用以后判断能不能根据之前的来更新。

为什么从i-1 到 0？ 比如1被update，然后再更新2，会利用1update以后的结果，这样dp[i]会比实际的大。
```py
class Solution:
    def minRefuelStops(self, target: int, startFuel: int, stations: List[List[int]]) -> int:
        # dp[i]: furthest dis we can get with i fueling
        # if dp[i] > stations[i][0], we can fuel there
        # then dp[i+1] = max(dp[i+1],dp[i]+stations[i][1])
        # dp[0] = startFuel
        if startFuel >= target: return 0
        n = len(stations)
        dp = [startFuel for _ in range(n+1)]
        for i in range(1,n+1):
            # update using smaller than i number of stations
            for j in range(i-1,-1,-1):
                if dp[j]<stations[i-1][0]:
                    break
                dp[j+1] = max(dp[j+1],dp[j]+stations[i-1][1])
        for i in range(n+1):
            if dp[i] >= target:
                return i
        return -1
```

#### Priority Queue
i - is the index of next stops to refuel.
res - is the times that we have refeuled.
pq - is a priority queue that we store all available gas.

We initial res = 0 and in every loop:

We add all reachable stop to priority queue.
We pop out the largest gas from pq and refeul once.
If we can't refuel, means that we can not go forward and return -1

```py
class Solution {
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        int i=0;
        int cur = startFuel;
        int counter = 0;
        // greedy, using pq to add the larest fuel each time
        // 用负号用最小堆来代替最大堆
        while(cur < target){
            // 每次都使用一个加油站，然后加能到达的加油站内最多的油
            while(i < stations.length && cur >= stations[i][0]){
                pq.offer(-stations[i++][1]);
            }
            if(pq.isEmpty()){
               return -1; 
            }
            cur += -pq.poll();
            counter += 1;
        }
        return counter;
    }
}
```

## 174. Dungeon Game
```py
class Solution:
    def calculateMinimumHP(self, dungeon: List[List[int]]) -> int:
        # dp[i][j] represents the min hp needed at position (i, j)
        # Add dummy row and column at bottom and right side        
        # if need < 0: 这个格子补血了，或者之后的格子补血了
        # hp[i][j] = min(hp[i+1][j],hp[i][j+1]) - dungeon[i][j]
        # 为什么是min(hp[i+1][j],hp[i][j+1]),因为要找最小的need，所以找最小的hp
        # dp[i][j] = need <= 0? 0 : need
        m,n = len(dungeon),len(dungeon[0])
        dp = [[99999999 for _ in range(n+1)] for _ in range(m+1)]
        dp[m][n-1] = dp[m-1][n] = 1
        for i in range(m-1,-1,-1):
            for j in range(n-1,-1,-1):
                need = min (dp[i+1][j],dp[i][j+1]) - dungeon[i][j]
                dp[i][j] = need if need > 0 else 1
        return dp[0][0]

```