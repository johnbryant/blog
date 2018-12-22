---

layout: post

title: "Median of Two Sorted Arrays"

date: 2018-12-22

description: "Find the median of the two sorted arrays within time O(log(n))"

tags: Array BinarySearch Hard

---

## Problem Description

There are two sorted arrays **nums1** and **nums2** of size **m** and **n** respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume **nums1** and **nums2** cannot be both empty.

#### Example 1:

> nums1 = [1, 3]
> nums2 = [2]
> 
> The median is 2.0

#### Example 2:

> nums1 = [1, 2]
> nums2 = [3, 4]
> 
> The median is (2 + 3)/2 = 2.5

Here is the code field:

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        
    }
}
```

## Analysis

The difficulty of this problem is how to achieve tha task in O(log(n)) time. If we traverse the arrays, the time will be O(n). Obviously, the binary search can make help.

First, consider what is median? In statistics, the median is used for `Dividing a set into two equal length subsets, that one subset is always greater than the other`. Suppose we have 2 arrays, A and B.

```
	left part	| 	right part
A[0], A[1], ..., A[i-1] | A[i], A[i+1], ..., A[m-1]
B[0], B[1], ..., B[j-1]	| B[j], B[j+1], ..., B[n-1]
```

If we can ensure:

1. lenth(left_part) = length(right_part)

2. max(left_part) <= min(right_part)

Then we divide all elements into two parts with equal length, and right part is always greater than left part. Then

> Median = (max(left_part) + min(right_part)) / 2



To ensure these two conditions, we just need to ensure:

1. i + j = m - i + n - j

2. A[i-1] <= B[j], B[j-1] <= A[i]

From `1`, we get j = (m+n)/2 -i. So we only find a proper `i` in array A with binary search.

For more consideration, we let `i` is from 0 to m, and `j` is from 0 to n. Because the max value of `i` is m, if m > n, j will be negative value. Thus we need to ensure m <= n.

Another consideration is, if the `m+n` is odd number, the median is one number, and if `m+n` is even number, the median is average of two numbers. If `m+n` is odd number, we let the left side has one more element, thus, to make the lenth of left and right be equal,  the new equation will be`i + j = m - i + n - j +1`, thus `j = (m+n+1)/2-i`.



Here is the code use binary search:

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        // make sure m <= n, so that j > 0
        if (nums1.length > nums2.length) {
            int[] temp = nums1;
            nums1 = nums2;
            nums2 = temp;
        }
        int m = nums1.length;
        int n = nums2.length;
        int iMin = 0, iMax = m;
        
        while (iMin <= iMax) {
            int i = (iMin + iMax) / 2;
            int j = (m+n+1)/2-i;
            if (i > iMin && nums1[i-1] > nums2[j]) {
                iMax = i - 1;
            } else if (i < iMax && nums2[j-1] > nums1[i]) {
                iMin = i + 1;
            } else { // i is perfect
                int maxLeft = 0;
              	// consider the special case
                if (i == 0) {
                    maxLeft = nums2[j-1];
                } else if (j == 0) {
                    maxLeft = nums1[i-1];
                } else {
                    maxLeft = Math.max(nums1[i-1], nums2[j-1]);
                }
              	// if m+n is odd number, return the max value of left part
                if ((m+n)%2 == 1) return maxLeft;
                
                int minRight = 0;
                if (i == m) {
                    minRight = nums2[j];
                } else if (j == n) {
                    minRight = nums1[i];
                } else {
                    minRight = Math.min(nums1[i], nums2[j]);
                }
              	// if m+n is even number, return average of maxLeft and minRight
                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
}
```

#### Complexity

- Time: O(log (min(m, n))). We make sure m <= n, so we only traverse the array nums1 with binary search

- Space: O(1)



That's all for this problem!
