# Rotting Oranges
## https://leetcode.com/problems/rotting-oranges

In a given grid, each cell can have one of three values:
```
the value 0 representing an empty cell;
the value 1 representing a fresh orange;
the value 2 representing a rotten orange.
```
**Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.**

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.

![Rotting Oranges](oranges.png?raw=true "Rotting Oranges")

# Implementation :
```java
class RottenOrange {
    int row, column, minute;
    public RottenOrange(int row, int column, int minute){
        this.row = row; 
        this.column = column; 
        this.minute = minute;
    }
}


class Solution {
    int[] x = {0, 0, -1, 1};
    int[] y = {1, -1, 0, 0};
    
    public int orangesRotting(int[][] grid) {
        int rows = grid.length, columns = grid[0].length;
        
        Queue<RottenOrange> q = new ArrayDeque<>();
        
        for(int row = 0; row < rows; row++){
            for(int column = 0; column < columns; column++){
                if(grid[row][column] == 2){
                    q.add(new RottenOrange(row, column, 0));
                }
            }
        }
        
        int minutes = 0;
        while(!q.isEmpty()){
            RottenOrange rottenOrange = q.poll();
            minutes = Math.max(minutes, rottenOrange.minute);
            
            for(int direction = 0; direction < x.length; direction++){
                int row = rottenOrange.row + x[direction];
                int column = rottenOrange.column + y[direction];
                
                if(isValid(grid, row, column) && grid[row][column] == 1) {
                    grid[row][column] = 2; 
                    q.add(new RottenOrange(row, column, rottenOrange.minute + 1));                  
                }
            }
        }
        
        for(int row = 0; row < rows; row++) {
            for(int column = 0; column < columns; column++) {
                if(grid[row][column] == 1)
                    return -1;
            }
        }
        
        return minutes;
    }
    
    private boolean isValid(int[][] grid, int row , int column) {
        if(row < 0 || row >= grid.length || column < 0 || column >= grid[0].length) 
            return  false;
        return true;
    }
}
```

#### Implementation 2 : Same approach
```java
class Solution {
    static int[][] sides = { {0, 1} , {0, -1} , {-1, 0} , {1, 0} };
    class RottenOrange{
        int row, column, minute;
        RottenOrange(int row, int column, int minute) {
            this.row = row; this.column = column; this.minute = minute;
        }
    }
    public int orangesRotting(int[][] grid) {
        if(grid == null || grid.length == 0)
            return 0;
        int max = 0;
        Queue<RottenOrange> q = new ArrayDeque<>();
        int rows = grid.length, columns = grid[0].length;
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < columns; j++) {
                if(grid[i][j] == 2)
                q.add(new RottenOrange(i, j, 0));
            }
        }
        while(!q.isEmpty()) {
            RottenOrange rottenOrange = q.remove();
            max = Math.max(max, rottenOrange.minute);
            int row = rottenOrange.row, column = rottenOrange.column;
            for(int i = 0; i < sides.length; i++) {
                int x = row + sides[i][0], y = column + sides[i][1];
                if(isValid(grid, x , y) && grid[x][y] == 1) {
                    grid[x][y] = 2;
                    q.add(new RottenOrange(x, y, rottenOrange.minute + 1));
                }
                    
            }
        }
        
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < columns; j++) {
                if(grid[i][j] == 1)
                    return -1;
            }
        }
        return max;
    }
    
    private boolean isValid(int[][] grid, int row, int column) {
        if(row < 0 || row >= grid.length || column < 0 || column >= grid[0].length)
            return false;
        return true;
    }
}
```

# Notes :
Note that using Stack in this case will result in an incorrect output.

![Rotting Oranges - Queue Vs. Stack](Rotten-Orange-Queue-Vs-Stack.JPG?raw=true "Rotting Oranges")

# References :
https://www.youtube.com/watch?v=vzPobzjJYbw
