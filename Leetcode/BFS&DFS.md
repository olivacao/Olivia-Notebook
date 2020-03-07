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

### 2. Walls and Gates 286
**Problem:**  

You are given a m x n 2D grid initialized with these three possible values.  
`-1`: A wall or an obstacle.  
`0`: A gate.  
`INF`: Infinity means an empty room.  
We use the value `2^31 - 1 = 2147483647` to represent INF as you may assume that the distance to a gate is less than 2147483647.  
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a Gate, that room should remain filled with `INF`  

**Example:**
```
Input:
[[0,-1],[2147483647,2147483647]]
Output:
[[0,-1],[1,2]]
```
**First Solution:**

```java
public class Solution {
    /**
     * @param rooms: m x n 2D grid
     * @return: nothing
     */
    public void wallsAndGates(int[][] rooms) {
        // write your code here
        if (rooms == null || rooms.length == 0 || rooms[0].length == 0) {
            return;
        }
        int row = rooms.length;
        int col = rooms[0].length;
        
        
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (rooms[i][j] == 0) {
                    BFS(rooms,i,j);
                }
            }
        }
    }
    private void BFS(int[][] rooms, int i, int j) {
        int row  = rooms.length;
        int col = rooms[0].length;
        Queue<Cell> queue= new ArrayDeque<>();
        queue.offer(new Cell(i,j));
        int[] directionX = {-1,1,0,0};
        int[] directionY = {0,0,-1,1};
      
        while (!queue.isEmpty()) {
            Cell cur = queue.poll();
            for (int m = 0; m < 4; m++) {
                int nextX = directionX[m] + cur.x;
                int nextY = directionY[m] + cur.y;
                if (inBound(rooms, nextX, nextY)) {
                    if (rooms[nextX][nextY] > 0 && rooms[nextX][nextY] > rooms[cur.x][cur.y] + 1) {
                        rooms[nextX][nextY] = rooms[cur.x][cur.y] + 1;
                        queue.offer(new Cell(nextX,nextY));
                    }
                }
            }
           
            
        }
    }
    private boolean inBound(int[][] rooms, int i, int j) {
        int row = rooms.length;
        int col = rooms[0].length;
        
        return (i >= 0 && j >= 0 && i < row && j < col);
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

**Second Solution**
Step1: push all doors into queue  
Step2: BFS all doors and update each `INF`. 
```java
public class Solution {
    /**
     * @param rooms: m x n 2D grid
     * @return: nothing
     */
    public void wallsAndGates(int[][] rooms) {
        // write your code here
        if (rooms == null || rooms.length == 0 || rooms[0].length == 0) {
            return;
        }
        int row = rooms.length;
        int col = rooms[0].length;
        Queue<Cell> queue = new ArrayDeque<>();
        int[] dx = {0,0,1,-1};
        int[] dy = {1,-1,0,0};
        //push all door into queue
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (rooms[i][j] == 0) {
                    queue.offer(new Cell(i,j));
                }
            }
        }
        //BFS each door
        while (!queue.isEmpty()) {
            Cell cur = queue.poll();
            for (int i = 0; i < 4; i++) {
                int nextX = cur.x + dx[i];
                int nextY = cur.y + dy[i];
                if (inBound(rooms,nextX,nextY) && rooms[nextX][nextY] == Integer.MAX_VALUE) {
                    queue.offer(new Cell(nextX,nextY));
                    rooms[nextX][nextY] = rooms[cur.x][cur.y] + 1;
                }
            }
            
        }
        
    }
  
    private boolean inBound(int[][] rooms, int i, int j) {
        int row = rooms.length;
        int col = rooms[0].length;
        
        return (i >= 0 && j >= 0 && i < row && j < col);
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
