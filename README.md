import java.util.Scanner;
import java.util.Random;

/**
 * Tic Tac Toe Honors Java Group Project 2016
 * Main Editor, Ian Rothermel
 * Colin Messanger, Alec Roth  
 */
public class TicTacToe1 {
public class RNGPlacement {

   Random plot = new Random();
   int RNGplace = plot.nextInt(3) + 1;

}


   // consts to represent the plots and cell contents
   public static final int EMPTY = 0;
   public static final int X = 1;
   public static final int O = 2;
 
   // consts to represent the various states of the game
   public static final int PLAYING = 0;
   public static final int DRAW = 1;
   public static final int X_WON = 2;
   public static final int O_WON = 3;
 
   // The game board and the game status
   public static final int ROWS = 3, COLS = 3;         // number of rows and columns
   public static int[][] board = new int[ROWS][COLS];  // game board in 2D array
                                                       //  containing (EMPTY, X, O)
   public static int currentState;                     // the current state of the game
                                                       // (PLAYING, DRAW, X_WON, O_WON)
   public static int currentPlayer;                    // the current player (X or O)
   public static int currentRow, currentCol;           // current plots's row and column
 
   public static Scanner in = new Scanner(System.in);  // the input Scanner
 
   // Program starts
   public static void main(String[] args) {
      
      initGame();
      
      // Play the game 
      do {
         playerMove(currentPlayer);                              // update currentRow and currentCol
         updateGame(currentPlayer, currentRow, currentCol);      // update currentState
         printBoard();
         // Print message if game-over
         if (currentState == X_WON) {
            System.out.println("'X' won! Bye!");
         } else if (currentState == O_WON) {
            System.out.println("'O' won! Bye!");
         } else if (currentState == DRAW) {
            System.out.println("It's a Draw! Bye!");
         }
         // Switch player
         currentPlayer = (currentPlayer == X) ? O : X;
      } while (currentState == PLAYING);          // repeat if not game-over
   }
 
   // Initialize the game-board contents and the current states
   public static void initGame() {
      for (int row = 0; row < ROWS; ++row) {
         for (int col = 0; col < COLS; ++col) {
            board[row][col] = EMPTY;              // all cells empty
         }
      }
      currentState = PLAYING;            // ready to play
      currentPlayer = X;                 // cross plays first
   }
 


   public static void playerMove(int thePlot) {
      boolean validInput = false;     // for input validation
      do {
         if (thePlot == X) {
            System.out.print("Player 'X', enter your move (row col): ");
         } else {
            System.out.print("Player 'O', enter your move (row col): ");
         }
         int row = in.nextInt() - 1;  // array index starts at 0 instead of 1
         int col = in.nextInt() - 1;
         if (row >= 0 && row < ROWS && col >= 0 && col < COLS && board[row][col] == EMPTY) {
            currentRow = row;
            currentCol = col;
            board[currentRow][currentCol] = thePlot;  // update game-board content
            validInput = true;                      
         } else {
            System.out.println("This move at (" + (row + 1) + "," + (col + 1) + ") is an invalid move!");
         }
      } while (!validInput);  // repeat until input is valid
   }
 
   // Update the "currentState" after the player with "thePlot" has placed on (currentRow, currentCol).
   
   public static void updateGame(int thePlot, int currentRow, int currentCol) {
      if (hasWon(thePlot, currentRow, currentCol)) {     // check if winning move
         currentState = (thePlot == X) ? X_WON : O_WON;
      } else if (isDraw()) {                             // check for draw
         currentState = DRAW;
      }
      // Otherwise, no change to currentState (still PLAYING).
   }
 
   // Return true if it is a draw (no more empty cell)

   public static boolean isDraw() {
      for (int row = 0; row < ROWS; ++row) {
         for (int col = 0; col < COLS; ++col) {
            if (board[row][col] == EMPTY) {
               return false;  // an empty cell found, not draw, exit
            }
         }
      }
      return true;            // no empty cell, it's a draw
   }
 
   // Return true if the player with "thePlot" has won after placing
   public static boolean hasWon(int thePlot, int currentRow, int currentCol) {
      return (board[currentRow][0] == thePlot             // 3-in-the-row
                   && board[currentRow][1] == thePlot
                   && board[currentRow][2] == thePlot
                   
                   || board[0][currentCol] == thePlot     // 3-in-the-column
                   && board[1][currentCol] == thePlot
                   && board[2][currentCol] == thePlot
                   
                   || currentRow == currentCol            // 3-in-the-diagonal
                   && board[0][0] == thePlot
                   && board[1][1] == thePlot
                   && board[2][2] == thePlot
                   
                   || currentRow + currentCol == 2        // 3-in-the-opposite-diagonal
                   && board[0][2] == thePlot
                   && board[1][1] == thePlot
                   && board[2][0] == thePlot);
   }
 
   /** Print the game board */
   public static void printBoard() {
      for (int row = 0; row < ROWS; ++row) {
         for (int col = 0; col < COLS; ++col) {
            printCell(board[row][col]); // print each of the cells
            if (col != COLS - 1) {
               System.out.print("|");   // print vertical lines
            }
         }
         System.out.println();
         if (row != ROWS - 1) {
            System.out.println("-----------"); // print horizontal lines
         }
      }
      System.out.println();
   }
 
   // Print a cell with the specified letter
   public static void printCell(int letter) {
      switch (letter) {
         case EMPTY:  System.out.print("   "); break;
         case O: System.out.print(" O "); break;
         case X:  System.out.print(" X "); break;
      }
   }
}
