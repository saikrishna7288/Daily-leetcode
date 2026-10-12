Daily LeetCode Log — October 12, 2026
Today's practice focused on Dynamic Programming (DP) with recursion and memoization.
Problems Solved
1. LeetCode 63 — Unique Paths II
2. LeetCode 931 — Minimum Falling Path Sum
1. LeetCode 63 — Unique Paths II
Topic: Dynamic Programming, Recursion, Memoization
Difficulty: Medium
Problem Description
Given an m x n grid where 1 represents an obstacle and 0 represents an empty cell, return the number of unique paths from the top-left cell to the bottom-right cell. You may move only right or down, and you cannot move through an obstacle.
Approach
Use recursion with memoization to count valid paths from each cell to the destination.
Steps
1. If the starting cell is an obstacle, return 0.
2. If the current row or column is outside the grid, return 0.
3. If the destination is reached, return 1 if it is open; otherwise, return 0.
4. If dp[i][j] already contains a result, return it.
5. Explore the right cell only if it is inside the grid and is not an obstacle.
6. Explore the cell below only if it is inside the grid and is not an obstacle.
7. Add both path counts, store the result in dp[i][j], and return it.
Code (Java)
class Solution {
    int dp[][];

    public int find(int i, int j, int ei, int ej, int[][] arr) {
        if (i > ei || j > ej) return 0;
        if (i == ei && j == ej) {
            return arr[i][j] == 1 ? 0 : 1;
        }
        if (dp[i][j] != -1) return dp[i][j];

        int r = 0, d = 0;
        if (j + 1 <= ej && arr[i][j + 1] != 1) {
            r = find(i, j + 1, ei, ej, arr);
        }
        if (i + 1 <= ei && arr[i + 1][j] != 1) {
            d = find(i + 1, j, ei, ej, arr);
        }
        return dp[i][j] = r + d;
    }

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        dp = new int[m][n];
        for (int k = 0; k < m; k++) {
            Arrays.fill(dp[k], -1);
        }
        if (obstacleGrid[0][0] == 1) return 0;
        int i = 0, j = 0, ei = m - 1, ej = n - 1;
        return find(i, j, ei, ej, obstacleGrid);
    }
}
Complexity Analysis
- Time: O(m × n) — each cell's result is computed at most once.
- Space: O(m × n) for the memoization table, plus recursion stack space.
2. LeetCode 931 — Minimum Falling Path Sum
Topic: Dynamic Programming, Recursion, Memoization
Difficulty: Medium
Problem Description
Given an n x n integer matrix, find the minimum sum of a falling path. A falling path starts at any cell in the first row and, for each next row, moves directly down, diagonally down-left, or diagonally down-right.
Approach
Use recursion with memoization to calculate the minimum falling path sum starting from each possible cell in the first row.
Steps
1. Try every column in the first row as a starting point.
2. If the row or column is outside the matrix, return Integer.MAX_VALUE to mark an invalid move.
3. If the current cell is in the last row, return its value.
4. If the result is already stored in dp[row][col], return it.
5. Recursively calculate the three possible moves: down, down-left, and down-right.
6. Choose the minimum of those results and add the current cell's value.
7. Store the result in dp[row][col].
8. Return the minimum result across all starting columns.
Code (Java)
class Solution {
    int dp[][];

    public int find(int row, int col, int arr[][]) {
        int m = arr.length - 1;
        int n = arr[0].length - 1;

        if (row > m || col > n || col < 0) return Integer.MAX_VALUE;
        if (row == m) return arr[row][col];
        if (dp[row][col] != Integer.MIN_VALUE) return dp[row][col];

        int down = find(row + 1, col, arr);
        int dl = find(row + 1, col - 1, arr);
        int dr = find(row + 1, col + 1, arr);

        return dp[row][col] =
            Math.min(down, Math.min(dl, dr)) + arr[row][col];
    }

    public int minFallingPathSum(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;

        dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(dp[i], Integer.MIN_VALUE);
        }

        int ans = Integer.MAX_VALUE;
        for (int col = 0; col < n; col++) {
            ans = Math.min(ans, find(0, col, matrix));
        }
        return ans;
    }
}
Complexity Analysis
- Time: O(n²) — each cell's result is computed at most once.
- Space: O(n²) for the memoization table, plus recursion stack space.
Implementation Note
Your submitted approach is preserved above. One edge case to watch: adding a cell value to Integer.MAX_VALUE from an invalid child can overflow an int. A safer version checks whether the minimum child result is Integer.MAX_VALUE before adding the current value, or uses a suitably small infinity value.
Key Takeaway
Both problems use top-down Dynamic Programming:
- LeetCode 63: Count valid paths while avoiding blocked cells.
- LeetCode 931: Find the minimum path sum while considering three possible moves to the next row.
The shared pattern is to define a state, handle base cases, explore valid next moves, and memoize results to avoid repeated calculations.
Java note: Add import java.util.Arrays; (or import java.util.*;) at the top if compiling these snippets outside LeetCode.