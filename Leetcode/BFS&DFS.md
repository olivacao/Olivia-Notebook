# BFS/DFS

### 1. [Number of Islands](https://leetcode.com/problems/number-of-islands/) 200

```java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int row = grid.length;
        int col = grid[0].length;
        int count = 0;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == '1') {
                    BFS(grid,i,j);
                    count++;
                }
            }
        }
        return count;
    }
    private void BFS(char[][]grid, int i, int j) {
        int row = grid.length;
        int col = grid[0].length;
        int[] directionY = {-1,1,0,0};
        int[] directionX = {0,0,-1,1};
        
        Queue<Cell> queue = new ArrayDeque<>();
        queue.offer(new Cell(i,j));
        grid[i][j] = '0';
        while (!queue.isEmpty()) {
            Cell cur = queue.poll();
        
            for (int m = 0; m < 4; m++) {
                int curX = cur.x + directionX[m];
                int curY = cur.y + directionY[m];
                if (inBound(grid, curX, curY) && grid[curX][curY] == '1') {
                    queue.offer(new Cell(curX, curY));
                    grid[curX][curY] = '0';
                }
            }
        }
    }
    private boolean inBound(char[][] grid, int i, int j){
        int row = grid.length;
        int col = grid[0].length;
        return i >= 0 && j >= 0 && i < row && j < col;
    }
}
class Cell {
    int x;
    int y;
    public Cell(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

