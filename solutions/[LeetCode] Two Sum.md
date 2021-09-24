## Description

[Two Sum](https://leetcode.com/problems/two-sum/): Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

**Example:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```



## Solution

if `z = x + y`, we can get `y = z - x`.

So, the basic idea is to use a **Hash Table** to maintain each element and its index (0-indexed). For each element, we search if it is already in the hash table. If NO, we add `target - num` as a key and `its index` as a value into the hash map. Otherwise, we get the answer, because the index of `target - num` is already stored in the hash.



## Code

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> cache;
        for(int i = 0; i < nums.size(); i++) {
            if(cache.find(nums[i]) != cache.end()) 
                return {cache[nums[i]], i};
            cache[target - nums[i]] = i;
        }
        return {-1, -1};
    }
};
```



## Complexity

Time complexity: O(n)

Space complexity: O(n)
