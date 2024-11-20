## LeetCode 51. N皇后

### 题目描述

n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。给你一个整数 n，返回所有不同的 n 皇后问题 的解决方案。每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

**示例 1:**
```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2:**
```
输入：n = 1
输出：[["Q"]]
```

**提示：**
- `1 <= n <= 9`

### Java 实现代码

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> solutions = new ArrayList<List<String>>();
        int[] queens = new int[n];
        Arrays.fill(queens, -1);
        Set<Integer> columns = new HashSet<Integer>();
        Set<Integer> diagonals1 = new HashSet<Integer>();
        Set<Integer> diagonals2 = new HashSet<Integer>();
        backtrack(solutions, queens, n, 0, columns, diagonals1, diagonals2);
        return solutions;
    }

    public void backtrack(List<List<String>> solutions, int[] queens, int n, int row, Set<Integer> columns, Set<Integer> diagonals1, Set<Integer> diagonals2) {
        if (row == n) {
            List<String> board = generateBoard(queens, n);
            solutions.add(board);
        } else {
            for (int i = 0; i < n; i++) {
                if (columns.contains(i)) {
                    continue;
                }
                int diagonal1 = row - i;
                if (diagonals1.contains(diagonal1)) {
                    continue;
                }
                int diagonal2 = row + i;
                if (diagonals2.contains(diagonal2)) {
                    continue;
                }
                queens[row] = i;
                columns.add(i);
                diagonals1.add(diagonal1);
                diagonals2.add(diagonal2);
                backtrack(solutions, queens, n, row + 1, columns, diagonals1, diagonals2);
                queens[row] = -1;
                columns.remove(i);
                diagonals1.remove(diagonal1);
                diagonals2.remove(diagonal2);
            }
        }
    }

    public List<String> generateBoard(int[] queens, int n) {
        List<String> board = new ArrayList<String>();
        for (int i = 0; i < n; i++) {
            char[] row = new char[n];
            Arrays.fill(row, '.');
            row[queens[i]] = 'Q';
            board.add(new String(row));
        }
        return board;
    }
}

```

### 解题思路

1. **棋盘表示与状态跟踪**
- 用一个 `int[] queens` 数组表示棋盘的状态，其中 `queens[i]` 表示第 `i` 行的皇后放置在第 `queens[i]` 列。
- 使用 `Set<Integer> columns`、`Set<Integer> diagonals1` 和 `Set<Integer> diagonals2` 来追踪当前已被占用的列和对角线。

2. **回溯（Backtracking）**
- **主函数：** `solveNQueens(int n)` 初始化相关变量，然后调用 `backtrack` 函数开始回溯过程。
- **回溯过程：** 从 `row = 0` 开始，逐行放置皇后。每次尝试放置皇后时，检查该位置是否与已放置的皇后发生冲突。若无冲突，则放置皇后并递归到下一行。若成功放置所有皇后（即 `row == n`），则构建一个解，并将其添加到 `solutions` 中。
- **冲突判断：**
  - **列冲突：** 如果当前列 `i` 已经被占用（`columns.contains(i)`），则跳过。
  - **主对角线冲突：** 主对角线上的位置由 `row - i` 决定，如果这个值在 `diagonals1` 集合中，说明该主对角线已被占用。
  - **副对角线冲突：** 副对角线上的位置由 `row + i` 决定，如果这个值在 `diagonals2` 集合中，说明该副对角线已被占用。
- **递归和回溯：**
  - 放置皇后后，递归到下一行（`backtrack(solutions, queens, n, row + 1, columns, diagonals1, diagonals2)`）。
  - 递归返回时，撤销当前的选择，即恢复 `queens`、`columns`、`diagonals1` 和 `diagonals2` 的状态。

3. **生成棋盘**
- 每次成功放置一个解（即 `row == n` 时），调用 `generateBoard` 函数将当前 `queens` 数组转换为棋盘的字符串表示，加入到解集中。


### 复杂度分析

- **时间复杂度**：O(N!)，最坏情况下需要遍历所有可能的排列。
- **空间复杂度**：O(N)，其中 N 是皇后数量。空间复杂度主要取决于递归调用层数、记录每行放置的皇后的列下标的数组以及三个集合，递归调用层数不会超过 N，数组的长度为 N，每个集合的元素个数都不会超过 N
