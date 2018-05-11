---
layout: post
title: "Remove Duplicates from Sorted Array | O(1)空间内移除数组重复内容"
date: 2018-05-11
description: "Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length."
tags: array two_pointers easy
---   

## Description | 描述
Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

给定一个有序的数组nums，要求删除重复内容，使每个元素只出现一次并返回新数组的长度。

要求：空间复杂度为O(1)，即不能创建额外的数组，必须在原数组上进行操作

## Analysis | 分析
这个问题的难点在于如何在原数组上进行元素删除。我们知道java的Array不像LinkedList那样可以直接删除中间的元素，那么只能是用“覆盖”来实现了，即覆盖掉不需要的值。对于原数组，我们肯定要遍历一遍，同时又要在原数组上产生一个长度较小的新的数组，这样就需要两个指针，也就是two pointers的思想。

## Approach | 实现
根据上面的思想，用pointer1遍历原数组的每一个元素，把不重复的元素依次放到前面去，位置由pointer2来决定。

```
public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        int index = 1;
        int currentValue = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != currentValue) {
                currentValue = nums[i];
                nums[index] = nums[i];
                index++;
            }
        }
        return index;
    }
```
time: O(n)

space: O(1)。 因为在原数组上进行操作。


---

**接下来，问题稍微升级一下**

**Let's make the problem more complex**

## Description | 描述
Given a sorted array nums, remove the duplicates in-place such that duplicates appeared at most twice and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

依旧是在O(1)空间内去重，不过这次，允许每个元素最多出现2次。

## Analysis | 分析
还是相同的思路，不过这次要增加一个变量来判断重复的次数，当重复次数为2，则保留，当次数大于2，则忽略。

Need a variable to count the repeat, when repeating 2 times, keep the value, otherwise, remove it.


## Approach | 实现
###Approach #1 (My solution)
```
public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        int currentValue = nums[0];
        int currentRepeat = 1;
        int index = 1;
        
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == currentValue) {
                if (currentRepeat == 1) {
                    currentRepeat++;
                    nums[index++] = nums[i];
                }
            } else {
                currentValue = nums[i];
                nums[index++] = nums[i];
                currentRepeat = 1;
            }
        }
        
        return index;
    }

```
time: O(n)

space: O(1)

###Approach #2 (More elegant)

```
public int removeDuplicates(int[] nums) {
    int i = 0;
    for (int n : nums)
        if (i < 2 || n > nums[i-2])
            nums[i++] = n;
    return i;
}

```
Only 7 lines! The key is `n > nums[i-2]`, If the same number appears more than 2 times, n == nums[i-2].