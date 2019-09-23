---
layout: post
title:  "Leetcode Linked List"
categories: Interview
---
## 138. Copy List with Random Pointer
```
class Solution {
public:
    Node* copyRandomList(Node* head) {
        unordered_map<Node*,Node*> uMap;
        Node *newHead=NULL,*pre = NULL,*tmpHead=head,*newTmp;
        while(tmpHead){
            newTmp = new Node(tmpHead->val,NULL,NULL);
            uMap[tmpHead] = newTmp;
            if(!newHead) newHead = newTmp;
            if(pre) pre -> next = newTmp;
            pre = newTmp;
            tmpHead = tmpHead->next;
        }
        tmpHead = head;
        while(tmpHead){
            if(tmpHead->random == NULL){}
            else{
                uMap[tmpHead]->random = uMap[tmpHead->random];
            }
            tmpHead = tmpHead->next;
        }
        return newHead;
    }
};
```


```
当然，如果使用 HashMap 占用额外的空间，如果这道题限制了空间的话，就要考虑别的方法。下面这个方法很巧妙，可以分为以下三个步骤：

1. 在原链表的每个节点后面拷贝出一个新的节点。

2. 依次给新的节点的随机指针赋值，而且这个赋值非常容易 cur->next->random = cur->random->next。

3. 断开链表可得到深度拷贝后的新链表。

举个例子来说吧，比如原链表是 1(2) -> 2(3) -> 3(1)，括号中是其 random 指针指向的结点，那么这个解法是首先比遍历一遍原链表，在每个结点后拷贝一个同样的结点，但是拷贝结点的 random 指针仍为空，则原链表变为 1(2) -> 1(null) -> 2(3) -> 2(null) -> 3(1) -> 3(null)。然后第二次遍历，是将拷贝结点的 random 指针赋上正确的值，则原链表变为 1(2) -> 1(2) -> 2(3) -> 2(3) -> 3(1) -> 3(1)，注意赋值语句为：

cur->next->random = cur->random->next;

这里的 cur 是原链表中结点，cur->next 则为拷贝链表的结点，cur->next->random 则为拷贝链表的 random 指针。cur->random 为原链表结点的 random 指针指向的结点，因为其指向的还是原链表的结点，所以我们要再加个 next，才能指向拷贝链表的结点。最后再遍历一次，就是要把原链表和拷贝链表断开即可，参见代码如下：

 
```

#### Soltution without extra space
```

class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head) return NULL;
        Node *tmp = head;
        while (tmp) {
            Node *t = new Node(tmp->val, nullptr, nullptr);
            t->next = tmp->next;
            tmp->next = t;
            tmp = t->next;
        }
        tmp = head;
        while(tmp){
            if(tmp->random) tmp->next->random = tmp -> random->next ;
            tmp = tmp->next->next;
        }
        tmp = head;
        Node *result = head->next;
        while(tmp){
            Node* t = tmp->next;
            tmp->next = t->next;
            if(t->next) t->next = t->next->next;
            tmp = tmp->next;                
        }
        return result;
    }
};
```
<hr>

## Fast slow pointer
[Great blog for this problem](https://blog.csdn.net/suibianshen2012/article/details/52032138
)
## 141 Find the ring of linked list

## 142 Find the start position of ring
 ```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(!head or !head->next) return NULL;
    
        ListNode* slow=head;
        ListNode* fast=head;
        while(true){
            if(slow->next and fast->next and fast->next->next){
                slow = slow->next;
                fast = fast->next->next;
            }
            else{
                return NULL;
            }
            if(slow == fast) break;
        }
        ListNode* hd = head;
        while(hd != fast){
            hd = hd -> next;
            fast = fast ->next ;
        }
        return hd;
    }
};
```

<hr>

## 239. Sliding Window Maximum
单调队列
https://blog.csdn.net/roufoo/article/details/78443281

每个元素进出队列1次，复杂度O(N)


```
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        // get max in O(1), delete in O(1)
        if(k == 1) return nums;
        int front=0,rear=0;
        vector<int> result;
        // first is value, second is id
        deque<pair<int,int>> q;
        for(int front=0,i=0;i<nums.size();i++){
            // enqueue, pop smaller numbers in the rear of queue
            while(!q.empty() and q.back().first<nums[i]){
                q.pop_back();
            }
            q.push_back({nums[i],i});
            
            // if the front of queue is out-dated, delete it from the front
            while(!q.empty() and  q.front().second < front){
                q.pop_front();
            }
            if(i >= k-1){
                result.push_back(q.front().first);
                front++;
            } 
        }
        return result;
    }
};
```

<hr>

