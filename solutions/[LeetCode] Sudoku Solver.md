## Description

[Sudoku Solver](https://leetcode.com/problems/sudoku-solver/): Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

The `'.'` character indicates empty cells.

**Example:**

```
Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
```



## Solution

You can do [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/) at first, then you may get a idea.

The basic idea is **backtracking**. We traverse all empty cells one by one, and check it if it satisfies **all rules** in its row, column, and sub-box. 

If this digit meets all rules, we set new rules to its row, column, and sub-box. Then, we go to the next empty cell. 

If this digit can't meets any rule, we pick another digit to check. However, if all digits (1 - 9) can't meets the rules, this means this solution is invalid. Then, we stop this way, and go back to previous cell for checking.



## Code

```cpp
class Solution {
public:
    bool solver(vector<vector<char>>& board, vector<int>& rows, vector<int>& cols, vector<int> & subs, int i, int j) {
        if(i == 9)  return true;
        if(j == 9)  return solver(board, rows, cols, subs, i + 1, 0);
        if(board[i][j] != '.')  return solver(board, rows, cols, subs, i, j + 1);
        
        for(int x = 1; x <= 9; x++) {
            if((rows[i] & (1 << x)) || (cols[j] & (1 << x)) || (subs[i/3 * 3 + j/3] & (1 << x)))
                continue;
            
            rows[i] ^= (1 << x);
            cols[j] ^= (1 << x);
            subs[i/3 * 3 + j/3] ^= (1 << x);
            
            board[i][j] = x + '0';
            if(solver(board, rows, cols, subs, i, j + 1))
                return true;
            
            rows[i] ^= (1 << x);
            cols[j] ^= (1 << x);
            subs[i/3 * 3 + j/3] ^= (1 << x);
            
        }
        board[i][j] = '.';
        return false;
    }
    
    void solveSudoku(vector<vector<char>>& board) {
        vector<int> rows(9, 0), cols(9, 0), subs(9, 0);
        for(int i = 0; i < 9; i++) {
            for(int j = 0; j < 9; j++) {
                if(board[i][j] != '.') {
                    int bit = board[i][j] - '0';
                    rows[i] |= (1 << bit);
                    cols[j] |= (1 << bit);
                    subs[i/3 * 3 + j/3] |= (1 << bit);
                }
            }
        }
        solver(board, rows, cols, subs, 0, 0);
    }
};
```



## Complexity

Time complexity:  the worst is 81 ^ 9, but it's stll O(1)

Space complexity: the solution only contains constants, so the space complexity is O(1)
