## Description

[Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/): You are given an `m x n` integer matrix `points` (**0-indexed**). Starting with `0`points, you want to **maximize** the number of points you can get from the matrix.

To gain points, you must pick one cell in **each row**. Picking the cell at coordinates `(r, c)` will **add** `points[r][c]` to your score.

However, you will lose points if you pick a cell too far from the cell that you picked in the previous row. For every two adjacent rows `r` and `r + 1` (where `0 <= r < m - 1`), picking cells at coordinates `(r, c1)` and `(r + 1, c2)` will **subtract** `abs(c1 - c2)` from your score.

Return *the **maximum** number of points you can achieve*.

`abs(x)` is defined as:

- `x` for `x >= 0`.
- `-x` for `x < 0`.

**Example:**

```
Input: points = [[1,2,3],[1,5,1],[3,1,1]]
Output: 9
Explanation:
The blue cells denote the optimal cells to pick, which have coordinates (0, 2), (1, 1), and (2, 0).
You add 3 + 5 + 3 = 11 to your score.
However, you must subtract abs(2 - 1) + abs(1 - 0) = 2 from your score.
Your final score is 11 - 2 = 9.
```



## Solution

The basic idea is to use dp solution.

we use arrays `l2r` (left to righ) and `r2l` (right to left) to record **the maximal value** we can get in previous row. Then, we use it to add the point above it. We process all points front bottom to top.



## Code

```cpp
class Solution {
public:
    long long maxPoints(vector<vector<int>>& points) {
        int m = points.size(), n = points[0].size();
        vector<long long> dp(n, 0);
        
        long long ans = 0;
        for(int i = m - 1; i >= 0; i--) {
            vector<long long> l2r(n), r2l(n);
            l2r[0] = dp[0], r2l[n - 1] = dp[n - 1];
            for(int j = 1; j < n; j++) {
                l2r[j] = max(l2r[j - 1] - 1, dp[j]);
                r2l[n - 1 - j] = max(r2l[n - j] - 1, dp[n - 1 - j]);
            }
            for(int j = 0; j < n; j++) 
                dp[j] = points[i][j] + max(l2r[j], r2l[j]);
        }
        
        for(int j = 0; j < n; j++)
            ans = max(ans, dp[j]);
        return ans;
    }
};
```



## Optimized Space Solution

We can optimize its space by removing temporary arrays (`l2r` and `r2l`), and allow all operations in `dp` array.

```cpp
class Solution {
public:
    long long maxPoints(vector<vector<int>>& points) {
        int m = points.size(), n = points[0].size();
        vector<long long> dp(n, 0);
        for(int i = m - 1; i >= 0; i--) {
            for(int j = 1; j < n; j++) {
                dp[j] = max(dp[j - 1] - 1, dp[j]);
                dp[n - 1 - j] = max(dp[n - j] - 1, dp[n - 1 - j]);
            }
            for(int j = 0; j < n; j++) 
                dp[j] += points[i][j];
        }
        long long ans = 0;
        for(int j = 0; j < n; j++)
            ans = max(ans, dp[j]);
        return ans;
    }
};
```



## Complexity

Time complexity: O(m * n)

Space complexity: O(n)

