---
layout: post
title: "Product of Array Except Self"
date: 2018-02-18
description: "Given an array nums[], return an array such that output[i] is equal to the product of all the elements of nums except nums[i]"
tag: Array
---   

## Description

Given an array of n integers where n > 1, `nums`, return an array `output` such that `output[i]` is equal to the product of all the elements of nums except `nums[i]`.

Solve it **without division** and in **O(n)**.

For example, given `[1,2,3,4]`, return `[24,12,8,6]`.

**Follow up:**

Could you solve it with **constant space complexity**? (Note: The output array does not count as extra space for the purpose of space complexity analysis.)

## Analysis
Solve it in O(n) time, O(n) space and without division, how does it possible?

There is an idea of **2 directions**. The first iteration is from left-to-right, we calculate the product on left side of output[i]. The second iteration is from right-to-left, we calculate the product on right side of output[i], than we get the final result. No division, in O(n) time and space. Bingo!

## Solutions

```
public int[] productExceptSelf(int[] nums) {
        int[] res = new int[nums.length];
        res[0] = 1;
        // use res[i] to record the product on left side from left to right
        for (int i = 1; i < res.length; i++) {
            res[i] = res[i-1] * nums[i-1];
        }

        // use 'right' to record the product on right side
        int right = 1;
        // get the final result from right to left
        for (int i = res.length-1; i >= 0; i--) {
            res[i] *= right;
            // update 'right'
            right *= nums[i];
        }
        return res;
    }

```

很有意思呀，哈哈