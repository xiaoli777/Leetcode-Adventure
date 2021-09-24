## Description

[Shortest Distance to Target Color](https://leetcode.com/problems/shortest-distance-to-target-color/): You are given an array `colors`, in which there are three colors: `1`, `2` and `3`.

You are also given some queries. Each query consists of two integers `i` and `c`, return the shortest distance between the given index `i` and the target color `c`. If there is no solution return `-1`.

**Example:**

```
Input: colors = [1,1,2,1,3,2,2,3,3], queries = [[1,3],[2,2],[6,1]]
Output: [3,0,3]
Explanation: 
The nearest 3 from index 1 is at index 4 (3 steps away).
The nearest 2 from index 2 is at index 2 itself (0 steps away).
The nearest 1 from index 6 is at index 3 (3 steps away).
```



## Solution

The basic idea is DP (Dynamic Programming). We use `dist[i][j]` to represent the shortest distance from `COLORj` to the index `i` (e.g., `dist[1][3]` means that the shortest distance from `color3` to the index `1` ). 

Firstly, we traverse all elments **from begin to end**. If a blue color appears, we record its index `i1`. Then, if a red color indexed `i2` arrives, we can just update the distance `i2 - i1`.

If a new blue color indexed `i3` shows, we update the distance to min(`i2 - i1`, `i3 - i2`). This is because either `i1` or `i3` is the closest red color to the blue color `i2`. 

However, we still have a problem: by using this way, we can NOT get all distance (e.g., 1 1 2, we can NOT get the distance from `index1` to `color2`, because `index1` and  `index2` are same color, and `index2` will overlap `index1`. So, will still need to traverse **from end to begin**. If a distance from index to color is not update, we update it.



## Code

```cpp
class Solution {
public:
    vector<int> shortestDistanceColor(vector<int>& colors, vector<vector<int>>& queries) {
        int p1 = -1, p2 = -1, p3 = -1, n = colors.size();
        vector<vector<int>> dist(n, vector<int>(4, 1e6));
        
        for(int i = 0; i < n; i++) {
            if(colors[i] == 1)
                p1 = i;
            else if(colors[i] == 2)
                p2 = i;
            else 
                p3 = i;
            
            if(p1 != -1) {
                dist[p1][colors[i]] = min(dist[p1][colors[i]], i - p1);
                dist[i][1] = i - p1;
            }
            if(p2 != -1) {
                dist[p2][colors[i]] = min(dist[p2][colors[i]], i - p2);
                dist[i][2] = i - p2;
            }
            if(p3 != -1) {
                dist[p3][colors[i]] = min(dist[p3][colors[i]], i - p3);
                dist[i][3] = i - p3;
            }
        }
        
        for(int i = n - 1; i >= 0; i--) {
            if(colors[i] == 1) {
                if(p1 != -1) {
                    dist[i][2] = min(dist[i][2], p1 - i + dist[p1][2]);
                    dist[i][3] = min(dist[i][3], p1 - i + dist[p1][3]);
                }
                p1 = i;
            }
            else if(colors[i] == 2) {
                if(p2 != -1) {
                    dist[i][1] = min(dist[i][1], p2 - i + dist[p2][1]);
                    dist[i][3] = min(dist[i][3], p2 - i + dist[p2][3]);
                }
                p2 = i;
            } 
            else {
                if(p3 != -1) {
                    dist[i][1] = min(dist[i][1], p3 - i + dist[p3][1]);
                    dist[i][2] = min(dist[i][2], p3 - i + dist[p3][2]);
                }
                p3 = i;
            } 
        }
        
        vector<int> ans;
        for(auto &q : queries) 
            ans.push_back((dist[q[0]][q[1]] == 1e6)? -1 : dist[q[0]][q[1]]);
        return ans;
    }
};
```



## Optimized Solution

I just make the solution templated by using an array `p` to represent `p1`, `p2` and `p3`. 

So, new solution will be shorter and clearer.

Also, if the question changed, like 5 colors, new solution is earier to modify to meet the new requirement

```cpp
class Solution {
public:
    vector<int> shortestDistanceColor(vector<int>& colors, vector<vector<int>>& queries) {
        int p[4] = {-1, -1, -1, -1}, n = colors.size();
        vector<vector<int>> dist(n, vector<int>(4, 1e6));
        
        for(int i = 0; i < n; i++) {
            for(int j = 1; j <= 3; j++) {
                if(colors[i] == j)
                    p[j] = i;
                
                if(p[j] != -1) {
                    dist[p[j]][colors[i]] = min(dist[p[j]][colors[i]], i - p[j]);
                    dist[i][j] = i - p[j];
                }
            }
        }
        
        for(int i = n - 1; i >= 0; i--) {
            for(int j = 1; j <= 3; j++) {
                if(colors[i] == j && p[j] != -1) {
                    // 1 -> 2, 3 | 2 -> 1, 3 | 3 -> 1, 2
                    int d1 = colors[i]%3 + 1, d2 = (colors[i] + 1)%3 + 1;
                    dist[i][d1] = min(dist[i][d1], p[j] - i + dist[p[j]][d1]);
                    dist[i][d2] = min(dist[i][d2], p[j] - i + dist[p[j]][d2]);
                }
                p[j] = i;
            }
        }
        
        vector<int> ans;
        for(auto &q : queries) 
            ans.push_back((dist[q[0]][q[1]] == 1e6)? -1 : dist[q[0]][q[1]]);
        return ans;
    }
};
```



## Complexity

Time complexity: two for loops ===> O(n)

Space complexity: O(n)
