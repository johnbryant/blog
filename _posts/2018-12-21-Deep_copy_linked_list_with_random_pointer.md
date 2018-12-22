---

layout: post

title: "Deep Copy Linked List with Random Pointer"

date: 2018-12-21

description: "A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null. Return a deep copy of the list."

tags: LinkedList HashTable Medium

---

## Description

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

```
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        
    }
}
```



## Analysis

First, what is **deep copy**? Let's conside the code:

```java
Object a = new Object();
Object b = a;	// now b and a point to the same memory
```

Here, `b` is a **shallow copy** of `a`. Shallow copy duplicates as little as possible. A shallow copy of a collection is a copy of the collection structure, not the elements. With a shallow copy, two collections now share the individual elements.

**Deep copy** duplicates everything. A deep copy of a collection is two collections with all of the elements in the original collection duplicated.

For this problem, making a deep copy of a linked list means, copy every attribute of every node to the new memory,  including variables and pointers.

Then look at how the linked list looks like:

![linkedlist](/blog/images/posts/2018-12-21-Deep_copy_linked_list_with_random_pointer/linkedlist.png)

For every node, there is a `variable` called *label*, a `pointer` called *next* which point to the next node, and a `pointer` named *random* which point to random node in the list or null.

Copy the variable and the next pointer is easy, but the difficulty is how to copy the random pointer. 

The keyword I think is to make sure that the node in old list and and the copy node in new list have a relative position, we copy the random pointer according to the relative position. Now I conclude 3 approaches:

## Approach 1: Recursive using HashMap | 递归

For each node, we copy the `label`, then copy the `next` and `random` recursively.

We use a `HashMap<oldNode, newNode>` to record the relative position of a node and its copy node.

One trap in recursion is that there may be a **cycle** because the `random` pointer can point to any node. The HashMap can check whether there is a duplicate node. If one node has already been copied, we return the reference of it, rather than create a new node.

Here is the code:

```java
public class Solution {
  // HashMap which holds old nodes as keys and new nodes as its values.
  HashMap<RandomListNode, RandomListNode> visitedHash = new HashMap<RandomListNode, RandomListNode>();

  public RandomListNode copyRandomList(RandomListNode head) {

    if (head == null) {
      return null;
    }

    // If we have already processed the current node, then we simply return the cloned version of it.
    if (this.visitedHash.containsKey(head)) {
      return this.visitedHash.get(head);
    }

    // Create a new node with the label same as old node. (i.e. copy the node)
    RandomListNode node = new RandomListNode(head.label);

    // Save this value in the hash map. This is needed since there might be
    // loops during traversal due to randomness of random pointers and this would help us avoid them.
    this.visitedHash.put(head, node);

    // Recursively copy the remaining linked list starting once from the next pointer and then from the random pointer.
    // Thus we have two independent recursive calls.
    // Finally we update the next and random pointers for the new node created.
    node.next = this.copyRandomList(head.next);
    node.random = this.copyRandomList(head.random);

    return node;
  }
}
```

#### Complexity

- Time: O(N), where N is the number of nodes in the linked list.

- Space: O(N), If we look closely, we have the recursion stack and we also have the space complexity to keep track of nodes already cloned i.e. using the visited dictionary. But asymptotically, the complexity isO(N).

## Approach 2: Iterative using HashMap | 迭代(使用HashMap)

Similar approach with resursive approach, copy the node one by one, and use HashMap too.

```java
public class Solution {
  // Visited dictionary to hold old node reference as "key" and new node reference as the "value"
  HashMap<RandomListNode, RandomListNode> visited = new HashMap<RandomListNode, RandomListNode>();

  public RandomListNode getClonedNode(RandomListNode node) {
    // If the node exists then
    if (node != null) {
      // Check if the node is in the visited dictionary
      if (this.visited.containsKey(node)) {
        // If its in the visited dictionary then return the new node reference from the dictionary
        return this.visited.get(node);
      } else {
        // Otherwise create a new node, add to the dictionary and return it
        this.visited.put(node, new RandomListNode(node.label));
        return this.visited.get(node);
      }
    }
    return null;
  }

  public RandomListNode copyRandomList(RandomListNode head) {

    if (head == null) {
      return null;
    }

    RandomListNode oldNode = head;
    // Creating the new head node.
    RandomListNode newNode = new RandomListNode(oldNode.label);
    this.visited.put(oldNode, newNode);

    // Iterate on the linked list until all nodes are cloned.
    while (oldNode != null) {
      // Get the clones of the nodes referenced by random and next pointers.
      newNode.random = this.getClonedNode(oldNode.random);
      newNode.next = this.getClonedNode(oldNode.next);

      // Move one step ahead in the linked list.
      oldNode = oldNode.next;
      newNode = newNode.next;
    }
    return this.visited.get(head);
  }
}
```

#### Complexity

- Time: O(N), one loop

- Space: O(N), as we use HashMap

## Approach 3: Iterative with linked list | 迭代

In this approach, we do not use HashMap to store the relative position of old node and copy node, instead, we only use the given linked list.

For example, the old list may looks like: A --> B --> C --> D, we insert the copy node after the origin node, the result looks like: A --> A\` --> B --> B\` --> C --> C\` --> D --> D\`.

Here is the code:

```java
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        if (head == null) return null;
        RandomListNode temp, iter = head;
        
        // first round, create the copy node, and intert it to the next of old node
        while (iter != null) {
            temp = iter.next;
            RandomListNode cur = new RandomListNode(iter.label);
            iter.next = cur;
            cur.next = temp;
            iter = temp;
        }
        
        // second round, copy the random pointer
        iter = head;
        while (iter != null) {
            if (iter.random != null) {
                iter.next.random = iter.random.next;
            }
            iter = iter.next.next;
        }
        
        // copy the copy node from old list to new list
      	// do not forget recover the origin list
        RandomListNode result = new RandomListNode(0);
        RandomListNode copy = result;
        iter = head;
        while (iter != null) {
            temp = iter.next.next;
            copy.next = iter.next;
            iter.next = temp;
            
            copy = copy.next;
            iter = temp;
        }
        return result.next;
    }
}
```

#### Complexity

- Time: O(N)

- Space: O(1), do not need any extra space



That's all of this problem!


