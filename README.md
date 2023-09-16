 # suduko solver 

 #include <iostream>
using namespace std;

const int N = 9;

// Function to print the Sudoku grid
void printGrid(int grid[N][N]) {
    for (int row = 0; row < N; row++) {
        for (int col = 0; col < N; col++) {
            cout << grid[row][col] << " ";
        }
        cout << endl;
    }
}

// Function to check if a number can be placed in a cell
bool isSafe(int grid[N][N], int row, int col, int num) {
    // Check if 'num' is not already present in the current row and column
    for (int x = 0; x < N; x++) {
        if (grid[row][x] == num || grid[x][col] == num) {
            return false;
        }
    }

    // Check if 'num' is not already present in the current 3x3 subgrid
    int startRow = row - row % 3;
    int startCol = col - col % 3;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (grid[i + startRow][j + startCol] == num) {
                return false;
            }
        }
    }

    return true;
}

// Function to solve the Sudoku puzzle using backtracking
bool solveSudoku(int grid[N][N]) {
    int row, col;

    // Find the first empty cell in the grid
    bool isEmpty = false;
    for (row = 0; row < N; row++) {
        for (col = 0; col < N; col++) {
            if (grid[row][col] == 0) {
                isEmpty = true;
                break;
            }
        }
        if (isEmpty) {
            break;
        }
    }

    // If no empty cell is found, the Sudoku puzzle is solved
    if (!isEmpty) {
        return true;
    }

    // Try filling the empty cell with numbers from 1 to 9
    for (int num = 1; num <= 9; num++) {
        if (isSafe(grid, row, col, num)) {
            // Place the number in the cell
            grid[row][col] = num;

            // Recursively solve the rest of the puzzle
            if (solveSudoku(grid)) {
                return true;
            }

            // If no solution is found with this number, backtrack
            grid[row][col] = 0;
        }
    }

    // If no number can be placed in this cell, return false
    return false;
}

int main() {
    int initialGrid[N][N] = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    if (solveSudoku(initialGrid)) {
        cout << "Solved Sudoku:" << endl;
        printGrid(initialGrid);
    } else {
        cout << "No solution exists!" << endl;
    }

    return 0;
}
