## Description

[Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/): Given an array of integers arr, find the sum of `min(b)`, where `b` ranges over every (contiguous) subarray of `arr`. Since the answer may be large, return the answer **modulo** `109 + 7`.

**Example:**

```
Input: arr = [3,1,2,4]
Output: 17
Explanation: 
Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Sum is 17.
```



## Solution

We use a **stack** to keep the index `i` and the `sum` after it.

For  `i`, if `arr[i] <= arr[stk.top()]` , we pop the top util the stack is **empty** or we can find a top which `arr[i] > arr[stk.top()]`. This is because if a subarry contains both `arr[i]`(smaller) and `arr[stk.top()]`(greater), arr[i] must be the minimum.

- the stack is empty: `arr[i]` is the minimum for all elements we have seen, we push it into stack
- `arr[i] > arr[stk.top()]`: `arr[i]` is the minimum between `i` and `stk.top()`, we push it into stack

**Why we use a sum with the index?** 

Because we can easily get the sum for the all element we have seen.

Finally, the result will be the total sum.

Take **[11,2,94,43,3]** as an example:

```
Index  0	1	 2	 3	 4
Arr  	 11	2	 25	 43	 3
```

- arr[i] = 3: the stack is empty, so we push the index(4) and its sum(3) into the stack

- arr[i] = 43: 43 > 3 (top), so we push the index(3) and its sum(46 = 43 + 3) into the stack

- arr[i] = 25: 25 < 43(top), so we pop the top. Then, 25 > 3(top), so we push the index(2) and its sum(53 = 25 * 2 + 3) into the stack

- arr[i] = 2: 2 < 25(top), so we pop the top. Then, 2 < 3(top), so we pop the top. Then, the stack is empty, so we push the index(1) and its sum(8 = 2 * 4) into the stack

- arr[i] = 11: 11 > 2(top), so we push the index(0) and its sum(19 = 11 + 8) into the stack

  So, **The TOTAL SUM = 3 + 46 + 53 + 8 + 19 = 129**



## Code

```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {
        int n = arr.size(), mod = 1e9 + 7;
        long sum = 0;
        stack<pair<int, long>> sums;
        for(int i = n - 1; i >= 0; i--) {
            while(!sums.empty() && arr[i] <= arr[sums.top().first]) 
                sums.pop();
            if(sums.empty()) 
                sums.push({i, (arr[i] * (n - i))%mod});
            else 
                sums.push({i, (sums.top().second + arr[i] * (sums.top().first - i))%mod});
            sum = (sum + sums.top().second)%mod;
        }
        return sum%mod;
    }
};
```



## Complexity

Time complexity: At most traversing all elements twice, so the time complexity is O(n)

Space complexity: O(n)
