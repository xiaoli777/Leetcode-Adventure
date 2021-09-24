## Description

[ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/): The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows.

**Example:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
```



## Solution

The basic idea is to find the pattern. I find a pattern as follows:

<img src="/Users/xiaoli/Documents/Codes/github/Leetcode-Adventure/images/string/s001.png" style="zoom:50%;" />



## Code

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows == 1)    return s;
        
        int len = s.length();
        string res = "";
        
        int tmp = 0;
        int circle = numRows - 1, inc = 2 * circle;
        while(tmp < len) {
            res += s[tmp];
            tmp += inc;
        }
        
        for(int i = 1; i < circle; i++) {
            tmp = i;
            int sum = inc;
            while(tmp < len) {
                res += s[tmp];
                if(sum - tmp < len)
                    res += s[sum - tmp];
                tmp += inc;
                sum += 2 * inc;
            }
        }
        
        tmp = circle;
        while(tmp < len) {
            res += s[tmp];
            tmp += inc;
        }
        
        return res;
    }
};
```



## Complexity

Time complexity: O(n)

Space complexity: O(n)

