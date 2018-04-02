---
layout: post
title: "Move Zeroes | 把0移到数组末尾"
date: 2018-04-01
description: "Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements."
tags: array two_pointers easy
---   

## Description | 描述
Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

For example, given `nums = [0, 1, 0, 3, 12]`, after calling your function, nums should be `[1, 3, 12, 0, 0]`.

**Note:**
You must do this in-place without making a copy of the array.
Minimize the total number of operations.

## Analysis | 分析
这个题有俩基本要求，第一是要把一个数组里所有的零移动到数组末尾，第二是保持其他元素顺序不变(并且，空间复杂度最好保持1，即只在原数组内进行操作)。

## Approach | 实现

### Approach #1
一个基本的思路是，把所有的非零元素填充到数组前面，最后用零来补完。我们可以用two pointer的方法，第一个pointer（快）指向每一个元素，第二个pointer（慢）指向非零元素。

```
public void moveZeroes(int[] nums) {
		  if (nums == null || nums.length < 2)
            return;
        int non_zero = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i]!=0) {
                nums[non_zero++] = nums[i];
            }
        }
        for (int i = non_zero; i < nums.length; i++) {
            nums[i] = 0;
        }
    }
```
time: O(n)。写操作，包括`nums[non_zero++] = nums[i]`和`nums[i] = 0`一共执行了n遍。(不论任何情况都是n)

space: O(1)。因为在原数组上进行操作。

### Approach #2
Approach #1在时间上不是最优的。举个例子，如果一个数组除了最后一个元素外都是0，即[0,0,0...,0,1]，按照以上方法，要做n次操作。但在这个例子里，最优只需要一次操作就够了。

那么，要对以上方法进行改进。对于一个非0元素，其最终位置是当前位置或更前位置，如果是后者，这个元素的当前位置会被其他元素所占据，可能是0也可能是非0。我们直接把它和前面的0交换，这样就会省去一些步骤。

方法是，依旧是两个指针，快的指针`i`指遍历每个元素，慢的指针`lastNonZeroFoundAt `指向，如果快指针当前元素是非零，则交换两个位置的元素，并且两个指针都+1。

```
public void moveZeroes(int[] nums) {
        if (nums == null || nums.length < 2)
            return;

        int lastNonZeroFoundAt = 0;
        for (int i = 0; i < nums.length; i++){
            if(nums[i] != 0){
                int temp = nums[lastNonZeroFoundAt];
                nums[lastNonZeroFoundAt] = nums[i];
                nums[i] = temp;
                lastNonZeroFoundAt++;
            }
        }
    }
```
time: O(n)。当数组内的0越多，相比方法1就越优化。当数组元素只有一个非0时，复杂度是O(1);当数组元素全为非0时，复杂度和方法1相同

space: O(1)。因为在原数组上进行操作