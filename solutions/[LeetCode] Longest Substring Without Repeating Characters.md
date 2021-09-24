## Description

[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/): Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```



## Solution

The basic idea is to use a dictionary (called `cache`) to keep track **where the character last appeared**.

This is easy to see that **length = end - start**. For each character, we take its index as `end`. For `start`, we hope to use the index, all characters after it are not repeated. So, the `start` will be the maximum of `start` and `the index of the character last appeared`. Finally, we return the max length.

The idea is as follows:

<img src="/Users/xiaoli/Documents/Codes/github/Leetcode-Adventure/images/dp/dp001.jpg" style="zoom:60%;" />



## Code

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        vector<int> cache(256, -1);
        
        int start = -1, maxRes = 0;
        for(int i = 0; i < s.length(); i++) {
            start = max(start, cache[s[i]]);
            cache[s[i]] = i;
            maxRes = max(maxRes, i - start);
        }
        return maxRes;
    }
};
```



## Complexity

Time complexity: O(n)

Space complexity: only use 256 array space, so the space cost is O(1)
