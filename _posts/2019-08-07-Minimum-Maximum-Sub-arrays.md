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

	{}: denotes the minimum max sub-array sum for these elements

	dp[4][3]:
	sub-problems: 
			{},7+2+5+10
			{7}, 2+5+10
			{7,2}, 5+10
			{7,2,5}, 10	//split {7,2,5} into k-1 subarrays


	dp[1][0] = min(dp[1][0],max(split{},k-1) , sum{7}) 	
	dp[2][1] = min(dp[2][1],max(split{7},k-1) , sum{2}) || max(split{},k-1), sum{7,2})
	dp[3][2] = min(dp[3][2],max(split{7,2},k-1) , sum{5}) || max(split({7},k-1), sum{2,5})
	dp[4][3] = min(dp[4][3],max(split({7,2,5},k-1) , sum{10})) || max(split{7},k-1), sum{2,5,10})
								   || max(split{7,2},k-1), sum{5,10})


	//This shows how we must first calculate all the previous 1 to (m-1) splits:

	dp[4][1] - dp[0][0]
	dp[4][2] - dp[0][1]
	dp[4][3] - dp[0][2]  //split first 0 elements into 2 groups
	
	dp[4][1] - dp[1][0]
	dp[4][2] - dp[1][1] //split first 1 elements into 1 groups
	dp[4][3] - dp[1][2]
	
	dp[4][1] - dp[2][0]
	dp[4][2] - dp[2][1]
	dp[4][3] - dp[2][2] //split first 2 into 2 groups
	
	dp[4][1] - dp[3][0]
	dp[4][2] - dp[3][1]
	dp[4][3] - dp[3][2] //split first 3 elements into 2 groups
	
	
We must also sum from the end of the previous split to the current ith element.
	
	For dp[4][3] - dp[3][2], this means the mth split (last split) has a sum of {10}.
	For dp[4][2] - dp[3][1], the last split has a sum of {5,10}

	
Example:
	    dp[1][1] = 7
	    dp[2][1] = 9

	    dp[3][2] = min(dp[3][2],max(dp[1][1],sum{2,5}))
	    dp[3][2] = min(dp[3][2],max(dp[2][1],sum{5}))
	    dp[3][2] = 7
	
	
