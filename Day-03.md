# Daily LeetCode Log — October 13, 2026

Today's practice focused on **Dynamic Programming (DP)** through two patterns: the Longest Increasing Subsequence and the largest square containing only `1`s in a binary matrix.

## Problems Solved

1. [LeetCode 221 — Maximal Square](https://leetcode.com/problems/maximal-square/)
2. [LeetCode 300 — Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

---

## 1. LeetCode 221 — Maximal Square

**Topic:** Dynamic Programming, 2D DP  
**Difficulty:** Medium

### Problem Description

Given a binary matrix containing `'0'` and `'1'`, find the largest square containing only `'1'`s and return its **area**.

### Approach

Use a 2D DP table where `dp[i][j]` represents the side length of the largest all-`1` square whose **bottom-right corner** is at cell `(i, j)`.

**Steps**
1. Create a DP table with the same dimensions as the matrix and initialize `maxSide = 0`.
2. Traverse every cell.
3. If the current cell is `'0'`, its DP value remains `0`.
4. If the current cell is `'1'` in the first row or first column, set `dp[i][j] = 1`.
5. Otherwise, check the **top**, **left**, and **top-left diagonal** DP values.
6. Set `dp[i][j] = 1 + min(top, left, diagonal)`.
7. Update `maxSide` with the largest side length found.
8. Return `maxSide * maxSide`, because the question asks for area.

### Code (Java)

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;

        int[][] dp = new int[m][n];
        int maxSide = 0;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '1') {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = 1 + Math.min(
                            dp[i - 1][j],
                            Math.min(
                                dp[i][j - 1],
                                dp[i - 1][j - 1]
                            )
                        );
                    }
                    maxSide = Math.max(maxSide, dp[i][j]);
                }
            }
        }
        return maxSide * maxSide;
    }
}
```

### Complexity Analysis

- **Time:** `O(m × n)` — each cell is processed once.
- **Space:** `O(m × n)` — the DP table matches the matrix dimensions.

### Key Insight

A square can grow at `(i, j)` only when the top, left, and top-left diagonal cells support it. Taking the minimum prevents the square from including a `0`.

---

## 2. LeetCode 300 — Longest Increasing Subsequence

**Topic:** Dynamic Programming, Recursion, Memoization  
**Difficulty:** Medium

### Problem Description

Given an integer array `nums`, return the length of the longest **strictly increasing subsequence**. The selected elements keep their original order, but they do not need to be adjacent.

### Approach

Use recursion with memoization. `idx` is the index of the previously selected element, and `j` is the current candidate being considered.

**Steps**
1. Try every index as the first selected element.
2. Count that starting element as length `1`, then call `find(i, i + 1, nums)` for the remaining elements.
3. If `j` reaches the end of the array, return `0`.
4. If `dp[idx][j]` already contains a result, return it.
5. **Take:** If `nums[j] > nums[idx]`, select it and calculate `1 + find(j, j + 1, nums)`.
6. **Skip:** Ignore the current element and calculate `find(idx, j + 1, nums)`.
7. Store the larger of the take and skip results in `dp[idx][j]`.
8. Return the maximum result across all starting indices.

### Code (Java)

```java
class Solution {
    int dp[][];

    public int find(int idx, int j, int nums[]) {
        if (j >= nums.length) {
            return 0;
        }

        if (dp[idx][j] != -1) return dp[idx][j];

        int take = 0;
        if (nums[idx] < nums[j]) {
            take = 1 + find(j, j + 1, nums);
        }

        int skip = find(idx, j + 1, nums);

        return dp[idx][j] = Math.max(take, skip);
    }

    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        dp = new int[n][n];

        for (int i = 0; i < n; i++) {
            Arrays.fill(dp[i], -1);
        }

        int ans = 1;
        for (int i = 0; i < n; i++) {
            int x = 1 + find(i, i + 1, nums);
            ans = Math.max(ans, x);
        }

        return ans;
    }
}
```

### Complexity Analysis

- **Time:** `O(n²)` — there are `O(n²)` possible `(idx, j)` states, and each is memoized.
- **Space:** `O(n²)` for the DP table, plus recursion stack space.

### Key Insight

At each candidate element, choose to **take** it if it is greater than the previously selected element, or **skip** it. Memoization avoids recalculating the same `(idx, j)` state.

---

## Daily Summary

| Problem | DP Pattern | Main Decision |
|---|---|---|
| LeetCode 221 — Maximal Square | Bottom-up 2D DP | Extend a square using top, left, and diagonal neighbors |
| LeetCode 300 — Longest Increasing Subsequence | Top-down DP with memoization | Take or skip the current element |

**What I practiced today:** Two DP approaches—building answers iteratively in a grid and caching recursive take/skip decisions.

> Java note: Add `import java.util.Arrays;` (or `import java.util.*;`) at the top of the LeetCode 300 snippet if compiling outside LeetCode.
