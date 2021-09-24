## Description

[Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair/): You are given an integer array `values` where values[i] represents the value of the `ith` sightseeing spot. Two sightseeing spots `i` and `j` have a **distance** `j - i` between them.

The score of a pair (`i < j`) of sightseeing spots is `values[i] + values[j] + i - j`: the sum of the values of the sightseeing spots, minus the distance between them.

Return *the maximum score of a pair of sightseeing spots*.

**Example:**

```
Input: values = [8,1,5,2,6]
Output: 11
Explanation: i = 0, j = 2, values[i] + values[j] + i - j = 8 + 5 + 0 - 2 = 11
```



## Solution

The basic way is to use mathematics.

According to the description: The score of a pair (`i < j`) of sightseeing spots is `values[i] + values[j] + i - j`, and our goal it to maximize the score.

Then, we modify this formula: `values[i] + values[j] + i - j` = `(values[j] - j) + (values[i] + i) `.

So, the only thing for us is to find an anwser: `(values[j] - j) + max(values[i] + i) `, where `i < j`.



## Code

```cpp
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& values) {
        int n = values.size(), prev = values[0] + 0, ans = 0;
        for(int i = 1; i < n; i++) {
            ans = max(ans, prev + values[i] - i);
            prev = max(prev, values[i] + i);
        }
        return ans;
    }
};
```



## Complexity

Time complexity: O(n)

Space complexity: O(1)

