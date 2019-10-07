---
layout: post
title:  "电面"
categories: Interview
---
# Coupang
## Question:Lexicographically smallest array after at-most K consecutive swaps
https://www.geeksforgeeks.org/lexicographically-smallest-array-k-consecutive-swaps/
#### DFS
需要注意的是 翻转n次的最优解和n+1次的最优解没关系，不能用dp
失败：比较大小的isLarger写错了
```
#include <iostream>
#include <vector>
using namespace std;
bool isLarger(vector<int>&a,vector<int>&b){
    for(auto i=0;i<a.size();i++){
        if(a[i] > b[i]) return true;
        else if(a[i] < b[i]) return false;
    }
    return true;
}
void DFS(vector<int>&result,vector<int>&input,int k){
    if(isLarger(result,input)) result.assign(input.begin(),input.end());
    if(k==0) return;
    for(int i=0;i<input.size()-1;i++){
        swap(input[i],input[i+1]);
        DFS(result,input,k-1);
        swap(input[i],input[i+1]);
    }
}

int main()
{
    for(int n=0;n<=12;n++){
        vector<int> input{4,3,2,1,5};
        vector<int> result = input;
        DFS(result,input,n);   
        for(auto x:result){
            cout<<x<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```

#### Optimum
O(n^2),因为每次把最高位变的尽可能小就得到最小
```
#include <iostream>
#include <vector>
using namespace std;
void DFS(vector<int>&result,int k){
    for(int i=0;i<result.size()-1 and k > 0;i++){
        int pos = i;
        for(int j=i+1;j<result.size();j++){
            if(j-i>k) break;
            if(result[j] < result[pos]) pos = j;
        }
        for(int j=pos;j>i;j--){
            swap(result[j-1],result[j]);
        }
        k -= (pos-i);
    }
}

int main()
{
    for(int n=0;n<=15;n++){
        vector<int> input{4,3,2,1,5};
        DFS(input,n);
        for(auto x:input){
            cout<<x<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```

## Cadence
聊C++，匹兹堡，Citrix实习，过

四轮Onsite 过

## Yahoo Verizon Media
Yeah, I used `yahoo.com` a lot when I was in elementry school. And I think Yahoo is really a great company for providing people with valuuable contents.

1. Virtual Function/non-virtual function
When we use the pointer or reference of the base class, we will be able to run different versions of the funtion.

Why virtual destructor:
Deleting a derived class object using a pointer to a base class that has a non-virtual destructor results in undefined behavior. To correct this situation, the base class should be defined with a virtual destructor. For example, following program results in undefined behavior.

2. SQL Join
A join is an SQL operation performed to establish a connection between two or more database tables based on matching columns, thereby creating a relationship between the tables. Most complex queries in an SQL database management system involve join commands. There are different types of joins.

inner join:[website](https://www.w3schools.com/sql/sql_join_inner.asp)
The INNER JOIN keyword selects records that have matching values in both tables.

LeftJoin: left outter join
[website](https://www.w3schools.com/sql/sql_join_left.asp)
The LEFT JOIN keyword returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side, if there is no match.

SQL RIGHT JOIN Keyword
The RIGHT JOIN keyword returns all records from the right table (table2), and the matched records from the left table (table1). The result is NULL from the left side, when there is no match.


SQL FULL OUTER JOIN Keyword
The FULL OUTER JOIN keyword return all records when there is a match in left (table1) or right (table2) table records.

Note: FULL OUTER JOIN can potentially return very large result-sets!

Tip: FULL OUTER JOIN and FULL JOIN are the same.

3. External Sort
when the data is too large to be fit into the memory

10份数据，先每一份都排好放到10个地方，然后执行完以后执行10路归并排序，然后把结果放到缓冲区，当缓冲区满输出到persistent layer.

* 并行计算
用多个磁盘驱动器并行处理数据，可以加速顺序磁盘读写。[4]
在计算机上使用多线程，可在多核心的计算机上得到优化。
使用异步输入输出，可以同时排序和归并，同时读写。
使用多台计算机用高速网络连接，分担计算任务。[5]
* 提高硬件速度
增大内存，减小磁盘读写次数，减小归并次数。
使用快速的外存设备，比如15000 RPM的硬盘或固态硬盘。
使用性能更优良个各种设备，比如使用多核心CPU和延迟时间更短的内存。
* 提高软件速度
对于某些特殊数据，在第一阶段的排序中使用基数排序。
压缩输入输出文件和临时文件。

4. 线程进程区别
A computer process is composed of 
Level1: Register,Counter, Stack
Level2: Heap
Level3: Code

All the threads in a process may share same code an heap, but they have their own set of stack and reigster


Processes vs. Threads — Advantages and Disadvantages

Processes are heavyweight operations	Threads are lighter weight operations
Each process has its own memory space	Threads use the memory of the process they belong to
Inter-process communication is slow as processes have different memory addresses	Inter-thread communication can be faster than inter-process communication because threads of the same process share memory with the process they belong to
Context switching between processes is more expensive	Context switching between threads of the same process is less expensive
Processes don’t share memory with other processes	Threads share memory with other threads of the same process

https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/


http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html