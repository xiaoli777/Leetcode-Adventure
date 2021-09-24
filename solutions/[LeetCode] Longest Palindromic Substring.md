## Description

[Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/): Given a string `s`, return *the longest palindromic substring* in `s`.

**Example:**

```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```



## Solution

The basic idea for this question is to use dynamic programming. This is because **whether a string is a palindrome** can be transfered from **whether its substring is a palindrome**. For example, if a string `abcba` is palindrome, its substring `bcb` also is a palindrome. But if a substring `bcb` is a palindrome, a string `abcbd` may not be a palindrome.

Moreover, we notice that **the odd length string** can be transfered from **its previous odd length substring**, and **the even length string** can be transfered from **its previous even length substring**. For example, `abcba` which length is 5 can be transfered from `bcb` which length is 3. Also, `abccba` which length is 6 can be transfered from `bccb` which length is 4.

Accroding to these two analyses, we can get the transfer equation: `dp[i][j] = dp[i + 1][j - 1] + 2` if `dp[i + 1][j - 1]` is a palindrome and `s[i] == s[j]`. (`i` and `j` are indexes of the string `s`)

Finally, the last thing we should do is to record the `start` and `end` of longest palindromic substring for return.



## Code

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int len = s.length();
        int start = 0, end = 0;
        
        vector<vector<int>> dp(2, vector(len, 0));
        dp[0][0] = 1;
        for(int i = 1; i < len; i++) {
            dp[1 & 1][i] = 1;
            if(s[i] == s[i - 1]) {
                dp[2 & 1][i] = 2;
                start = i - 1;
                end = i;
            }
        }
        
        for(int curLen = 3; curLen <= len; curLen++) {
            for(int i = len - 1; i >= curLen - 1; i--) {
                if((dp[curLen & 1][i - 1] == curLen - 2) && s[i - curLen + 1] == s[i]) {
                    dp[curLen & 1][i] = curLen;
                    start = i - curLen + 1;
                    end = i;
                }
            }
        }
        return s.substr(start, end - start + 1);
    }
};
```



## Complexity

Time complexity: O(n ^ 2)

Space complexity: O(n) for dynamic programming
