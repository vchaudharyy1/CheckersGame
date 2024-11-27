# CheckersGame
This is a Checkers Game for final project.
This is just the basic layout for the game. I will be editing this.
```java

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.awt.image.*;

public class CheckersGame extends JFrame
{
    protected static final int gameSize = 8;
    static final Color Lght_Clr = new Color(240, 240, 201);
    static final Color DrkClr = new Color(118, 150, 86);
    private static final Color RedPce = Color.RED;
    private static final Color BlackPece = Color.BLACK;

    private JButton[][] boardButtons = new JButton[gameSize][gameSize];
    private boolean isBlackTurn = true;
    private Point selectedPiece = null;
    private boolean isChainCapture = false;

    private JLabel turnLabel;

    public CheckersGame() {
        setTitle("Checkers Game");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        //creating the board.
        JPanel boardPanel = new JPanel(new GridLayout(gameSize, gameSize));
        board_initializ(boardPanel);

        //adding the control buttons.
        JPanel controlPanel = new JPanel();
        JButton newGamBtn = new JButton("New Game");
        JButton pauseButton = new JButton("Pause");
        JButton rematchButton = new JButton("Rematch");

        newGamBtn.addActionListener(e -> resetBoard());
        pauseButton.addActionListener(e -> JOptionPane.showMessageDialog(this, "Game Paused!"));
        rematchButton.addActionListener(e -> resetBoard());

        controlPanel.add(newGamBtn);
        controlPanel.add(pauseButton);
        controlPanel.add(rematchButton);

        turnLabel = new JLabel("Turn: Black");
        turnLabel.setFont(new Font("Arial", Font.BOLD, 16));
        controlPanel.add(turnLabel);

        add(boardPanel, BorderLayout.CENTER);
        add(controlPanel, BorderLayout.SOUTH);

        setSize(800, 800);
        setLocationRelativeTo(null);
        resetBoard();
        setVisible(true);
    }

    private void board_initializ()
    {
        
    }

    private void resetGame()
    {
            
    }

    private Icon iconcreate_Pieces(Color color)
    {
        BufferedImage image = new BufferedImage(50, 50, BufferedImage.TYPE_INT_ARGB);
        Graphics2D g2 = image.createGraphics();
        g2.setColor(color);
        g2.fillOval(5, 5, 40, 40);
        g2.dispose();
        return new ImageIcon(image);
    }

    private Icon createKingIcon(Color color)
    {
        BufferedImage image = new BufferedImage(50, 50, BufferedImage.TYPE_INT_ARGB);
        Graphics2D g2 = image.createGraphics();
        g2.setColor(color);
        g2.fillOval(5, 5, 40, 40);
        g2.setColor(Color.WHITE);
        g2.setStroke(new BasicStroke(3));
        g2.drawOval(10, 10, 30, 30);
        g2.dispose();
        return new ImageIcon(image);
    }

    private void handleMove(ActionEvent e)
    {

    }

    private void makeMove
    {

    }

    private Color getPieceColor 
   {

   }

   private boolean ifIsValidMoves
   {

   }

   public static void main(String[] args)
   {
        SwingUtilities.invokeLater(CheckersGame::new);
   }
}
