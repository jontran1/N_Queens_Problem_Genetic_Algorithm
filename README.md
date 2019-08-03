Jonathan Tran

Program 2 N-Queens Problem

### Introduction

​	The n queens problem is the problem of placing n queens on a chessboard so that no two queens threaten each other i.e on the same row or diagonal. The naïve solution would be to generate all possible configurations of the chess board until a solution is found, but that will take an extremely long time depending on the board size.

```code
while solution is not found:
	generate the next possible solution.
	if possible solution is correct:
		print this solution.
	end if.
end loop.
```

### Time Complexity

​	The N Queen backtracking solution checks for all possible arrangements of N Queens on the board. The number of possible arrangements of N Queens on an NxN board is N!. Since each column is only allowed one queen. I am already skipping the column and already have the queen placed. The average and worst case time complexity is O(N!), since, it is checking all possible arraignments i.e N^n. The best time complexity is if it finds a solution before checking all possible N^N arrangements. 

### Backtracking Solution

​	The backtracking solution checks each column and row one by one. Each column can only have one queen. So once a valid position is found for the column, a queen will be placed on the stack and board. The next column and its corresponding row are all checked for valid positions (that's why its N^n for possible configurations). If a none of the positions are valid in that column. The backtrack function will be called. It will pop the most recent queen of the stack and look for the next available position for that queen's column. If none of the rows are valid, then backtrack function will be called again. The process will repeat until all the queens are placed on the board. The backtracking solution is able to find the solution when n = 10, 25, and 50 if given enough time. But at n = 100, the required is too much. Its just not a viable option to use backtracking for when n = 100. 

```java
    /**
     * This function will check if the current position is valid for a queen placement.
     * @return
     */
    private boolean isValid() {
        //If current queen is 0 then there's no need to check. It is the only piece on the board.
        if(currentColumn == 0){
            return true;
        }
        /*
        This function will never check if the two points are on the same column,
        because currentColumn will never only have one queen for each column.
         */
        for(int i = 0 ; i < stack.size(); i++){
            Queen currentQueen = stack.get(i);
            //Check if the two points are on the same row.
            if(currentQueen.getX() == currentRow){
                return false;
            }
            if (Math.abs(currentQueen.getX()-currentRow) == Math.abs(currentQueen.getY() - currentColumn)){
                return false;
            }
        }
        return true;
    }
    /**
     * If a position isn't valid this backtrack function will be called.
     * This function will pop the most recent. It will then check the next available
     * position for that current queen's column and row. It the max row is reach. It
     * will recursively call the backtrack function again to pop the next queen since the last
     * queen available positions have been exhausted.
     * @throws Exception
     */
    public void backTrack() throws Exception {
        Queen tempQueen = stack.pop();
        chessBoard[currentColumn] = currentRow;
        if(stack.isEmpty()){
            if(tempQueen.getX() == rows-1 && tempQueen.getY() == 0){
                throw new Exception("No solution found");
            }
        }
        if(tempQueen.getX() == rows-1){
            backTrack();
        }else {
            currentRow = tempQueen.getX() + 1;
            currentColumn = tempQueen.getY();
        }
    }
```

### Genetic Algorithm Solution

​	Genetic algorithm is a an algorithm based on the process of natural selection and evolution. When applying genetic algorithm to N-Queens we hope to converge on a solution. First, a population of randomly generated solutions or chromosomes are created. (These chromosomes are represented as a list of integers of the permutation of (0,1,2,...n-1)). A new population is generated by randomly selecting fit parents and creating children. The overall fitness for the next population is better thanks to my selection operator for having a high probability of selecting fit parents due to the population already being sorted. So this process will repeat until a solution is found. With a population size 1000 a solution is quickly found  for when n = 4, 8, 10, 25, and even 100. Sometimes depending on n, a solution is found right away because it randomly generated a correct solution, so breeding isn't need. At n = 100, with population size of 100 it will take less then 100 generations. However, sometimes it may take more. The problem is its randomly selecting fit parents, creating child, and randomly mutating the children's DNA. Eventually it will converge on an answer given enough time and at a better rate then backtracking with its O(N!) time complexity. 

```java
    /**
     * Two random variables are generated. One mutation has an 80 percent chance and
     * the other has a 40 percent chance. If mutation is successful then mutationHelper function is called.
     * @param childSequence
     */
    private static void mutation(List<Integer> childSequence){
        Random random = new Random();
        int prob1 = random.nextInt(100);
        int prob2 = random.nextInt(100);

        if(prob1 <= 80){
            mutationHelper(childSequence);
        }
        if(prob2 <= 40){
            mutationHelper(childSequence);
        }

    }

    /**
     * Two indexes are randomly selected and swapped.
     * @param childSequence
     */
    private static void mutationHelper(List<Integer> childSequence){
        Random random = new Random();
        int i = random.nextInt(childSequence.size()/2);
        int j = i + random.nextInt(childSequence.size()-i);
        Collections.swap(childSequence,i,j);
    }
```

















### Conclusion

​	Backtracking takes on average O(N!), but if it would go through all possible configurations of the board it will take N^N. At n = 100, backtracking isn't a viable option the time needed would take far to long. Choosing the most fit solution and making that solution mate with other fit solutions will sometimes produce even better solutions. Given enough time, genetic algorithm will converge on a solution in a reasonable time even when n = 100. 



#### References

Sarkar, Uddalok, and Sayan Nag Nag. *An Adaptive Genetic Algorithm for Solving NQueens Problem*. 					 	arxiv.org/ftp/arxiv/papers/1802/1802.02006.pdf.
