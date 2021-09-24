## Description

[Add Strings](https://leetcode.com/problems/add-strings/): Given two non-negative integers, `num1` and `num2` represented as string, return *the sum of* `num1` *and* `num2` *as a string*.

**Example:**

```
Input: num1 = "456", num2 = "77"
Output: "533"
```



## Solution

The basic idea is to add the numbers one by one from tail to head.



## Code

```cpp
class Solution {
public:
    string addStrings(string num1, string num2) {
        int l1 = num1.length(), l2 = num2.length(), flag = 0;
        string res = "";
        while(l1 + l2 + flag) {
            int sum = 0;
            if(l1 == 0 && l2 == 0)
                sum = sum + flag;
            else if(l1 == 0) 
                sum = sum + int(num2[--l2] - '0') + flag;
            else if(l2 == 0) 
                sum = sum + int(num1[--l1] - '0') + flag;
            else 
                sum = sum + int(num1[--l1] - '0') + int(num2[--l2] - '0') + flag;
            
            flag = sum/10;
            res = char(sum%10 + '0') + res;
        }
        return res;
    }
};
```



## Complexity

Time complexity: O(max(m,n))

Space complexity: O(max(m,n))
