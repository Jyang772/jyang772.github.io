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
	m = 3

<script src="https://gist.github.com/Jyang772/e2e8651468d73a4acf0652e5d3f1f97d.js?file=410"></script>



We must also sum from the end of the previous split to the current ith element.
	
	For dp[4][3] - dp[3][2], this means the mth split (last split) has a sum of {10}.
	For dp[4][2] - dp[3][1], the last split has a sum of {5,10}

	
Example:
	    dp[1][1] = 7
	    dp[2][1] = 9

	    dp[3][2] = min(dp[3][2],max(dp[1][1],sum{2,5}))
	    dp[3][2] = min(dp[3][2],max(dp[2][1],sum{5}))
	    dp[3][2] = 7
	
	
