## Count All Valid Pickup and Delivery OptionsCount All Valid Pickup and Delivery OptionsDescription

[Valid Sudoku](https://leetcode.com/problems/valid-sudoku/): Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Example:**

```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
```



## Solution

The basic idea is to verify if each row, each column, and each sub-box is valid.

We use the bits of a binary number to record the information of rows, columns, and sub-boxes. If a digits is appeared, we the the corresponding bit to `1`. However, if a bit is already `1`, we will kown that this row \ column \ sub-box is invalid.

So, the only thing is how to define different rows, columns, and sub-boxes. I use a diagram to illustrate my idea.

<img src="/Users/xiaoli/Documents/Codes/github/Leetcode-Adventure/images/Sudoku.png" style="zoom: 33%;" />

## Code

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<int> row(9, 0), col(9, 0), sub(9, 0);
        
        for(int i = 0; i < 9; i++) {
            for(int j = 0; j < 9; j++) {
                if(board[i][j] == '.')
                    continue;
                
                int num = board[i][j] - '0';
                if((row[i] & (1 << num)) || (col[j] & (1 << num)) || (sub[i/3 * 3 + j / 3] & (1 << num)))
                    return false;
                
                row[i] |= (1 << num);
                col[j] |= (1 << num);
                sub[i/3 * 3 + j / 3] |= (1 << num);
            }
        }
        return true;
    }
};
```



## Complexity

Time complexity: O(1)

Space complexity: O(1)
