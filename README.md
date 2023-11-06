# Tic_Tac_Toe

package TicTacToe;  // Package declaration for organization

import java.util.Random;  // Importing necessary Java libraries
import java.util.Scanner;

class TicTacToe
{
	 static char[][] board;  // A 2D array to represent the game board
	
	public TicTacToe()    // Constructor for initializing the game
	{
		board = new char[3][3];  // Create a 3x3 game board
		initBoard();	         // Initialize the board during creation
	}
	
	void initBoard()            // Method for initializing the game board
	{
		for(int i = 0; i < board.length; i++) 
		{
			for(int j = 0; j < board[i].length; j++) 
			{
				board[i][j] = ' ';  // Initialize each cell with empty spaces
			}
		}
	}
	
	static void disBoard()  // Method to display the current game board
	{
		System.out.println("-------------");
		for(int i = 0; i < board.length; i++) 
		{
			System.out.print("| ");
			for(int j = 0; j < board[i].length; j++) 
			{
				System.out.print(board[i][j] + " | ");  // Display the content of each cell
			}
			System.out.println();
			System.out.println("-------------");
		}
	}
	
	static void placeMark(int row, int col, char mark)  // Method to place a player's mark on the board
	{
		if(row >= 0 && row <= 2 && col >= 0 && col <= 2) {
			board[row][col] = mark;  // Place the player's mark if the position is valid
		}
		else {
			System.out.println("Invalid position");
		}
	}
	
	static boolean checkColWin()  // Check for a column-wise win
	{
		for(int j = 0; j <= 2; j++) 
		{
			if (board[0][j] != ' ' && board[0][j] == board[1][j] && board[1][j] == board[2][j]) 
			{
				return true;  // Return true if a player has won in a column
			}
		}
		return false;  // Return false if no player has won in a column
	}
	
	static boolean checkRowWin()  // Check for a row-wise win
	{
		for(int i = 0; i < board.length; i++) {
			if (board[i][0] != ' ' && board[i][0] == board[i][1] && board[i][1] == board[i][2])
			{
				return true;  // Return true if a player has won in a row
			}
		}
		return false;  // Return false if no player has won in a row
	}
	
	static boolean checkDiagWin()  // Check for a diagonal win
	{
		if ((board[0][0] != ' ' && board[0][0] == board[1][1] && board[1][1] == board[2][2]) ||  // Check main diagonal
		    (board[0][2] != ' ' && board[0][2] == board[1][1] && board[1][1] == board[2][0]))  // Check other diagonal
		{
			return true;  // Return true if a player has won diagonally
		}
		else {
			return false;  // Return false if no player has won diagonally
		}
	}
	
	static boolean checkDraw()  // Check for a draw (no empty spaces left)
	{
		for(int i = 0; i <= 2; i++) 
		{
			for(int j = 0; j <= 2; j++) 
			{
				if (board[i][j] == ' ')  // If there is any empty space, the game is not a draw
				{
					return false;
				}
			}
		}
		return true;  // Return true if the game is a draw (no empty spaces left)
	}	
}

abstract class Player
{
	String name;
	char mark;
	
	abstract void makeMove();  // Abstract method for making a move
	
	boolean isValidMove(int row, int col)  // Check if a move is valid
	{
		if (row >= 0 && row <= 2 && col >= 0 && col <= 2)
		{
			if (TicTacToe.board[row][col] == ' ') 
			{
				return true;  // Return true if the move is valid
			}
		}
		return false;  // Return false if the move is not valid
	}
}

class HumanPlayer extends Player
{
	HumanPlayer(String name, char mark)  // Constructor for a human player
	{
		this.name = name;
		this.mark = mark;
	}
	
	void makeMove()  // Implementation of making a move for a human player
	{
		Scanner scan = new Scanner(System.in);
		int row;
		int col;
		do 
		{
			System.out.println("Enter the row and col:");
			row = scan.nextInt();
			col = scan.nextInt();
		}
		while (!isValidMove(row, col));
		
		TicTacToe.placeMark(row, col, mark);  // Place the player's mark on the board
	}
}

class AIPlayer extends Player
{
	AIPlayer(String name, char mark)  // Constructor for an AI player
	{
		this.name = name;
		this.mark = mark;
	}
	
	void makeMove()  // Implementation of making a move for an AI player
	{
		Scanner scan = new Scanner(System.in);
		int row;
		int col;
		do 
		{
			Random r = new Random();  // Create a random number generator
			row = r.nextInt(3);  // Generate a random row
			col = r.nextInt(3);  // Generate a random column
		}
		while (!isValidMove(row, col));
		
		TicTacToe.placeMark(row, col, mark);  // Place the AI player's mark on the board
	}
}

public class LaunchGame {

	public static void main(String[] args) 
	{
		TicTacToe t = new TicTacToe();  // Create a TicTacToe game object
		
		HumanPlayer p1 = new HumanPlayer("Bob", 'X');  // Create a human player named Bob with mark 'X'
		AIPlayer p2 = new AIPlayer("TAI", 'O');  // Create an AI player named TAI with mark 'O'
		
		Player cp;  // Current player
		cp = p1;  // Start the game with player 1 (human player)
		
		while (true)  // Game loop
		{
			System.out.println(cp.name + "'s turn ");
			cp.makeMove();  // Let the current player make a move
			TicTacToe.disBoard();  // Display the updated game board
			
			if (TicTacToe.checkColWin() || TicTacToe.checkRowWin() || TicTacToe.checkDiagWin()) 
			{
				System.out.println(cp.name + " has won! ");  // Check if the current player has won
				break;  // Exit the game loop
			}
			else if (TicTacToe.checkDraw()) 
			{
				System.out.println("The game is a draw.");  // Check if the game is a draw
				break;  // Exit the game loop
			}
			else
			{
				if (cp == p1) 
				{
					cp = p2;  // Switch to the other player for the next turn
				}
				else
				{
					cp = p1;  // Switch back to the first player for the next turn
				}
			}
		}
	}
}

