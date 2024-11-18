# CheckersGame
This is a Checkers Game for final project.
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class CheckersGame 
{
    private JFrame frame;
    private JButton[][] squares;

    public static void main(String[] args) 
    {
     new CheckersGame().startGame(); 
    }

    //Basic layout for this game.
    public void startGame() 
    {
        initializeGUI();
        setupBoard();
     
    }

    private void initializeGUI() 
    {
        frame = new JFrame("Checkers Game");
        frame.setSize(800, 800);

    }

    private void setupBoard() 
    {
        squares = new JButton[8][8];
        
    }

    private void newGame() 
    {
       
    }

    private void resetGame()
    {
            
    }

    private void rematch() 
    {
     
        
    }

    private void pauseGame() 
    {
        
    }
}
