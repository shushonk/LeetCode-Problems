# LeetCode-Problems

# 3257. Maximum Value Sum by Placing Three Rooks II

[LeetCode Problem Link](https://leetcode.com/problems/maximum-value-sum-by-placing-three-rooks-ii/)

---

## Problem Statement

You are given an `m x n` 2D array `board` representing a chessboard, where `board[i][j]` is the value of the cell `(i, j)`.

Rooks in the same row or column attack each other. You need to place three rooks on the chessboard such that the rooks do not attack each other.

Return the **maximum sum** of the cell values on which the rooks are placed.

---

## Examples

**Example 1:**

```

Input: board = [[-3,1,1,1],[-3,1,-3,1],[-3,2,1,1]]
Output: 4


Explanation:
We can place the rooks at cells (0, 2), (1, 3), and (2, 1) for a sum of 1 + 1 + 2 = 4.

```
<img width="322" height="496" alt="image" src="https://github.com/user-attachments/assets/eb4f5276-da9b-491e-8752-b0f508a0fe82" />

```
```
**Example 2:**

```

Input: board = [[1,2,3],[4,5,6],[7,8,9]]
Output: 15

Explanation:
We can place the rooks at cells (0, 0), (1, 1), and (2, 2) for a sum of 1 + 5 + 9 = 15.

```

**Example 3:**

```

Input: board = [[1,1,1],[1,1,1],[1,1,1]]
Output: 3

Explanation:
We can place the rooks at cells (0, 2), (1, 1), and (2, 0) for a sum of 1 + 1 + 1 = 3.

````

---

## Constraints

- `3 <= m == board.length <= 500`
- `3 <= n == board[i].length <= 500`
- `-10^9 <= board[i][j] <= 10^9`

---

## Approach

1. Precompute maximum values per column from top to current row (`tb`) and from bottom to current row (`bt`).
2. For each row `i` (excluding first and last), get the top 3 max values from:
   - `tb[i - 1]` (rows above),
   - `board[i]` (current row),
   - `bt[i + 1]` (rows below).
3. Try all combinations of these top 3 candidates, ensuring no two rooks share the same column.
4. Track the maximum sum found.

---

## Solution Code (Python)

```python
class Solution:
    def maximumValueSum(self, board):
        n = len(board)
        m = len(board[0])
        
        # tb[i][j] = max of board[x][j] for x in [0..i]
        tb = [[0] * m for _ in range(n)]
        tb[0] = board[0][:]
        for i in range(1, n):
            for j in range(m):
                tb[i][j] = max(tb[i - 1][j], board[i][j])
        
        # bt[i][j] = max of board[x][j] for x in [i..n-1]
        bt = [[0] * m for _ in range(n)]
        bt[n - 1] = board[n - 1][:]
        for i in range(n - 2, -1, -1):
            for j in range(m):
                bt[i][j] = max(bt[i + 1][j], board[i][j])
        
        ans = float('-inf')
        
        for i in range(1, n - 1):
            max3Top = self.getMax3(tb[i - 1])
            max3Cur = self.getMax3(board[i])
            max3Bottom = self.getMax3(bt[i + 1])
            
            for topVal, topIdx in max3Top:
                for curVal, curIdx in max3Cur:
                    for botVal, botIdx in max3Bottom:
                        if topIdx != curIdx and topIdx != botIdx and curIdx != botIdx:
                            cand = topVal + curVal + botVal
                            ans = max(ans, cand)
        
        return ans
    
    def getMax3(self, row):
        # Returns top 3 pairs (value, index) from the row
        ans = [[float('-inf'), -1] for _ in range(3)]
        for j, val in enumerate(row):
            if val >= ans[0][0]:
                ans[2] = ans[1]
                ans[1] = ans[0]
                ans[0] = [val, j]
            elif val >= ans[1][0]:
                ans[2] = ans[1]
                ans[1] = [val, j]
            elif val > ans[2][0]:
                ans[2] = [val, j]
        return ans


````

---

## Complexity

* **Time Complexity:** O(n * m)
* **Space Complexity:** O(n * m)

---

## References

* [LeetCode 3257 Problem](https://leetcode.com/problems/maximum-value-sum-by-placing-three-rooks-ii/)





Would you like me to help with setting up the GitHub repo or LeetCode Discuss post next?
```
