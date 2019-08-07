---
layout: post
title: Minimum Maximum Sub-arrays
date: 2019-08-07 11:20:54
tags: dynamic-programming
---


The problem is presented on Leetcode #410. The task is to find the minimum
maximum sub-array sum given an array of integers. The optimal solution involves
a binary search, but that did not come intuitively to me. The first approach
that came to my mind was dynamic programming.

Ex.
	[7,2,5,10,8]
	m = 2

	{}: denotes the minimum max sub-array sum for these elements

	dp[4][2]:
	sub-problems: 
			{},7+2+5+10
			{7}, 2+5+10
			{7,2}, 5+10
			{7,2,5}, 10	//split {7,2,5} into k-1 subarrays

