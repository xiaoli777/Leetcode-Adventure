## Description

[Longest Line of Consecutive One in Matrix](https://leetcode.com/problems/zigzag-conversion/): Given an `m x n` binary matrix `mat`, return *the length of the longest line of consecutive one in the matrix*.

The line could be horizontal, vertical, diagonal, or anti-diagonal.

**Example:**

```
Input: mat = [[0,1,1,0],[0,1,1,0],[0,0,0,1]]
Output: 3
```



## Solution

The basic idea is DP (Dynamic Programming), and we use 4 different number to record different types of consecutive ones.

`dp0`: the vertical consecutive ones

`dp1`: the horizontal consecutive ones

`dp2`: the diagonal consecutive ones

`dp3`: the anti-diagonal consecutive ones

Then, the only thing for us is to find the longest consecutive ones.



## Code

```cpp
class Solution {
public:
    int longestLine(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size(), ans = 0;
        vector<vector<vector<int>>> dp(m + 2, vector<vector<int>>(n + 2, vector<int>(4, 0)));
        for(int i = 1; i <= m; i++) {
            for(int j = 1; j <= n; j++) {
                if(mat[i - 1][j - 1]) {
                    dp[i][j][0] = dp[i - 1][j][0] + 1;
                    dp[i][j][1] = dp[i][j - 1][1] + 1;
                    dp[i][j][2] = dp[i - 1][j - 1][2] + 1;
                    dp[i][j][3] = dp[i - 1][j + 1][3] + 1;
                    ans = max({ans, dp[i][j][0], dp[i][j][1], dp[i][j][2], dp[i][j][3]});
                }
            }
        }
        return ans;
    }
};
```



## Optimized Space Solution

```cpp
class Solution {
public:
    int longestLine(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        vector<vector<int>> dp(4, vector<int>(n + 2, 0));
        int ans = 0;
        for(int i = 1; i <= m; i++) {
            fill(dp[1].begin(), dp[1].end(), 0);
            int prev = dp[2][0];
            for(int j = 1; j <= n; j++) {
                int tmp = dp[2][j];
                if(mat[i - 1][j - 1] == 0) {
                    dp[0][j] = dp[1][j] = dp[2][j] = dp[3][j] = 0;
                }
                else {
                    dp[0][j] = dp[0][j] + 1;
                    dp[1][j] = dp[1][j - 1] + 1;
                    dp[2][j] = prev + 1;
                    dp[3][j] = dp[3][j + 1] + 1;
                    ans = max({ans, dp[0][j], dp[1][j], dp[2][j], dp[3][j]});
                }
                prev = tmp;
            }
        }
        return ans;
    }
};
```



## Complexity

Time complexity: O(m * n)

Space complexity: O(m * n) -> O(n)

