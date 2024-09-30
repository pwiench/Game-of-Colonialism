import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.HashMap;
import java.util.Map;
import java.util.Random;

public class GameOfColonies extends JPanel implements ActionListener {

    private static final int WIDTH = 1200; // Window width
    private static final int HEIGHT = 800; // Window height
    private static final int CELL_SIZE = 4; // Size of each cell
    private static final int COLS = WIDTH / CELL_SIZE; // Number of columns
    private static final int ROWS = HEIGHT / CELL_SIZE; // Number of rows
    private static final int INITIAL_COLONIES = 10; // Number of initial colonies
    private static final int GENERATION_INTERVAL = 200; // Time between generations (ms)

    private int[][] grid = new int[COLS][ROWS]; // Grid for cell states
    private Color[][] colors = new Color[COLS][ROWS]; // Color array for each cell
    private Random random = new Random(); // Random generator for initial colonies
    private javax.swing.Timer timer; // Timer to handle periodic updates
    private int generations = 0; // Generation counter
    private int livingCells = 0; // Counter for total living cells

    private JButton exitButton;
	private JButton resetButton;


    public GameOfColonies() {
        setPreferredSize(new Dimension(WIDTH, HEIGHT)); // Set size of the panel
        setBackground(Color.WHITE); // Set background color
        initializeColonies(); // Initialize the first colonies
        timer = new javax.swing.Timer(GENERATION_INTERVAL, this); // Set timer for periodic updates
        timer.start(); // Start the timer

        exitButton = new JButton("Exit");
        exitButton.addActionListener(e -> System.exit(0));
        setLayout(new BorderLayout());
        add(exitButton, BorderLayout.SOUTH);
		
		resetButton = new JButton("Reset");
        resetButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                resetGame();
            }
        });
        add(resetButton, BorderLayout.SOUTH);   

    }

    private void resetGame() {
        generations = 0;
        livingCells = 0;
        for (int x = 0; x < COLS; x++) {
            for (int y = 0; y < ROWS; y++) {
                grid[x][y] = 0;
                colors[x][y] = null;
            }
        }
        initializeColonies();
        repaint();
    }

    }

    private void initializeColonies() {
        for (int i = 0; i < INITIAL_COLONIES; i++) {
            int x = random.nextInt(COLS); // Random x-coordinate
            int y = random.nextInt(ROWS); // Random y-coordinate
            Color color = new Color(random.nextInt(256), random.nextInt(256), random.nextInt(256)); // Random color
            grid[x][y] = 1; // Mark cell as alive
            colors[x][y] = color; // Assign random color to the living cell
            // Initialize neighboring cells with the same color
            for (int dx = -1; dx <= 1; dx++) {
                for (int dy = -1; dy <= 1; dy++) {
                    int newX = (x + dx + COLS) % COLS;
                    int newY = (y + dy + ROWS) % ROWS;
                    if (grid[newX][newY] == 0 && random.nextInt(2) == 0) { // Randomly initialize neighboring cells
                        grid[newX][newY] = 1;
                        colors[newX][newY] = color;
                    }
                }
            }
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        updateColonies(); // Update the state of the grid based on Game of Life rules
        repaint(); // Repaint the grid to reflect the new state
        generations++; // Increment generation counter
    }

    private void updateColonies() {
        int[][] newGrid = new int[COLS][ROWS]; // Create new grid for next generation
        Color[][] newColors = new Color[COLS][ROWS]; // New color array for next generation
        livingCells = 0; // Reset the living cells counter for this generation

        // Traverse every cell in the grid
        for (int x = 0; x < COLS; x++) {
            for (int y = 0; y < ROWS; y++) {
                int aliveNeighbors = countAliveNeighbors(x, y); // Count alive neighbors

                // Game of Life rules
                if (grid[x][y] == 1) { // If cell is alive
                    if (aliveNeighbors < 2 || aliveNeighbors > 3) {
                        newGrid[x][y] = 0;
                        newColors[x][y] = null;
                    } else {
                        newGrid[x][y] = 1;
                        newColors[x][y] = colors[x][y];
                        livingCells++;
                    }
                } else { // If cell is dead (potential for growth)
                    if (aliveNeighbors == 2) {
                        newGrid[x][y] = 1;
                        newColors[x][y] = getDominantNeighborColor(x, y);
                        livingCells++;
                    }
                }
            }
        }

        grid = newGrid;
        colors = newColors;
    }

    private int countAliveNeighbors(int x, int y) {
        int count = 0;
        for (int dx = -1; dx <= 1; dx++) {
            for (int dy = -1; dy <= 1; dy++) {
                if (dx != 0 || dy != 0) {
                    int newX = (x + dx + COLS) % COLS; // Wrap around horizontally
                    int newY = (y + dy + ROWS) % ROWS; // Wrap around vertically
                    if (grid[newX][newY] == 1) {
                        count++;
                    }
                }
            }
        }
        return count;
    }

    private Color getDominantNeighborColor(int x, int y) {
        Map<Color, Integer> colorCount = new HashMap<>();

        for (int dx = -1; dx <= 1; dx++) {
            for (int dy = -1; dy <= 1; dy++) {
                if (dx != 0 || dy != 0) {
                    int newX = (x + dx + COLS) % COLS;
                    int newY = (y + dy + ROWS) % ROWS;
                    if (grid[newX][newY] == 1) {
                        Color neighborColor = colors[newX][newY];
                        colorCount.put(neighborColor, colorCount.getOrDefault(neighborColor, 0) + 1);
                    }
                }
            }
        }

        return colorCount.entrySet().stream()
            .max(Map.Entry.comparingByValue())
            .map(Map.Entry::getKey)
            .orElse(Color.WHITE);
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);
        for (int x = 0; x < COLS; x++) {
            for (int y = 0; y < ROWS; y++) {
                if (grid[x][y] == 1) {
                    g.setColor(colors[x][y]);
                    g.fillRect(x * CELL_SIZE, y * CELL_SIZE, CELL_SIZE, CELL_SIZE);
                } else {
                    g.setColor(Color.WHITE);
                    g.fillRect(x * CELL_SIZE, y * CELL_SIZE, CELL_SIZE, CELL_SIZE);
                }
            }
        }

        // Draw grid lines
        g.setColor(Color.WHITE);
        for (int x = 1; x < COLS; x++) {
            g.drawLine(x * CELL_SIZE, 0, x * CELL_SIZE, HEIGHT);
        }
        for (int y = 1; y < ROWS; y++) {
            g.drawLine(0, y * CELL_SIZE, WIDTH, y * CELL_SIZE);
        }

        // Draw generation count and living cells count below the exit button
        g.setColor(Color.BLACK);
        g.setFont(new Font("Arial", Font.BOLD, 12));
        g.drawString("Generation: " + generations, 10, exitButton.getY() - 15);
        g.drawString("Living Cells: " + livingCells, 10, exitButton.getY() - 5);
    }

    public static void main(String[] args) {
        JFrame frame = new JFrame("Game of Colonies");
        GameOfColonies game = new GameOfColonies();
        frame.add(game);
        frame.setExtendedState(JFrame.MAXIMIZED_BOTH);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
