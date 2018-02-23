---
layout: post
title: "Simple Regular Expression Matching"
date: 2018-02-23 
description: "Implement regular expression matching with support for '.' and '*'."
tags: dynamic_programming backtracking
---   

## Description 问题描述
Implement regular expression matching with support for '.' and '*'.

实现包含 . 和 * 的正则表达式匹配

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true

```

## Analysis 问题分析
首先从动态规划的角度来分析。

要用动态规划来解决，就要先找到子问题。判断string和pattern是否匹配的子问题是什么呢？是string的某一部分是否和pattern的某一部分匹配。可以构建一个二维数组dp[ ][ ]来存储匹配结果，dp[i][j]表示string[i:m]和pattern[j:n]是否匹配（m和n分别表示string和pattern的长度），最后我们只要求出dp[0][0]就可以知道string和pattern是否匹配啦。

找到子问题后，下一个要考虑的是递推关系，即原问题如何由子问题求得呢？从字符串末尾开始，我们每次只向前进行1位的匹配，即判断string[i]和pattern[j]是否匹配，然后再结合上下文情况来判断，下面是一些上下文情况分析：

首先，判断string和pattern的当前位是否匹配，`current_match = (string[i] == pattern[j]) or (pattern[j] == '.')`

然后，判断pattern中当前位的下一位是否是 * ，如果是，情况就比较复杂

因为 * 可以表示0个，也可以表示多个，这时

`dp[i][j] = dp[i][j+2]` // 把a* 当做空值忽略掉

或者

`dp[i][j] = dp[i+1][j]` // a* 代表重复的多个值，继续用a* 去匹配string[i+1]

如果pattern中当前位的下一位不是 * ，就是简单的情况，只需判断string和pattern当前为的下一位是否匹配就可以了，即 `dp[i][j] = current_match && dp[i+1][j+1]`

## Solution 解答
#### Dynamic Programming 动态规划
```
	public boolean isMatch(String s, String p) {
		boolean[][] dp = new boolean[s.length()+1][p.length()+1];
		dp[s.length()][p.length()] = true;
		for (int i = s.length(); i >= 0; i--) {
			for (int j = p.length() - 1; j >= 0; j--) {
				boolean current_match = i < s.length() && (s.charAt(i) == p.charAt(j) || p.charAt(j) == '.');
				if (j < p.length() - 1 && p.charAt(j+1) == '*') {
					dp[i][j] = dp[i][j+2] || (current_match && dp[i+1][j]);
				} else {
					dp[i][j] = current_match && dp[i+1][j+1];
				}
			}
		}
		return dp[0][0];
	}

```