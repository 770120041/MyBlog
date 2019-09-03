---
layout: post
title:  Stack&PriorityQueue
categories: Interview
---

## 155. Min Stack
#### brute force but not constant time
```
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
        
    }
    
    void push(int x) {
        minV = min(minV,x);
        nums.push_back(x);
    }
    
    void pop() {
        if(nums.back() == minV){
            minV = INT_MAX;
            for(int i=0;i<nums.size()-1;i++) minV = min(minV,nums[i]);
        }
        nums.pop_back();
    }
    
    int top() {
        return nums.back();
    }
    
    int getMin() {
        return minV;
    }
private:
    vector<int> nums;
    int minV = INT_MAX;
};
```
#### Tricky - 2 stack
Using two stacks, the reason why this works is because larger values appears later will never be used as the min value.
```
class MinStack {
public:
    MinStack() {
    }
    
    void push(int x) {
        s1.push(x);
        if(s2.empty() or s2.top() >= x){
            s2.push(x);
        }
    }
    
    void pop() {
        if(s1.top() == s2.top()){
            s2.pop();
        }
        s1.pop();
    }
    
    int top() {
        return s1.top();
    }
    
    int getMin() {
        return s2.top();
    }
private:
    stack<int> s1,s2;
};

 ```

 #### One stack
 We can store the min value temporaryly inside the stack
 ```
 class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
        minVal = INT_MAX;
    }
    
    void push(int x) {
        if(x <= minVal){
            s.push(minVal);
            minVal = x;
        }
        s.push(x);
    }
    
    void pop() {
        int t = s.top(); s.pop();
        if(t == minVal){
            minVal = s.top(); s.pop();
        }
    }
    
    int top() {
        return s.top();
    }
    
    int getMin() {
        return minVal;
    }
    stack<int> s;
    int minVal;
};
```

 <hr>


## 225. Implement Stack using Queues

<hr>