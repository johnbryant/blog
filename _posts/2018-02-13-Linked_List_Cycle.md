---

layout: post
title: "Linked List Cycle"
date: 2018-02-13 
description: "Given a linked list, determine if it has a cycle in it."

tags: TwoPointers LinkedList
---

## Description

```
Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?
```

## Solution

###Approach #1 (Hash Table)
If ignoring the follow-up, we can use hash table. We go through each node one by one and record each node's reference in a hash table. If current node’s reference is in the hash table, then return true.

```
public boolean hasCycle(ListNode head) {
    Set<ListNode> nodesSeen = new HashSet<>();
    while (head != null) {
        if (nodesSeen.contains(head)) {
            return true;
        } else {
            nodesSeen.add(head);
        }
        head = head.next;
    }
    return false;
}
```

#### Complexity analysis

- Time complexity: O(n)
- Space complexity: O(n)

### Approach #2 (Two Pointers)

Imagine two runners running on a track at different speed. What happens when the track is actually a circle? 

We use a slow pointer and a fast pointer. The slow pointer moves one step at a time while the fast pointer moves two steps at a time. Consider a cyclic list and imagine the slow and fast pointers are two runners racing around a circle track. The fast runner will eventually meet the slow runner.

```
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    ListNode slow = head;
    ListNode fast = head.next;
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}
```

#### Complexity analysis

- Time complexity: O(n)
- Space complexity: O(1)
