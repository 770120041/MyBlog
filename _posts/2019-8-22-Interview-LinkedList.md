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

