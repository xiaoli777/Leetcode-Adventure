## Description

[Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/): Given two strings `s1` and `s2`, return *the lowest **ASCII** sum of deleted characters to make two strings equal*.

**Example:**

```
Input: s1 = "sea", s2 = "eat"
Output: 231
Explanation: Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum.
Deleting "t" from "eat" adds 116 to the sum.
At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.
```



## Solution

The basic idea is to use DP: `dp[i][j]` means the lowest ASCII sum between `string1` (indices from `0` to `i - 1`) and  `string2` (indices from `0` to `j - 1`).

The transfer equation is: 

1. if `string1[i - 1] == string2[j - 1]`, then we can get `dp[i][j] = dp[i - 1][j - 1]`
2. if `string1[i - 1] != string2[j - 1]`, then we can get `dp[i][j] = min(dp[i][j - 1] + string2[j - 1], dp[i - 1][j] + string1[i - 1])`

e.g., sea eat

            0                       1                       2                       3
        0   0                       s                       se                      sea
        1   e                       min(s + e, e + s)       s                       min(s + a, sea + e)
        2   ea                      min(se + a, ea + s)     min(se + ea, ea + se)   s
        3   eat                     min(sea + t, eat + s)   min(sea + te, eat + se) min(eat + sea, s + t)

Notice that we can use type conversion to convert `char` to `int`.



## Code

```cpp
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int l1 = s1.length(), l2 = s2.length();
        vector<vector<int>> dp(l1 + 1, vector<int>(l2 + 1, 0));
        for(int i = 1; i <= l1; i++) 
            dp[i][0] = dp[i - 1][0] + int(s1[i - 1]);
        for(int j = 1; j <= l2; j++) 
            dp[0][j] = dp[0][j - 1] + int(s2[j - 1]);
        for(int i = 1; i <= l1; i++) {
            for(int j = 1; j <= l2; j++) {
                if(s1[i - 1] == s2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1];
                else
                    dp[i][j] = min(dp[i - 1][j] + int(s1[i - 1]), dp[i][j - 1] + int(s2[j - 1]));
            }
        }
        return dp[l1][l2];
    }
};
```



## Optimized Space Solution

```cpp
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int l1 = s1.length(), l2 = s2.length();
        vector<int> dp(l2 + 1, 0);
        for(int j = 1; j <= l2; j++) 
            dp[j] = dp[j - 1] + int(s2[j - 1]);
        for(int i = 1; i <= l1; i++) {
            int prev = dp[0];
            dp[0] += int(s1[i - 1]);
            for(int j = 1; j <= l2; j++) {
                int tmp = dp[j];
                dp[j] = ((s1[i - 1] == s2[j - 1])? prev : min(dp[j] + int(s1[i - 1]), dp[j - 1] + int(s2[j - 1])));
                prev = tmp;
            }
        }
        return dp[l2];
    }
};
```



## Complexity

Time complexity: O(m ^ n)

Space complexity: O(m ^ n) -> O(n)

