## Description

[Add Two Numbers](https://leetcode.com/problems/add-two-numbers/): You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and **return the sum** as a linked list.

**Example:**

```
Input: l1 = [9,9], l2 = [1,9,8,9]
Output: [0,9,0,0,1]
Explanation: 99 + 9891 = 10090.
```

<img src="/Users/xiaoli/Documents/Codes/github/Leetcode-Adventure/images/list/list001.png" style="zoom: 33%;" />



## Solution

The basic solution is to use a `flag` as the carry bit, obtain the sum by adding the values of ListNode1 and ListNode2, update `flag (sum / 10)` and `value (sum % 10)` util we finish the two lists. Note if a list is finished first, we treat its value as **ZERO**.

The space cost of my solution is **O(n)** for conciseness. However, it is easy to optimize it to O(1) solution by using the longer list for updating values.



## Code

```C++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if(l1 == nullptr || l2 == nullptr)
            return (l1 == nullptr)? l2 : l1;
        
        ListNode* head = new ListNode(-1), *cur = head;
        
        int flag = 0;
        while(l1 || l2 || flag) {
            int tmp = flag;
            
            if(l1 != nullptr) {
                tmp += l1->val;
                l1 = l1->next;
            }
            if(l2 != nullptr) {
                tmp += l2->val;
                l2 = l2->next;
            }
            
            flag = tmp/10;
            cur->next = new ListNode(tmp%10);
            cur = cur->next;
        }
        
        return head->next;
    }
};
```



## Complexity

Time complexity: O(n)

Space complexity: O(n)

