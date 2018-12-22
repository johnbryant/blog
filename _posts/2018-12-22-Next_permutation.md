---

layout: post

title: "Next Greater lexicographical Permutation |字典序"

date: 2018-12-22

description: "Implement**next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers."

tags: Array Medium

---

## Description

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be **[in-place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3 -> 1,3,2
3,2,1 -> 1,2,3
1,1,5 -> 1,5,1
```

Here is the code field:

```java
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        
    }
};
```

## Analysis

First, let's make sense what is **Lexicographical permutation (字典序)**? Given a sequence of number: `1,2,3`. All of the lexicographical permutation in ascending order is: `1,2,3`, `1,3,2`, `2,1,3`, `2,3,1`, `3,1,2`, `3,2,1`. 

This problem asks us to get the next greater permutation. For example, the next permutation of `1,5,8,4,7,6,5,3,1` is `1,5,8,5,1,3,4,6,7`, how can we make that?

![lexicographical_permutation](/blog/images/posts/2018-12-22-Next_permutation/lexicographical_permutation.png)

The picture shows the chart of permutation `1,5,8,4,7,6,5,3,1`, we can observe that there's a pair of special numbers: `a[i-1]` and `a[i]`. Before them, the array is in ascending order; after them, the array is in descending order. To find the next great permutation, we need to swap `a[i-1]` with number which is just larger than `a[i-1`], say `a[j]`. Then we sort the array from index `i` to the end of array in ascending order. Fortunately, after `a[i-1]` swaps with `a[j]`, the numbers from `a[i]` to the end of the array are still in descending order, so that we just need to reverse the numbers from `a[i]` to end of the array.

Thus, here is what we need to do:

1. Find `a[i-1]` and `a[i]`

2. find `a[j]`

3. reverse the array from index `i` to end of the array



Here is the code in Java:

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int pos = -1;
        for (int i = nums.length - 1; i > 0; i--) {
            if (nums[i] > nums[i-1]) {
                pos = i-1;
                break;
            }
        }
        if (pos > -1) {
            for (int i = nums.length - 1; i > pos; i--) {
                if (nums[i] > nums[pos]) {
                    swap(nums, i, pos);
                    break;
                }
            }
        }
        reverse(nums, pos+1);
    }
    
    // reverse the array
    public void reverse(int[] nums, int start) {
        int end = nums.length - 1;
        while (start < end) {
            swap(nums, start, end);
            start++;
            end--;
        }
    }
    
    // swap the position of two element
    public void swap(int[] nums, int a, int b) {
        int temp = nums[b];
        nums[b] = nums[a];
        nums[a] = temp;
    }
    
}
```

#### Complexity

- Time: O(n). In worst case, only two scans of the array are needed.

- Space: O(1). No extra space is used, and in place replacement is done.



That's all of this problem!
