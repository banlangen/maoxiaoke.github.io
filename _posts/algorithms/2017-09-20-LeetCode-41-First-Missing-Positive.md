---
layout: post
title: "leetcode 41. First Missing Positive"
date: 2017-09-20 19:00:00 +0800 
categories: 算法
tags: 
    - others
---
* content
{:toc}

这里收集[`LeetCode`](https://leetcode.com)上的算法题目，并根据能找到的解答，加上自己的理解和思考，重新整理。

<!-- more -->

## Divide Two Integers

#### Description

>[原题链接](https://leetcode.com/problems/first-missing-positive/description/)

#### Solution

##### First 

&emsp;&emsp;要求是在O(n)的时间复杂度内，而且还必须是常量级的空间，所以决定复用nums数组，用nums[i]的下标i来表示正整数i + 1，然后用nums[i]的值来表示i + 1是否已经出现，但是这个时候如果用单纯的bool型来表示存在或者不存在，那么就丢失了本来存在于数组中的value的信息，所以为了保留value值，本题采取的思路是使用正负号来标志数据是否已经出现，具体的就是说用负数表示该下标所代表的正整数是否出现，正数表示没有出现。  
&emsp;&emsp;所以先进行一遍遍历，将其中的负数和零都赋值为MAX，赋值为MAX是因为，对于任何大于nums.size()的值，我们都可以将其忽略，出现这个值得时候肯定意味着之前已经有数被忽略了，所以赋值为MAX对我们的结果没有影响。  
&emsp;&emsp;然后再从数组的第一个元素开始遍历，取出其中的值，这个值所代表的正整数只要在nums.size()之内，那么我们就要找到这个值所对应的下标，并将对应的值更改为负值。  
![leetcode41_01](http://ovwkcbdpf.bkt.clouddn.com/image/leetcode41/leetcode41_01.png)  

##### Second 

&emsp;&emsp;第二种解法的想法是如果正整数i > 0 && < nums.size()将正整数i置换到数组nums的i-1的位置上，之后再从头开始遍历，当发现 nums[i] != i + 1的时候，说明这个i + 1就是我们要找的值。

#### Code

##### First

```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        if (nums.size() == 0) {
            return 1;
        }
        for (int i = 0; i< nums.size(); i++) {
            if (nums[i] <= 0) {
                nums[i] = INT_MAX;
            }
        }
        for (int i = 0; i < nums.size(); i++) {
            int num = abs(nums[i]);
            if (num <= nums.size()) {
                nums[num - 1] = -abs(nums[num - 1]);
            } 
        }
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) {
                return i + 1;
            }
        }
        return nums.size() + 1;
    }
};
```

##### Second

```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            while (nums[i] > 0 && nums[i] <= size && nums[nums[i] - 1] != nums[i]) {
                swap(nums[i], nums[nums[i] - 1]);
            }
        }

        for (int i = 0; i < size; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        return size + 1;
    }
};
```

#### Time Complexity

O(n);