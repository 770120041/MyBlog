---
layout: post
title:  "Leetcode Random"
categories: Interview
---

## 水塘抽样
```
https://blog.csdn.net/My_Jobs/article/details/48372399
```

## Fisher–Yates shuffle
[website](https://gaohaoyang.github.io/2016/10/16/shuffle-algorithm/)

```
-- To shuffle an array a of n elements (indices 0..n-1):
for i from n−1 down to 1 do
     j ← random integer such that 0 ≤ j ≤ i
     exchange a[j] and a[i]
```

## 384. Shuffle an Array
```

class Solution {
public:
    Solution(vector<int> nums): v(nums) {}
    
    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        return v;
    }
    
    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
        vector<int> res = v;
        for (int i = res.size()-1; i > 0; --i) {
            int ranNum = rand() % (i+1);
            swap(res[ranNum],res[i]);
        }
        return res;
    }
    
private:
    vector<int> v;
};
```

<hr>

## 398. Random Pick Index
```
class Solution {
public:
    Solution(vector<int>& nums) {
        for(int i=0;i<nums.size();i++){
            auto it = uMap.find(nums[i]);
            if(it == uMap.end()){
                vector<int> tmp{i};
                uMap[nums[i]] = tmp;
            }
            else it->second.push_back(i);
        }
    }
    
    int pick(int target) {
        auto cur = uMap[target];
        int randPos = rand()%cur.size();
        return cur[randPos];
    }
private:
    unordered_map<int,vector<int>> uMap;
};
```

<hr>

## 382. Linked List Random Node
#### 水塘抽样
维护一个大小为1的水塘，从第二个位置开始，每次如果rand==0,那么更新结果
```

class Solution {
public:
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        this->head = head;
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        ListNode* tmp = head->next;
        int result = head->val;
        int i = 2;
        while(tmp){
            int possibility = rand() %i;
            if(possibility == 0) result = tmp->val;
            i++;
            tmp = tmp->next;
        }
        return result;
    }
private:
    ListNode*head;
};
```

<hr>

## 380. Insert Delete GetRandom O(1)
#### Brute force
```
class RandomizedSet {
public:
    /** Initialize your data structure here. */
    unordered_set<int> uSet;
    RandomizedSet() {
        
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        if(!uSet.count(val)){
            uSet.insert(val);
            return true;
        }   
        return false;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val) {
        if(uSet.count(val)){
            uSet.erase(val);
            return true;
        }
        return false;
    }
    
    /** Get a random element from the set. */
    int getRandom(){
        int size = uSet.size();
        int pos = rand() % size;
        auto it = uSet.begin();
        for(int i=0;i<pos;i++){
            it++;
        }
        return *(it);
    }
};

```

#### Tricky
这道题让我们在常数时间范围内实现插入删除和获得随机数操作，如果这道题没有常数时间的限制，那么将会是一道非常简单的题，我们直接用一个 set 就可以搞定所有的操作。但是由于时间的限制，我们无法在常数时间内实现获取随机数，所以只能另辟蹊径。此题的正确解法是利用到了一个一维数组和一个 HashMap，其中数组用来保存数字，HashMap 用来建立每个数字和其在数组中的位置之间的映射，对于插入操作，我们先看这个数字是否已经在 HashMap 中存在，如果存在的话直接返回 false，不存在的话，我们将其插入到数组的末尾，然后建立数字和其位置的映射。删除操作是比较 tricky 的，我们还是要先判断其是否在 HashMap 里，如果没有，直接返回 false。由于 HashMap 的删除是常数时间的，而数组并不是，为了使数组删除也能常数级，我们实际上将要删除的数字和数组的最后一个数字调换个位置，然后修改对应的 HashMap 中的值，这样我们只需要删除数组的最后一个元素即可，保证了常数时间内的删除。而返回随机数对于数组来说就很简单了，我们只要随机生成一个位置，返回该位置上的数字即可，参见代码如下：

```
class RandomizedSet {
public:
    /** Initialize your data structure here. */
    vector<int> numbers;
    unordered_map<int,int> uMap;
    RandomizedSet() {
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        auto it = uMap.find(val);
        if(it == uMap.end()){
            uMap[val] = numbers.size();
            numbers.push_back(val);
            return true;
        }
        return false;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    // swap with elements in the rear
    bool remove(int val) {
        auto it = uMap.find(val);
        if(it == uMap.end()) return false;
        int index = uMap[val];
        uMap[numbers[numbers.size()-1]] = index;
        numbers[index] = numbers[numbers.size()-1];
        uMap.erase(val);
        numbers.pop_back();
        return true;
    }
    
    /** Get a random element from the set. */
    int getRandom() {
        int pos = rand() % numbers.size();
        return numbers[pos];
    }
};
 ```

<hr>

## 381 Insert Delete GetRandom O(1) - Duplicates allowed
```
class RandomizedCollection {
public:
    /** Initialize your data structure here. */
    RandomizedCollection() {
        
    }
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    bool insert(int val) {
        m[val].insert(nums.size());
        nums.push_back(val);
        return m[val].size() == 1;
    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    bool remove(int val) {
        if (m[val].empty()) return false;
        int idx = *m[val].begin();
        m[val].erase(idx);
        //if not rear of the array
        if (nums.size() - 1 != idx) {
            int t = nums.back();
            nums[idx] = t;
            m[t].erase(nums.size() - 1);
            m[t].insert(idx);
        } 
        nums.pop_back();
        return true;
    }
    
    /** Get a random element from the collection. */
    int getRandom() {
        int pos = (rand())%nums.size();
        return nums[pos];
    }
private:
    //store numbers sequentially. When a number is inserted, insert the index to unordered_set. When removed, change with numbers in the rear
    vector<int> nums;
    unordered_map<int, unordered_set<int>> m;
};

```
 
<hr>