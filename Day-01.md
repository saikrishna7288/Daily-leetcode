# Daily LeetCode Log — October 11, 2026

Today's practice focused on **Dynamic Programming (DP)** using recursion with memoization to avoid repeated calculations.

## Problems Solved

1. [LeetCode 62 — Unique Paths](https://leetcode.com/problems/unique-paths/)
2. [LeetCode 64 — Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

---

## 1. LeetCode 62 — Unique Paths

**Topic:** Dynamic Programming, Recursion, Memoization  
**Difficulty:** Medium

### Problem Description

Given an `m x n` grid, a robot starts at the top-left cell and wants to reach the bottom-right cell. The robot can move only **right** or **down**. Return the number of possible unique paths.

### Approach

Use recursion with memoization to count the paths from each cell to the destination.

**Steps**
1. Start from cell `(0, 0)` and set the destination to `(m - 1, n - 1)`.
2. If the current row or column goes outside the grid, return `0` because that path is invalid.
3. If the current cell is the destination, return `1` because one valid path has been found.
4. If the result for the current cell is already stored in `dp`, return it instead of recalculating.
5. Recursively count paths by moving **right** and **down**.
6. Add both results, store the sum in `dp[cx][cy]`, and return it.

### Code (Java)

```java
class Solution {
    int dp[][];

    public int find(int cx, int cy, int ex, int ey) {
        if (cx > ex || cy > ey) return 0;
        if (cx == ex && cy == ey) return 1;

        if (dp[cx][cy] != -1) return dp[cx][cy];

        int right = find(cx, cy + 1, ex, ey);
        int down = find(cx + 1, cy, ex, ey);

        return dp[cx][cy] = right + down;
    }

    public int uniquePaths(int m, int n) {
        int sx = 0, sy = 0, ex = m - 1, ey = n - 1;

        dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(dp[i], -1);
        }

        return find(sx, sy, ex, ey);
    }
}
```

### Complexity Analysis

- **Time:** `O(m × n)` — each cell's result is computed at most once.
- **Space:** `O(m × n)` for the memoization table, plus recursion stack space.

---

## 2. LeetCode 64 — Minimum Path Sum

**Topic:** Dynamic Programming, Recursion, Memoization  
**Difficulty:** Medium

### Problem Description

Given an `m x n` grid containing non-negative integers, find a path from the top-left cell to the bottom-right cell that has the **minimum sum**. You can move only **right** or **down**.

### Approach

Use recursion with memoization to find the minimum cost from each cell to the destination.

**Steps**
1. Start at `(0, 0)` and set the destination to `(m - 1, n - 1)`.
2. If the current position is outside the grid, return `Integer.MAX_VALUE` to mark that move as invalid.
3. If the current cell is the destination, return its grid value.
4. If the answer for the current cell is already in `dp`, return the cached result.
5. Recursively calculate the minimum path cost by moving **right** and **down**.
6. Choose the smaller of those two costs, add the current cell's value, store the result in `dp[i][j]`, and return it.

### Code (Java)

```java
class Solution {
    int dp[][];

    public int find(int i, int j, int m, int n, int grid[][]) {
        if (i > m || j > n) return Integer.MAX_VALUE;
        if (i == m && j == n) return grid[i][j];

        if (dp[i][j] != -1) return dp[i][j];

        int right = find(i, j + 1, m, n, grid);
        int down = find(i + 1, j, m, n, grid);

        return dp[i][j] = grid[i][j] + Math.min(right, down);
    }

    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;

        dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(dp[i], -1);
        }

        return find(0, 0, m - 1, n - 1, grid);
    }
}
```

### Complexity Analysis

- **Time:** `O(m × n)` — memoization ensures each cell's result is computed at most once.
- **Space:** `O(m × n)` for the memoization table, plus recursion stack space.

---

## Key Takeaway

Both problems use the same core pattern: **define a state, handle base cases, explore right/down choices, and memoize the result**.

- **Unique Paths:** Add the number of valid paths from the right and down cells.
- **Minimum Path Sum:** Add the current cell's value to the smaller of the right and down path costs.

> Java note: Add `import java.util.Arrays;` (or `import java.util.*;`) at the top if compiling these snippets outside LeetCode.
