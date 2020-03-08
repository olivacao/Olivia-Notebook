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
//Time = O(mn)
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
[[2147483647,-1,0,2147483647],[2147483647,2147483647,2147483647,-1],[2147483647,-1,2147483647,-1],[0,-1,2147483647,2147483647]]
Output:
[[3,-1,0,1],[2,2,1,-1],[1,-1,2,-1],[0,-1,3,4]]

Explanation:
the 2D grid is:
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
the answer is:
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
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

**Second Solution:**  
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
### 3.[Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/) 130
**Solution:**
```java
class Solution {
    public void solve(char[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0) {
            return;
        }
        int row = board.length;
        int col = board[0].length;
        int[][] direct = {{0,1},{0,-1},{-1,0},{1,0}};
        Queue<Cell> queue = new ArrayDeque<>();
        //traverse all cells on board and push them to queue        
        //top and bottom board
        for (int i = 0; i < col; i++) {
            if (board[0][i] == 'O') {
                queue.offer(new Cell(0,i));
            }
            if (board[row - 1][i] == 'O') {
                queue.offer(new Cell(row-1,i));
            } 
        }
        //left and right
        for (int i = 1; i < row-1; i++) {
            if (board[i][0] == 'O') {
                queue.offer(new Cell(i,0));
            }
            if (board[i][col-1] == 'O') {
                queue.offer(new Cell(i,col-1));
            }
        }
        
        //poll the queue, mark all O connected to the board O to B
        while(!queue.isEmpty()) {
            Cell cur = queue.poll();
            board[cur.x][cur.y] = 'B';
            for (int i = 0; i < 4; i++) {
                int nx = cur.x + direct[i][0];
                int ny = cur.y + direct[i][1];
                if (nx >= 0 && ny >= 0 && nx < row && ny < col && board[nx][ny] == 'O') {
                    queue.offer(new Cell(nx,ny));
                }
            }
        }
        //change all B to O, the rest O to X
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] == 'B') {
                    board[i][j] = 'O';
                } else if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                }
            }
        }
        
    }
}
class Cell {
    int x;
    int y;
    public Cell(int x,int y) {
        this.x = x;
        this.y = y;
    }
}
//Time = O(mn)
```

### 4.[Nested List Weight Sum](https://www.lintcode.com/problem/nested-list-weight-sum/description)
**Solution1:**(Recursion)
```java
public class Solution {
    public int depthSum(List<NestedInteger> nestedList) {
        // Write your code here
        return DFS(nestedList,1);
    }
    private int DFS(List<NestedInteger> nestedList, int depth) {
        if (nestedList == null || nestedList.size() == 0) {
            return 0;
        }
        int sum = 0;
        for(NestedInteger ele : nestedList) {
            if (ele.isInteger()) {
                sum += ele.getInteger() * depth;
            } else {
                sum += DFS(ele.getList(),depth+1);
            }
        }
        return sum;
    }
   
}
```
**Solution2:**(iterative)
```java
public class Solution {
    public int depthSum(List<NestedInteger> nestedList) {
        // Write your code here
        int sum = 0;
        int depth = 1;
        Queue<List<NestedInteger>> queue = new ArrayDeque<>();
        if (nestedList == null || nestedList.size() == 0) {
            return 0;
        }
        queue.offer(nestedList);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                List<NestedInteger> cur = queue.poll();
                for (NestedInteger ele: cur) {
                    if (ele.isInteger()) {
                        sum += ele.getInteger()*depth;
                    } else {
                        queue.offer(ele.getList());
                    }
                }
            }
            depth++;
        }
        
        return sum;
    }
}

```

### 5.[Nested List Weight Sum II](https://www.lintcode.com/problem/nested-list-weight-sum-ii/description)
**Solution:**
```java
public class Solution {
    /**
     * @param nestedList: a list of NestedInteger
     * @return: the sum
     */
    public int depthSumInverse(List<NestedInteger> nestedList) {
        // Write your code here.
        if (nestedList == null || nestedList.size() == 0) {
            return 0;
        }
        Queue<List<NestedInteger>> queue = new ArrayDeque<>();
        queue.offer(nestedList);
        int preSum = 0;
        int ans = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            int levelSum = 0;
            for (int i = 0; i < size; i++) {
                List<NestedInteger> cur = queue.poll();
                for (NestedInteger ele : cur) {
                    if (ele.isInteger()) {
                        levelSum += ele.getInteger();
                    } else {
                        queue.offer(ele.getList());
                    }
                }
            }
         
            preSum += levelSum;
            ans += preSum;
        }
        return ans;   
    }
   
}

```

### 6.[Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees)
**My Solution(DFS):**
```java
class Solution {
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
      int mins = 0;
      Map<Integer,List<Integer>> map = new HashMap<>();
      map.put(headID,new ArrayList<Integer>());
      for (int i = 0; i < n; i++) {
        List<Integer> cur = map.get(manager[i]);
        if (cur != null && manager[i]!= -1) {
          cur.add(i);              
        }
        if (cur == null) {
          List<Integer> tmp = new ArrayList<>();
          tmp.add(i);
          map.put(manager[i], tmp);
          }           
      }
      return DFS(map, headID, informTime);
    }
    public int DFS(Map<Integer,List<Integer>> map,Integer key,int[] informTime) {
      List<Integer> cur = map.get(key);
    	int maxTime = 0;
    	if (cur == null) {
    		return informTime[key];
    	}
    	for (Integer ele: cur) {
    		maxTime = Math.max(maxTime,DFS(map, ele, informTime));
    	}
    	return maxTime + informTime[key];
    }
    
}
```

**Solution(BFS)**

```java


//Time = O(n)
```
