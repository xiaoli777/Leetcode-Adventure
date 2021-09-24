## Description

[Jump Game II](https://leetcode.com/problems/jump-game-ii/): Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.

**Example:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```



## Solution

The basic idea is DP solution. 

We use 2 variables to keep track of moving: 

`prevDist`: record the furthest postion we can reach

`curDist`: record the next furthest postion we can reach based on the current postion

If `prevDist < i`, which means current position is **unreachable**. Then, we just update `prevDist = curDist`, in other words, we jump to the postion which help us move furthest. We keep moving util `prevDist < n - 1`.



## Code

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        if(n == 1)  return 0;
        
        int prevDist = -1, curDist = nums[0] + 0, count = 0;
        for(int i = 0; i < n; i++) {
            if(i > prevDist) {
                prevDist = curDist;
                ++count;
            }
            
            if(prevDist >= n - 1)
                return count;
            
            int newPos = i + nums[i];
            if(newPos > curDist)
                curDist = newPos;
        }
        return count;
    }
};
```



## Complexity

Time complexity: One pass, O(n)

Space complexity: Only some variables, O(1)

