## Description

[K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/): Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y**plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2`).

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example:**

```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```



## Solution

We do NOT need to sort all elements in the array, because we only need to get the k closet points.

So, we can solve it by using a **customized quick sorting**: the goal for us is to find a postion `p`. Any element **before** `p` is **closer** than `p` to the origin `(0, 0)`, and any element **after** `p` is **more distant** than `p` to the origin `(0, 0)`.

Please upvote if you think it is helpful. ^ _ ^.



## Code

```cpp
class Solution {
public:
    int partition(vector<vector<int>>& points, int l, int r) {
        int tmp = l, tmp_val = points[tmp][0] * points[tmp][0] + points[tmp][1] * points[tmp][1];
        l++;
        while(true) {
            while(l <= r && points[l][0] * points[l][0] + points[l][1] * points[l][1] <= tmp_val)   l++;
            while(l <= r && points[r][0] * points[r][0] + points[r][1] * points[r][1] >= tmp_val)   r--;
            if(l > r)
                break;
            swap(points[l], points[r]);
        }
        swap(points[tmp], points[r]);
        return r;
    }
    
    int qsort(vector<vector<int>>& points, int start, int end, int k) {
        if(start >= end)    return start;
        
        int p = partition(points, start, end);
        if(p == k)  return k;
        return (p > k)? qsort(points, start, p - 1, k) : qsort(points, p + 1, end, k);
    }
    
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        int p = qsort(points, 0, points.size() - 1, k - 1);
        return vector<vector<int>>(points.begin(), points.begin() + p + 1);
    }
};
```



## Complexity

Time complexity: O(nlogn)

Space complexity: O(1), we only use 1 stack space.

