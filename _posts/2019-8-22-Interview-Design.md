---
layout: post
title:  "Design"
categories: Interview
---

## 359 Logger Rate Limiter 
```
class Logger {
public:
    Logger() {}
    
    bool shouldPrintMessage(int timestamp, string message) {
        auto it = uMap.find(message);
        bool result = false;
        if(it == uMap.end() or timestamp - *it >= 10 ){
            result = true;
            uMap[message] = timestamp;
        }
        return result;
    }

private:
    unordered_map<string,int> uMap;
};
```

<hr>

## 346 Moving Average from Data Stream(Lintcode 642)
```
class MovingAverage {
public:
    MovingAverage(int size) {
        this->sum = 0;
        this->size = size;
    }

    double next(int val) {
        if(d.size() == size){sum -= d.front(); d.pop_front();}
        else{}
        d.push_back(val);
        sum += val;
        return sum/d.size();
    }
private:
    int size;
    double sum;
    deque<int> d;
};

```

<hr>

## 362 Design Hit Counter    
#### Queue simple
```
class HitCounter {
public:
    HitCounter() {}
    
    void hit(int timestamp) {
        s.push_back(timestamp);      
    }
    
    int getHits(int timestamp) {
        while(timestamp - s.front() > 300) s.pop_front();
        return s.size();
    }

private:
    deque<int> s;
};
```

## Follow up:
What if the number of hits per second could be very large? Does your design scale?

If the record is only for minutes, we can use an array to record the number of hit get currently.

Becasue it is mono increasing, we can just use array to record the time stamp and the hit number
```
class HitCounter {
public:
    /** Initialize your data structure here. */
    HitCounter() {
        th.resize(300);
    }
    
    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    void hit(int timestamp) {
        int idx = timestamp % 300;
        if (th[idx].first != timestamp) {
            th[idx].first = timestamp;
            th[index].second++;;
        } else {
            th[index].second++;
        }
    }
    
    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    int getHits(int timestamp) {
        int res = 0;
        for (int i = 0; i < 300; ++i) {
            if (timestamp - th[i].first < 300) {
                res += th[i].second;
            }
        }
        return res;
    }

private:
    vector<pair<int,int>> th;
};
```

<hr>

## 281 Zigzag Iterator	(Lint 540)

```
class ZigzagIterator {
public:
    ZigzagIterator(vector<int>& v1, vector<int>& v2) {
        this->v1=v1,this->v2=v2;
        index1=index2=0;
        first = true;
    }

    int next() {
        int result;
        if(index2 == v2.size() or (first and index1<v1.size())){
            result = v1[index1++];
        }
        else{
            result = v2[index2++];
        }
        first = !first;
        return result;
    }

    bool hasNext() {
        if(index1<v1.size() or index2<v2.size()) return true;
        return false;
    }
private:
    vector<int> v1,v2;
    int index1,index2;
    bool first;
};
```

<hr>

## Lint 541 Zigzag Iterator II
Follow up Zigzag Iterator: What if you are given k 1d vectors? How well can your code be extended to such cases? The "Zigzag" order is not clearly defined and is ambiguous for k > 2 cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic".

Example:
```
Input: k = 3
vecs = [
    [1,2,3],
    [4,5,6,7],
    [8,9],
]
Output: [1,4,8,2,5,9,3,6,7]
```

#### brute focce index
```
class ZigzagIterator2 {
public:
    /*
    * @param vecs: a list of 1d vectors
    */ZigzagIterator2(vector<vector<int>>& vecs) {
        this->vecs = vecs;
        curIndex = 0;
        curPop = 0;
        for(auto x:vecs) totalSize += x.size();
        indexs.resize(vecs.size());
        for(auto &x:indexs) x = 0;
    }

    int next() {
        while(indexs[curIndex] == vecs[curIndex].size()){
            curIndex = (curIndex+1)%vecs.size();
        }
        int result = vecs[curIndex][indexs[curIndex]++];
        curIndex = (curIndex+1)%vecs.size();
        curPop++;
        return result;
    }

    bool hasNext() {
        return curPop < totalSize;
    }
private:
    vector<vector<int>> vecs;
    int curPop;
    int totalSize;
    int curIndex;
    vector<int> indexs;
    
};
```

<hr>

## 146 LRU Cache
Using queue and Hash

O(1) lookup: hashmap

O(1) deletion, insertion and updating: double linked list

 In Which HashMap will hold the keys and address of the Nodes of Doubly LinkedList . And Doubly LinkedList will hold the values of keys.

When get: use hash to get the address of the linked list node, then fetch the value

When put: if it already exist, bring it to the front of the double linked list, if not, put it in the front and delete the rear(also remember to delete it in hashMap)

```
class LRUCache {
public:
    LRUCache(int capacity):capacity(capacity), size(0) {
        
    }
    
    int get(int key) {
        if(m1.find(key) == m1.end()) return -1;
        l.erase(m2[key]);
        l.push_front(key);
        m2[key] = l.begin();
        return m1[key];
    }
    
    void put(int key, int value) {
        if(m1.find(key) == m1.end()){
            if(size < capacity){
                size++;
            }
            //clean rear
            else{
                int keyVal = l.back();
                l.pop_back();
                m1.erase(keyVal);
                m2.erase(keyVal);
            }
        }
        else{
            l.erase(m2[key]);
        }
        m1[key] = value;
        l.push_front(key);
        m2[key] = l.begin();
    }
private:
    int capacity,size;
    list<int> l;
    unordered_map<int,int> m1;
    unordered_map<int,list<int>::iterator> m2;
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
 ```

<hr>