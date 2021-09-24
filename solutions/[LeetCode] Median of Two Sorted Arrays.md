## Description

[Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/): Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

**Example:**

```
Input: nums1 = [1,2,3,4,5], nums2 = [2,3,7]
Output: 3.00000
Explanation: merged array = [1,2,2,3,3,4,5,7] and median is (3 + 3) / 2 = 3.
```



## Solution

Median is **the middle number in a sorted list of numbers**, which makes the number of the first half euqal to the the second half.

For one sorted array `A` (shown as follows), if the position `i` is the median, **left_length = right_length** and **max(left element) <= min(right element)**.

<img src="/Users/xiaoli/Documents/Codes/github/Leetcode-Adventure/images/bs/bs001.png" style="zoom:33%;" />

Even if we divide `A` into two arrays `B` and `C` (shown as follows), we still have these two attributes. Suppose the length of `B` and `C` are `m` and `n`, the median position is `i` and `j` (we disscuss how to get `j` later). So, it is easy to kown, 

1. **B_left_length + C_left_length = B_right_length + C_right_length**
2. **max(left element) <= min(right element)**

<img src="/Users/xiaoli/Documents/Codes/github/Leetcode-Adventure/images/bs/bs002.png" style="zoom:33%;" />

According to the 1st feature, we can get **i + j = (m - i) + (n - j)**, so `j = (m + n) / 2 - i`. Also, there is a trick, if we set **n >= m**, `j` will be **[0, n]**.

So, the only thing for us is to satify the 2nd feature, in other words, `max(B[i - 1], C[j - 1]) <= max(B[i], C[j])`. It is easy too see that `B[i - 1] <= B[i]` and `C[j - 1] <= C[j]` (in two sorted arrays). Thus, we only consider three situations:

1. **B[i - 1] <= C[j]** and **C[j - 1] <= B[i]**: this `i` meets two features, so we return the result
2. **B[i - 1] > C[j]**: `i` is too large, we need to decrease it 
3. **C[j - 1] > B[i]**: `i` is too small, we need to increase it 

Notice that for the edge situations (**i = 0**, **i = m**, **j = 0** and **j = m**), `B[i - 1]`, `B[i]`, `C[j - 1]` and `C[j]` do **NOT** exist. We just set them to **maxima** or **minima**.

Finally, for the return of result, 

- **m + n** is odd, we return `min(B[i], C[j])`
- **m + n** is even, we return `(max(B[i - 1], C[j - 1]) + min(B[i], C[j])) / 2`



## Code

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        if(m > n)   return findMedianSortedArrays(nums2, nums1);
        
        int low = 0, high = m;
        while(left <= right) {
            int i = low + (high - low) / 2;
            int j = (m + n) / 2 - i;
            
            int left1  = (i == 0)? INT_MIN : nums1[i - 1];
            int right1 = (i == m)? INT_MAX : nums1[i];
            int left2  = (j == 0)? INT_MIN : nums2[j - 1];
            int right2 = (j == n)? INT_MAX : nums2[j];
            
            if(left1 > right2) {
                high = i - 1;	// decrease i
            }
            else if(left2 > right1) {
                low = i + 1;	// increase i
            }
            else {
                if((m + n) & 1)
                    return (i == m)? (double)right2 : (j == n)? (double)right1 : (double)min(right1, right2);
                
                double l = (i == 0)? (double)left2 : (j == 0)? double(left1) : (double)max(left1, left2);
                double r = (i == m)? (double)right2 : (j == n)? (double)right1 : (double)min(right1, right2);
                return (l + r) / 2;
            }
        }
        
        return double(-1);
    }
};
```



## Complexity

Time complexity: O(log(min(m,n))) for binary search

Space complexity: O(1)
