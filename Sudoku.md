# Sudoku

---

*Sudoku* is a logic based, combinatorial number placement puzzle. Given a partially completed $9 \times 9$ grid of numbers from 1 to 9 we fill all the remaining boxes with digits from 1 to 9 such that every row, column, diagonal and each of the nine $3 \times 3$  grids contain all the digits from 1 to 9.

|                        Sudoku puzzle                         |                           Solution                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![A typical Sudoku puzzle, with nine rows and nine columns that intersect at square spaces. Some of the cells are filled with a number; others are blank cells to be solved.](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Sudoku_Puzzle_by_L2G-20050714_standardized_layout.svg/250px-Sudoku_Puzzle_by_L2G-20050714_standardized_layout.svg.png) | ![The previous puzzle, showing its solution.](https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Sudoku_Puzzle_by_L2G-20050714_solution_standardized_layout.svg/250px-Sudoku_Puzzle_by_L2G-20050714_solution_standardized_layout.svg.png) |

A naive approach to solving this problem is to generate all possible configurations by placing numbers from 1 to 9 in the empty cells. This would result in a total of $9^e$ configurations, where $e$ is the number of empty cells in the puzzle, whose validity can checked in constant time. This gives us the time complexity of this algorithm as $O(9^e)$.

We shall now explore two different algorithms that can solve this puzzle faster.

1. Backtracking
2. Crook’s algorithm

## Backtracking

*Backtracking* is an algorithm that incrementally builds candidates to the solutions and abandons a candidate as soon as it determines that the candidate cannot possibly lead to a valid solution. When the algorithm abandons a candidate, it typically returns to a previous stage in the problem solving process. *Backtracking* is often much faster than the brute force solutions for a problem since it can eliminate multiple candidates with a single test.

The algorithm to solve sudoku with backtracking is as follows

1. Assign a number to an empty cell such that it can be part of the solution, that is it doesn’t violate any constraints of the puzzle.
2. Recursively check whether the above assignment leads to valid solution or not. If the above assignment fails we try the next number. If none of the numbers 1 to 9 lead to solution, we can conclude that puzzle has no solution.

A visualisation of this code along with the code can be found [here](https://trinket.io/python/ae539dcb34).



