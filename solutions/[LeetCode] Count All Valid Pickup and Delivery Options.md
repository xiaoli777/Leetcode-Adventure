## Description

[Count All Valid Pickup and Delivery Options](https://leetcode.com/problems/count-all-valid-pickup-and-delivery-options/): Given `n` orders, each order consist in pickup and delivery services. 

Count all valid pickup/delivery possible sequences such that delivery(i) is always after of pickup(i). 

Since the answer may be too large, return it modulo 10^9 + 7.

**Example:**

```
Input: n = 2
Output: 6
Explanation: All possible orders: 
(P1,P2,D1,D2), (P1,P2,D2,D1), (P1,D1,P2,D2), (P2,P1,D1,D2), (P2,P1,D2,D1) and (P2,D2,P1,D1).
This is an invalid order (P1,D2,P2,D1) because Pickup 2 is after of Delivery 2.
```



## Solution

This idea is based on DP and slot thinking.

If we have `(i-1)th` valid orders, how to make `ith` valid orders. Clearly, for a `(i-1)th` valid order, it contains `2 * i - 1` slots from top to end.

So, we set `Pi` to one of the slot, and set `Di` after it. Because the `(i-1)th` order is valid, the `ith` order is definitely valid after adding `Pi` and `Di`. In this way, there are totally ` (slot + slot - 1 + ... + 1) = (slot * (slot + 1)) / 2` valid `ith` orders.

Finally, for `k`  `(i-1)th` valid orders, we totally have `k * (slot * (slot + 1)) / 2` `ith` orders.

Take `n = 3` as an example:

`n = 2`, we have 6 possible orders. Just pick an n = 2 order, like `P1D1P2D2`, which has 5 slots ( `_P1_D1_P2_D2_`, `_` represents a slot).

We pick one of these slots to set `P3`, then we can set `D3` to all of slots after the slot we picked:

```
P3_P1_D1_P2_D2_
P1P3_D1_P2_D2_
P1D1P3_P2_D2_
P1D1P2P3_D2_
P1D1P2D2P3_
```

As shown above, we can insert `D3` into all of `_` slot. There are 15 (5 + 4 + 3 + 2 + 1 = 5 *6 / 2 = 15) possible orders.

So the total order = 6 * 15 = 90.



## Code

```cpp
class Solution {
public:
    int countOrders(int n) {
        long ans = 1, mod = 1e9 + 7;
        for(int i = 1, slot = 3; i < n; i++, slot += 2)
            ans = (ans * (slot * (slot + 1)) / 2)%mod;
        return ans;
    }
};
```



## Complexity

Time complexity: One loop, O(n)

Space complexity: O(1)
