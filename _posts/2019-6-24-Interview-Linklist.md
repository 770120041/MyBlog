---
layout: post
title:  "Leetcode Linklist"
categories: Interview
---
# Fast slow pointer
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