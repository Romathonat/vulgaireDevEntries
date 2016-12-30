The [eight queens puzzle](https://en.wikipedia.org/wiki/Eight_queens_puzzle) is a classic algorithmic problem.

You have a chessboard of 8*8. You have 8 queens. Your goal is to place the 8 queens on the board, without any of them threatening another one. A queen is threaten if she is on the same row, or the same column, or the same diagonal of another queen (like in the rules of chess).

A brute-force approache may take too much time here, because we have $$ 8 parmi 64$$ possibilities. The interesting thing about this problem is that you have different approaches to solve it (check on [Wikipedia](https://en.wikipedia.org/wiki/Eight_queens_puzzle)): brute-force, dynamic programming, genetic algorithm...

An interesting (and simple) approach that I find is the 'iterative repair'. The idea is to place a queen on each row, with a random position on this row. Then, we find the queen the more threaten, and we change her location on the same row. Here, I chose a new random position on the row, but selecting the position with the less conflict should be a very efficient heuristic (I kept it simple here).
