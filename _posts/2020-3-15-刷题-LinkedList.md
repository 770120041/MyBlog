---
layout: post
title:  Python LinkedList
categories: Interview
---

## 141. Linked List Cycle
slow and fast pointer
```py
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if not head or not head.next: return False
        slow = fast = head
        # must enter this loop at least once to be true
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast: return True
        return False
```
## 142. Linked List Cycle II
https://www.cnblogs.com/grandyang/p/4137302.html
