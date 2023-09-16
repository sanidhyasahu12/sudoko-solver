# sudoko-solver


#include <iostream>

class SudokuPuzzle {
private:
    int board[9][9];
    bool debug;

public:
    SudokuPuzzle() {
        debug = false;
        for (int y = 0; y < 9; y++) {
            for (int x = 0; x < 9; x++) {
                board[x][y] = 0;
            }
        }
    }

    void print() {
        for (int y = 0; y < 9; y++) {
            if (y % 3 == 0) {
                std::cout << "-------------------------------" << std::endl;
            }
            for (int x = 0; x < 9; x++) {
                if (x % 3 == 0) {
                    std::cout << "|";
                }
                if (board[x][y] != 0) {
                    std::cout << " " << board[x][y] << " ";
                } else {
                    std::cout << " . ";
                }
            }
            std::cout << "|" << std::endl;
        }
        std::cout << "-------------------------------" << std::endl;
    }

    void setBoardValue(int x_cord, int y_cord, int value) {
        board[x_cord][y_cord] = value;
    }

    bool solve() {
        return solve(0, 0);
    }

    int getBoardValue(int x_cord, int y_cord) {
        return board[x_cord][y_cord];
    }

private:
    bool solve(int x_cord, int y_cord) {
        if (board[x_cord][y_cord] != 0) {
            if (verifyValue(x_cord, y_cord)) {
                if (x_cord == 8 && y_cord == 8) {
                    return true;
                }
                int next_x = x_cord + 1;
                int next_y = y_cord;
                if (next_x >= 9) {
                    next_x = 0;
                    next_y++;
                }
                return solve(next_x, next_y);
            } else {
                return false;
            }
        } else {
            for (int val = 1; val <= 9; val++) {
                setBoardValue(x_cord, y_cord, val);
                if (verifyValue(x_cord, y_cord)) {
                    if (x_cord == 8 && y_cord == 8) {
                        return true;
                    }
                    int next_x = x_cord + 1;
                    int next_y = y_cord;
                    if (next_x >= 9) {
                        next_x = 0;
                        next_y++;
                    }
                    if (solve(next_x, next_y)) {
                        return true;
                    }
                }
            }
            board[x_cord][y_cord] = 0;
            return false;
        }
    }

    bool verifyValue(int x_cord, int y_cord) {
        int value = board[x_cord][y_cord];
        for (int x_verify = 0; x_verify < 9; x_verify++) {
            if (x_verify == x_cord) {
                continue;
            }
            int verifyValue = board[x_verify][y_cord];
            if (verifyValue == value) {
                return false;
            }
        }
        for (int y_verify = 0; y_verify < 9; y_verify++) {
            if (y_verify == y_cord) {
                continue;
            }
            int verifyValue = board[x_cord][y_verify];
            if (verifyValue == value) {
                return false;
            }
        }
        int box_x = x_cord / 3;
        int box_y = y_cord / 3;
        for (int y_verify = box_y * 3; y_verify < box_y * 3 + 3; y_verify++) {
            for (int x_verify = box_x * 3; x_verify < box_x * 3 + 3; x_verify++) {
                if (x_verify == x_cord && y_verify == y_cord) {
                    continue;
                }
                int verifyValue = board[x_verify][y_verify];
                if (verifyValue == value) {
                    return false;
                }
            }
        }
        return true;
    }
};

int main() {
    SudokuPuzzle puzzle;

    // Example Sudoku puzzle (replace with your own)
    puzzle.setBoardValue(0, 0, 5);
    puzzle.setBoardValue(1, 0, 3);
    puzzle.setBoardValue(4, 0, 7);
    puzzle.setBoardValue(0, 1, 6);
    puzzle.setBoardValue(3, 1, 1);
    puzzle.setBoardValue(4, 1, 9);
    puzzle.setBoardValue(5, 1, 5);
    puzzle.setBoardValue(1, 2, 9);
    puzzle.setBoardValue(2, 2, 8);
    puzzle.setBoardValue(7, 2, 6);
    puzzle.setBoardValue(0, 3, 8);
    puzzle.setBoardValue(4, 3, 6);
    puzzle.setBoardValue(8, 3, 3);
    puzzle.setBoardValue(0, 4, 4);
    puzzle.setBoardValue(3, 4, 8);
    puzzle.setBoardValue(5, 4, 3);
    puzzle.setBoardValue(8, 4, 1);
    puzzle.setBoardValue(0, 5, 7);
    puzzle.setBoardValue(4, 5, 2);
    puzzle.setBoardValue(8, 5, 6);
    puzzle.setBoardValue(1, 6, 6);
    puzzle.setBoardValue(6, 6, 2);
    puzzle.setBoardValue(7, 6, 8);
    puzzle.setBoardValue(3, 7, 4);
    puzzle.setBoardValue(4, 7, 1);
    puzzle.setBoardValue(5, 7, 9);
    puzzle.setBoardValue(4, 8, 8);
    puzzle.setBoardValue(7, 8, 5);
    puzzle.setBoardValue(8, 8, 7);

    std::cout << "Sudoku Puzzle:" << std::endl;
    puzzle.print();

    if (puzzle.solve()) {
        std::cout << "Solved Sudoku:" << std::endl;
        puzzle.print();
    } else {
        std::cout << "No solution exists." << std::endl;
    }

    return 0;
}
