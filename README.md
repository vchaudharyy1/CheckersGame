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

    public CheckersGame()
    {
        setTitle("Checkers Game");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        //creating the board.
        JPanel boardPanel = new JPanel(new GridLayout(gameSize, gameSize));
        board_initializ(boardPanel);

        // Add control buttons
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

    private void board_initializ(JPanel boardPanel) 
    {
        for (int game_Row = 0; game_Row < gameSize; game_Row++) 
        {
            for (int col = 0; col < gameSize; col++) {
                JButton button = new JButton();
                button.setBackground((game_Row + col) % 2 == 0 ? Lght_Clr : DrkClr);
                button.setBorderPainted(false);
                button.setOpaque(true);
                button.setActionCommand(game_Row + "," + col);
                button.addActionListener(this::handleMove);
                boardButtons[game_Row][col] = button;
                boardPanel.add(button);
            }
        }
    }

    private void resetBoard()
    {
        selectedPiece = null;
        isBlackTurn = true;
        isChainCapture = false;
        turnLabel.setText("Turn: Black");

        for (int row = 0; row < gameSize; row++)
        {
            for (int col = 0; col < gameSize; col++) 
            {
                JButton button = boardButtons[row][col];
                button.setIcon(null);

                if (row < 3 && (row + col) % 2 == 1) 
                {
                    button.setIcon(iconcreate_Pieces(RedPce));
                } else if (row > 4 && (row + col) % 2 == 1)
                {
                    button.setIcon(iconcreate_Pieces(BlackPece));
                }
            }
        }
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
        String[] pos = e.getActionCommand().split(",");
        int row = Integer.parseInt(pos[0]);
        int col = Integer.parseInt(pos[1]);
        JButton button = boardButtons[row][col];

        if (selectedPiece == null) 
        {
            if (button.getIcon() != null && isPlayerPiece(row, col)) 
            {
                selectedPiece = new Point(row, col);
                button.setBorder(BorderFactory.createLineBorder(Color.YELLOW, 2));
            }
        } else 
        {
            Point to = new Point(row, col);
            JButton fromButton = boardButtons[selectedPiece.x][selectedPiece.y];

            if (ifIsValidMoves(selectedPiece, to)) 
            {
                makeMove(selectedPiece, to);
                if (!isChainCapture) 
                {
                    isBlackTurn = !isBlackTurn;
                    turnLabel.setText(isBlackTurn ? "Turn: Black" : "Turn: Red");
                }
                selectedPiece = null;
            } else 
            {
                JOptionPane.showMessageDialog(this, "Invalid move! Mandatory captures must be made.");
                fromButton.setBorder(null);
                selectedPiece = null;
            }
        }
    }

    private boolean isPlayerPiece(int row, int col)
    {
        Color pieceColor = getPieceColor(boardButtons[row][col]);
        return (isBlackTurn && pieceColor.equals(BlackPece)) || (!isBlackTurn && pieceColor.equals(RedPce));
    }

    private void makeMove(Point from, Point to)
    {
        JButton fromButton = boardButtons[from.x][from.y];
        JButton toButton = boardButtons[to.x][to.y];
        toButton.setIcon(fromButton.getIcon());
        fromButton.setIcon(null);
        fromButton.setBorder(null);

        //this if for handling the captures.
        if (Math.abs(from.x - to.x) == 2) 
        {
            int midX = (from.x + to.x) / 2;
            int midY = (from.y + to.y) / 2;
            boardButtons[midX][midY].setIcon(null);
            isChainCapture = canCapture(to);
        } else 
        {
            isChainCapture = false;
        }

        //this is for handling king.
        if ((isBlackTurn && to.x == 0) || (!isBlackTurn && to.x == gameSize - 1)) 
        {
            toButton.setIcon(createKingIcon(getPieceColor(toButton)));
        }

        if (!isChainCapture)
        {
            checkForWin();
        }
    }

    private Color getPieceColor(JButton button) 
    {
        if (button.getIcon() != null)
        {
            BufferedImage bi = (BufferedImage) ((ImageIcon) button.getIcon()).getImage();
            return new Color(bi.getRGB(25, 25));
        }
        return null;
    }

    private boolean ifIsValidMoves(Point from, Point to) 
    {
        if (to.x < 0 || to.x >= gameSize || to.y < 0 || to.y >= gameSize) 
        {
            return false;
        }
        if (boardButtons[to.x][to.y].getIcon() != null) 
        {
            return false;
        }

        int dx = Math.abs(to.x - from.x);
        int dy = Math.abs(to.y - from.y);
        boolean isForward = (isBlackTurn && to.x < from.x) || (!isBlackTurn && to.x > from.x);
        boolean isKing = isKing(boardButtons[from.x][from.y]);

        if (dx == 1 && dy == 1 && (isForward || isKing)) 
        {
            return true;
        }

        if (dx == 2 && dy == 2) 
        {
            int midX = (from.x + to.x) / 2;
            int midY = (from.y + to.y) / 2;
            JButton middleButton = boardButtons[midX][midY];
            Color midColor = getPieceColor(middleButton);
            Color fromColor = getPieceColor(boardButtons[from.x][from.y]);

            return middleButton.getIcon() != null && !midColor.equals(fromColor);
        }

        return false;
    }

    private boolean isKing(JButton button)
    {
        return button.getIcon() != null && ((ImageIcon) button.getIcon()).getDescription() != null;
    }

    private boolean canCapture(Point from) 
    {
        int[][] directions = {{-2, -2}, {-2, 2}, {2, -2}, {2, 2}};
        for (int[] dir : directions) {
            Point to = new Point(from.x + dir[0], from.y + dir[1]);
            if (ifIsValidMoves(from, to)) {
                return true;
            }
        }
        return false;
    }

    private void checkForWin() 
    {
        boolean redExists = false;
        boolean blackExists = false;

        for (int row = 0; row < gameSize; row++) 
        {
            for (int col = 0; col < gameSize; col++) 
            {
                JButton button = boardButtons[row][col];
                Color pieceColor = getPieceColor(button);
                if (pieceColor != null) {
                    if (pieceColor.equals(RedPce))
                    {
                        redExists = true;
                    } else if (pieceColor.equals(BlackPece))
                    {
                        blackExists = true;
                    }
                }
            }
        }

        if (!redExists || !blackExists)
        {
            String winner = blackExists ? "Black" : "Red";
            JOptionPane.showMessageDialog(this, winner + " wins!");
            resetBoard();
        }
    }

    public static void main(String[] args)
    {
        SwingUtilities.invokeLater(CheckersGame::new);
    }
}
