The [eight queens puzzle](https://en.wikipedia.org/wiki/Eight_queens_puzzle) is a classic algorithmic problem.

# Problem
You have a chessboard of 8*8 square. You have 8 queens. Your goal is to place the 8 queens on the board, without any of them threatening another one. A queen is threaten if she is on the same row, or the same column, or the same diagonal of another queen (like in the rules of chess).

# Solutions
A brute-force approach may take too much time here, because we have $$\binom{8}{64}$$ possibilities. The interesting thing about this problem is that you have different approaches to solve it (check on [Wikipedia](https://en.wikipedia.org/wiki/Eight_queens_puzzle)): brute-force, dynamic programming, genetic algorithms...

An interesting (and simple) approach is the 'iterative repair'. The idea is to place a queen on each row, with a random position on this row. Then, we find the queen the more threaten, and we change her location on the same row : it is the **repair** operation. Here, I chose a new random position on the row, but selecting the position with the less conflict should be a very efficient heuristic (I kept it simple here).

This way of solving the problem is a greedy one. So, it can stay on a local extremum without finding a global one (= a solution). A solution to that is to give a count that you decrement each time you "repair" the current configuration. If that count is reached without finding that the current configuration is a solution, we consider that we are on a local extremum, we will not find a solution, so we generate another configuration.

![](https://github.com/Romathonat/vulgaireDevEntries/blob/master/eightQueensPuzzle/localmaxmin.png)  
*This function represents all possible configurations. When we reach a minimum, it is a solution. There are configurations when we stay in the local minimum, repairing after repairing we move a little on the curve but we stay here !*

Another thing with this algorithm is that it will find a solution, not necessarily all the solutions (or you must  repeat the algorithm but there is clever things to do if you want to find all solutions). But if you want to find a solution quickly, even on a big chessboard, this solution is good. In fact it works on way bigger chessboard : with 1 000 000 queens, the algorithm with the optimization of selecting the best next place when "repairing" takes 50 steps in average.
```python
from random import randint

#0 means no queen on this location, 1 that there is one.
game_board = [[0 for i in range(8)] for j in range(8)]

def init_board(game_board):
    #we place a queen on each row
    for i in range(len(game_board)):
        game_board[i][randint(0,7)] = 1

def check_configuration(game_board):
    """This function test if we are in a good configuration.
    If yes, it returns true, if not, it returns the location of the more conflicting queen """

    #key is the location of the queen, second is the number of conflict
    count_conflict = {}

    for i in range(len(game_board)):
        for j in range(len(game_board[i])):
            if(game_board[i][j] == 1):
                #we add this queen if we do not have found her yet
                count_conflict[(i,j)] = count_conflict.get((i,j), 0)

                #we check the top
                for k in range(i-1,0,-1):
                    if game_board[k][j] == 1:
                        count_conflict[(k,j)] = count_conflict.get((k,j), 0) + 1
                        #print('nique {} {} depuis {} {}'.format(k,j,i,j))
                        # we break because the current queen will not affect
                        # other queen behind the one we just found
                        break

                #we check the left
                for l in range(j-1,0,-1):
                    if game_board[i][l] == 1:
                        count_conflict[(i,l)] = count_conflict.get((i,l), 0) + 1
                        break

                #we check the bottom
                for k in range(i+1,len(game_board)):
                    if game_board[k][j] == 1:
                        count_conflict[(k,j)] = count_conflict.get((k,j), 0) + 1
                        break

                #we check the right
                for l in range(j+1,len(game_board[i])):
                    if game_board[i][l] == 1:
                        count_conflict[(i,l)] = count_conflict.get((i,l), 0) + 1
                        break

                #we check the upper left diagonal:
                if(i-1 >= 0 and j-1 >= 0):
                    next_location = (i-1,j-1)

                    while(next_location[0] >= 0 and next_location[1] >= 0):
                        if(game_board[next_location[0]][next_location[1]] == 1):
                            count_conflict[(next_location[0],next_location[1])] = \
                            count_conflict.get((next_location[0],next_location[1]), 0) + 1
                            break

                        next_location = (next_location[0]-1, next_location[1]-1)

                #we check the bottom left diagonal:
                if(i+1 < len(game_board) and j-1 >= 0):
                    next_location = (i+1,j-1)

                    while(next_location[0] < len(game_board) and next_location[1] >= 0):
                        if(game_board[next_location[0]][next_location[1]] == 1):
                            count_conflict[(next_location[0],next_location[1])] = \
                            count_conflict.get((next_location[0],next_location[1]), 0) + 1
                            break

                        next_location = (next_location[0]+1, next_location[1]-1)

                #we check the bottom right diagonal:
                if(i+1 < len(game_board) and j+1 < len(game_board[i])):
                    next_location = (i+1,j+1)
                    while(next_location[0] < len(game_board) and next_location[1] < len(game_board[i])):
                        if(game_board[next_location[0]][next_location[1]] == 1):
                            count_conflict[(next_location[0],next_location[1])] = \
                            count_conflict.get((next_location[0],next_location[1]), 0) + 1
                            break

                        next_location = (next_location[0]+1, next_location[1]+1)

                #we check the upper right diagonal:
                if(i-1 >= 0 and j+1 < len(game_board[i])):
                    next_location = (i-1,j+1)

                    while(next_location[0] >= 0 and next_location[1] < len(game_board[i])):
                        if(game_board[next_location[0]][next_location[1]] == 1):
                            count_conflict[(next_location[0],next_location[1])] = \
                            count_conflict.get((next_location[0],next_location[1]), 0) + 1
                            break

                        next_location = (next_location[0]-1, next_location[1]+1)


    more_conflicting = max(count_conflict, key=count_conflict.get)
    if(count_conflict[(more_conflicting[0],more_conflicting[1])] == 0):
        return True
    else:
        return more_conflicting

def print_game_board(game_board):
    for i in range(len(game_board)):
        print(game_board[i])

init_board(game_board)
more_conflicting = check_configuration(game_board)

#let's say we let our algorithm try 1000 times from an initial configuration. If we can't find
#a solution, we try with another random generation
count = 1000

while(more_conflicting != True):
    # we change the location of the queen
    game_board[more_conflicting[0]][more_conflicting[1]] = 0
    game_board[more_conflicting[0]][randint(0,7)] = 1
    more_conflicting = check_configuration(game_board)

    if count == 0:
        init_board(game_board)
        count = 1000
    count -= 1

print_game_board(game_board)

```
If we want to find all solutions, we can make a brute-force algorithm with a little of help : we begin the search from a configuration where there is a queen on each row ($$8^8 = 16,777,216$$ possibilities). If we do this with a DFS, row by row, we can eliminate many solutions soon in the search tree (it is the same idea as a branch-and-bound approach, you cut branches that you know are not good solutions).



