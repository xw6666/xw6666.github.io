---
title: 挑战leetcode hot 100（一）
date: 2023-01-23 15:39:30
tags: 算法
categories: 随便一刷
---



# 本篇文章记录leetcode hot 100中的1到10题的求解过程

## [1. 两数之和](https://leetcode.cn/problems/two-sum/)

![](/images/leetcode1/1.png)

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

## [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

![](/images/leetcode1/2.png)

类似归并排序，两边一路归并的加过去就好了，最后处理长的那一条链表

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
 typedef ListNode Node;
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        Node* cur1 = l1;
        Node* cur2 = l2;
        int t = 0;
        Node* head = new Node(0, NULL);
        Node* cur = head;
        while(cur1 && cur2) {
            t += cur1->val + cur2->val;
            Node* newNode = new Node(t % 10, NULL);
            cur->next = newNode;
            cur = newNode;
            t /= 10;
            cur1 = cur1->next;
            cur2 = cur2->next;
        }

        while(cur1) {
            t += cur1->val;
            Node* newNode = new Node(t % 10, NULL);
            cur->next = newNode;
            cur = newNode;
            t /= 10;
            cur1 = cur1->next;
        }

        while(cur2) {
            t += cur2->val;
            Node* newNode = new Node(t % 10, NULL);
            cur->next = newNode;
            cur = newNode;
            t /= 10;
            cur2 = cur2->next;
        }
        
        while(t) {
            Node* newNode = new Node(t % 10, NULL);
            cur->next = newNode;
            cur = newNode;
            t /= 10;
        }
        return head->next;
    }
};
```

## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

![](/images/leetcode1/3.png)

双指针扫一遍即可

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size();
        if(!n) return n;
        int h[130] = {0};
        int l = 0, r = 1;
        h[s[l]]++;
        int ans = 0;
        while(r < n) {
            if(h[s[r]]) {
                ans = max(ans, r - l);
                while(l < r && h[s[r]]) {
                    h[s[l]]--;
                    l++;
                }
            }
            h[s[r]]++;
            r++;
        }

        ans = max(ans, r - l);
        return ans;
    }
};
```

