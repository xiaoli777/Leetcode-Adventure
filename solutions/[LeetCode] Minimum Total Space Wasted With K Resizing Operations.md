## Description

[Minimum Total Space Wasted With K Resizing Operations](https://leetcode.com/problems/minimum-total-space-wasted-with-k-resizing-operations/): You are currently designing a dynamic array. You are given a **0-indexed** integer array `nums`, where `nums[i]` is the number of elements that will be in the array at time `i`. In addition, you are given an integer `k`, the **maximum** number of times you can **resize** the array (to **any** size).

The size of the array at time `t`, `sizet`, must be at least `nums[t]` because there needs to be enough space in the array to hold all the elements. The **space wasted**at time `t` is defined as `sizet - nums[t]`, and the **total** space wasted is the **sum**of the space wasted across every time `t` where `0 <= t < nums.length`.

Return *the **minimum** **total space wasted** if you can resize the array at most* `k`*times*.

**Note:** The array can have **any size** at the start and does **not** count towards the number of resizing operations.

**Example:**

```
Input: nums = [10,20,15,30,20], k = 2
Output: 15
Explanation: size = [10,20,20,30,30].
We can set the initial size to 10, resize to 20 at time 1, and resize to 30 at time 3.
The total wasted space is (10 - 10) + (20 - 20) + (20 - 15) + (30 - 30) + (30 - 20) = 15.
```



## Solution

The basic idea is DP(Dynamic Programming): 

**The elements before index `i`** don't change, which makes the cost `c1` of the first part (0 1 ... i - 2).

The `kth` resize happens among **the `ith` elements** of the array, which makes the cost `c2` of the second part (i - 1).

And, `k - 1` times happend among **the rest elements** of the array, whcih makes the cost `c3` of the third part (i i + 1 ... n - 1).

So, the total space cost `c = c1 + c2 + c3`.

Notice that for the answer, the first cost `c1 = 0`.

For example, array = [10,20,15,30,20], k = 2;

```
index		0		1		2		3		4		5

array		10	20	15	30	20

dp

k = 0		55	35	25	10	0		0

k = 1		25	15	10	0		0		0

k = 2		15	/		/		/		/		/
```



## Code

```cpp
class Solution {
public:
    int minSpaceWastedKResizing(vector<int>& nums, int k) {
        int n = nums.size();
        if(k >= n - 1)  return 0;
        
        vector<vector<int>> dp(k + 1, vector(n + 1, 0));
        for(int i = 0; i < n; i++) {
            int maxima = nums[i], sum = 0, ans = INT_MAX;
            for(int j = i; j < n; j++) {
                maxima = max(maxima, nums[j]);
                sum += nums[j];
            }
            dp[0][i] = maxima * (n - i) - sum;
        }
        if(k == 0)  return dp[0][0];
        
        for(int time = 1; time < k; time++) {
            for(int i = 0; i < n; i++) {
                int maxima = nums[i], sum = 0, ans = INT_MAX;
                for(int j = i; j < n; j++) {
                    maxima = max(maxima, nums[j]);
                    sum += nums[j];
                    int waste = maxima * (j - i + 1) - sum;
                    ans = min(ans, dp[time - 1][j + 1] + waste);
                }
                dp[time][i] = ans;
            }
        }
        
        int maxima = nums[0], sum = 0, ans = INT_MAX;
        for(int i = 0; i < n; i++) {
            maxima = max(maxima, nums[i]);
            sum += nums[i];
            int waste = maxima * (i + 1) - sum;
            ans = min(ans, dp[k - 1][i + 1] + waste);
        }
        return ans;
    }
};
```



## Complexity

Time complexity: `n ^ 2` for DP updation, and `k` for resizing. So, the total time cost is O(k * (n ^ 2))

Space complexity: O(k * (n ^ 2))



## Optimization

This is a concise solution, and space complexity is O(k * n).

```cpp
class Solution {
public:
    int minSpaceWastedKResizing(vector<int>& nums, int k) {
        int n = nums.size();
        if(k >= n - 1)  return 0;
        
        vector<int> dp(n + 1, 1e9);
        dp[n] = 0;
        
        for(int time = 0; time <= k; time++) {
            for(int i = 0; i < n; i++) {
                int maxima = nums[i], sum = 0, ans = 1e9;
                for(int j = i; j < n; j++) {
                    maxima = max(maxima, nums[j]);
                    sum += nums[j];
                    int waste = maxima * (j - i + 1) - sum;
                    ans = min(ans, dp[j + 1] + waste);
                }
                dp[i] = ans;
                if(time == k)   break;
            }
        }
        return dp[0];
    }

};
```

