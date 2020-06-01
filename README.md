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
        
        for(int row = 0; row < rows; row++){
            for(int column = 0; column < columns; column++){
                if(grid[row][column] == 1){
                    return -1;
                }
            }
        }
        
        return minutes;
    }
    
    private boolean isValid(int[][] grid, int row , int column){
        if(row < 0 || row >= grid.length || column < 0 || column >= grid[0].length) 
            return  false;
        return true;
    }
}
```

# References :
https://www.youtube.com/watch?v=vzPobzjJYbw
