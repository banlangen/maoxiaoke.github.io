---
layout: post
title: "leetcode 51. N-Queens"
date: 2017-09-25 19:00:00 +0800 
categories: 算法
tags: 
---
* content
{:toc}

这里收集[`LeetCode`](https://leetcode.com)上的算法题目，并根据能找到的解答，加上自己的理解和思考，重新整理。

<!-- more -->

## N-Queens

#### Description

>[原题链接](https://leetcode.com/problems/n-queens/description/)

#### Solution

&emsp;&emsp;第一次尝试，发现超时，关键原因在于valid函数进行了多次判断，实际上我们并没有必要每次都去判断行，列，对角线的每一个元素，我们只需要标记这行上面有没有元素就可以，所以有下面的更优的解法。

<div>
<video id='movie' width='90%' controls poster='http://ovwkcbdpf.bkt.clouddn.com/image/videopostert.png'>
    <source src='http://ovwkcbdpf.bkt.clouddn.com/image/leetcode51/2017-09-25-LeetCode-51-N-Queens.webm' type = 'video/webm'>
    Your browser does not support the video tag.
</video>
</div>
<script type='text/javascript'>document.getElementById('movie').style.height=document.getElementById('movie').scrollWidth*0.8+'px'</script>

&emsp;&emsp;

#### Code

##### 超时版本

```cpp
class Solution {
public:
    bool valid(vector<vector<char>> &res, int r, int c) {
        vector<pair<int, int>> direction = { {1, 1}, {-1, 1}, {1, -1}, {-1, -1} };  
        for (int i = 0; i < res.size(); i++) {
            if (res[i][c] == 'Q') {
                //cout << res[i][c] << endl;
                return false;
            }
        }
        for (int j = 0; j < res[0].size(); j++) {
            if (res[r][j] == 'Q') {
                //cout << res[r][j] << endl;
                return false;
            }
        }
        for (int i = 0; i < direction.size(); i++) {
            int tr = r;
            int tc = c;
            while (0 <= (tr + direction[i].second) && (tr + direction[i].second) < res.size() && 0 <= (tc + direction[i].first) && (tc + direction[i].first) < res.size()) {
                if (res[tr + direction[i].second][tc + direction[i].first] == 'Q') {
                    return false;
                }
                tr = tr + direction[i].second;
                tc = tc + direction[i].first;
            }
        }
        return true;
    }
    
    void helper(vector<vector<string>> &res, vector<vector<char>> &item, int cnt, int r, int c) {
        if (cnt == 0) {
            vector<string> nitem;
            for (int i = 0; i < item.size(); i++) {
                string str = "";
                for (int j = 0; j < item[0].size(); j++) {
                    str += item[i][j];
                }
                nitem.push_back(str);
            }
            res.push_back(nitem);
            return;
        }
        for (int i = r; i < item.size(); i++) {
            for (int j = c; j < item[0].size(); j++) {
                if (valid(item, i, j)) {
                    //cout << "i: " << i << "j: " << j << "is true;" << endl;
                    item[i][j] = 'Q';
                    helper(res, item, cnt - 1, r, c + 1);
                    item[i][j] = '.';
                }
                
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> res;
        vector<vector<char>> item(n, vector<char>(n, '.'));
        if (n == 0) {
            return res;
        }
        helper(res, item, n, 0, 0);
        return res;
    }
};
```

##### 优化

```cpp
class Solution {
public:
    void helper(vector<vector<string>> &res, vector<vector<char>> &item, vector<bool> &col, vector<bool> &diag_x, vector<bool> &diag_y, int cnt, int i) {
        if (cnt == 0) {
            vector<string> nitem;
            for (int i = 0; i < item.size(); i++) {
                string str = "";
                for (int j = 0; j < item[0].size(); j++) {
                    str += item[i][j];
                }
                nitem.push_back(str);
            }
            res.push_back(nitem);
            return;
        }
        for (int j = 0; j < item[0].size(); j++) {
            if (col[j] && diag_x[i + j] && diag_y[i - j + item.size() - 1]) {
                item[i][j] = 'Q';
                col[j] = false;
                diag_x[i + j] = false;
                diag_y[i - j + item.size() - 1] = false;
                helper(res, item, col, diag_x, diag_y, cnt - 1, i + 1);
                col[j] = true;
                diag_x[i + j] = true;
                diag_y[i - j + item.size() - 1] = true;
                item[i][j] = '.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> res;
        vector<vector<char>> item(n, vector<char>(n, '.'));
        if (n == 0) {
            return res;
        }
        //vector<bool> row(n, true);
        vector<bool> col(n, true);
        vector<bool> diag_x(2 * n - 1, true);
        vector<bool> diag_y(2 * n - 1, true);
        helper(res, item, col, diag_x, diag_y, n, 0);
        return res;
    }
};
```
#### Time Complexity

O(n);