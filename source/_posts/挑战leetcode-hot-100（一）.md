---
title: 挑战leetcode hot 100（一）
date: 2023-01-23 15:39:30
tags: 算法
categories: 随便一刷
---



`本篇文章记录leetcode hot 100中的1到10题的求解过程`

`1. 两数之和`

题目来源于[这里](https://leetcode.cn/problems/two-sum/)

![](/source/_posts/挑战leetcode-hot-100（一）/1.png)

非常简单的一道题，将题目转换成`x + nums[i] == target`后得到`x == target - nums[i]`即可哈希。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> mp;
        int n = nums.size();
        for(int i = 0;i < n;i++) {
            if(mp.count(nums[i])) return {mp[nums[i]], i};
            
            mp[target - nums[i]] = i;
        }

        return {};
    }
};
```