## LFU cache
[blog](https://www.cnblogs.com/grandyang/p/6258459.html)

```
class LFUCache {
public:
    LFUCache(int capacity) {
        cap = capacity;
    }
    
    int get(int key) {
        if (m.count(key) == 0) return -1;
        freq[m[key].second].erase(iter[key]);
        ++m[key].second;
        freq[m[key].second].push_back(key);
        iter[key] = --freq[m[key].second].end();
        if (freq[minFreq].size() == 0) ++minFreq;
        return m[key].first;
    }
    
    void put(int key, int value) {
        if (cap <= 0) return;
        if (get(key) != -1) {
            m[key].first = value;
            return;
        }
        if (m.size() >= cap) {
            m.erase(freq[minFreq].front());
            iter.erase(freq[minFreq].front());
            freq[minFreq].pop_front();
        }
        m[key] = {value, 1};
        freq[1].push_back(key);
        iter[key] = --freq[1].end();
        minFreq = 1;
    }

private:
    int cap, minFreq;
    unordered_map<int, pair<int, int>> m;
    unordered_map<int, list<int>> freq;
    unordered_map<int, list<int>::iterator> iter;
};
```

<hr>

## 42. Trapping Rain Water
#### Brute Force
For each element, iterative to left and right, `O(N^2)`
```
class Solution {
public:
    int trap(vector<int>& height) {
        int result = 0;
        for(int i=1;i<(int)height.size()-1;i++){
            int lh=height[i],rh=height[i];
            int l=i-1,r=i+1;
            while(l>=0){
                lh = max(lh,height[l]);
                l--;
            }
            while(r<=height.size()-1){
                rh = max(rh,height[r]);
                r++;
            }
            // printf("i=%d,result=%d\n",i,(min(lh,rh)-height[i]));
            result += (min(lh,rh)-height[i]);
        }
        return result;
    }
};
```
#### DP
O(N)

Don't need to get lmax height and right max height all the time, record it by an array

```
class Solution {
public:
    int trap(vector<int>& height) {
        int size = height.size();
        if(size <= 2) return 0;
        vector<int> lMax = height;
        vector<int> rMax = height;
        int result = 0;
        for(int i=1;i<size;i++){
            lMax[i] = max(lMax[i-1],height[i]);
            rMax[size-1-i] = max(rMax[size-i],height[size-1-i]);
        }
        for(int i=1;i<size;i++){
            // printf("i=%d,sum=%d\n",i,min(lMax[i],rMax[i]) - height[i]);
            result += (min(lMax[i],rMax[i]) - height[i]);
        }
        return result;
    }
};
```
#### Two Pointer
Simliar idea to LC11 Containers with Most water
```
class Solution {
public:
    int trap(vector<int>& height) {
        int size = height.size();
        if(size <= 2) return 0;
        int result = 0;
        int left=0,right=height.size()-1;
        int lmax=0,rmax=0;
        while(left < right){
            if(height[left] < height[right]){
                if(height[left] >= lmax){
                    lmax = height[left];
                }
                else{
                    result += (lmax - height[left]);
                }
                left++;
            }
            else{
                if(height[right] >= rmax){
                    rmax = height[right];
                }
                else{
                    result += (rmax-height[right]);
                }
                right--;
            }
        }
        return result;
    }
};
```

<hr>

## 238. Product of Array Except Self
#### DP
```
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        if(nums.size()<=1) return nums;
        vector<int> left(nums.size()+2,1);
        vector<int> right(nums.size()+2,1);
        vector<int> result;
        for(int i=1;i<=nums.size();i++){
            left[i] = left[i-1] * nums[i-1];
        }
        for(int i=nums.size();i>=1;i--){
            right[i] = right[i+1] * nums[i-1];
        }
        for(int i=1;i<=nums.size();i++){
            result.push_back(left[i-1]*right[i+1]);
        }
        return result;
    }
};
```

#### No extra Space
Using result as tmp space
```
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        if(nums.size()<=1) return nums;
        vector<int> result=nums;
        result.back() = 1;
        for(int i=nums.size()-2;i>=0;i--){
            result[i] = result[i+1]*nums[i+1];
        }
        int left = 1;
        for(int i=0;i<nums.size();i++){
            result[i] = left*result[i];
            left*=nums[i];
        }
        return result;
    }
};
```

<hr>

## 127. Word Ladder
```
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> uSet;
        for(auto s:wordList) uSet.insert(s);
        unordered_set<string> visited;
        queue<pair<string,int>> q;
        q.push({beginWord,0});
        while(!q.empty()){
            auto cur = q.front(); q.pop();
            for(int i=0;i<cur.first.size();i++){
                if(cur.first == endWord) return cur.second+1;
                for(int j=0;j<26;j++){
                    string next = cur.first;
                    next[i] = 'a' + j;
                    if(uSet.count(next) and !visited.count(next)){
                        visited.insert(next);
                        q.push({next,cur.second+1});
                    }
                }
            }
        }
        return 0;
    }
};
```

<hr>

## 682. Baseball Game

```
class Solution {
public:
    int calPoints(vector<string>& ops) {
        stack<int> points;
        for(auto c:ops){
            if(c == "C"){
                points.pop();
            }
            else if(c == "+"){
                int num1 = points.top(); points.pop();
                int num2 = points.top(); points.pop();
                points.push(num2);
                points.push(num1);
                points.push(num1+num2);
            }
            else if(c == "D"){
                points.push(points.top()*2);
            }
            else{
                int x;
                sscanf(c.c_str(),"%d",&x);
                points.push(x);
            }
        }
        
        int result=0;
        while(!points.empty()){
            result+= points.top();
            points.pop();
        }
        return result;
    }
};
```

<hr>

