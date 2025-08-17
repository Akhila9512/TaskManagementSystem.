# TaskManagementSystem.
//This program is about javaGUI through VScode.
import javax.swing.*;
import java.awt.*;

// Node class
class TaskNode {
    String taskName;
    int priority;
    TaskNode next, prev;

    TaskNode(String name, int priority) {
        this.taskName = name;
        this.priority = priority;
        this.next = null;
        this.prev = null;
    }
}

// Task list (Doubly Linked List)
class DoublyTaskList {
    private TaskNode head;

    void addTask(String name, int priority) {
        TaskNode newNode = new TaskNode(name, priority);
        if (head == null) {
            head = newNode;
        } else {
            TaskNode temp = head;
            while (temp.next != null) {
                temp = temp.next;
            }
            temp.next = newNode;
            newNode.prev = temp;
        }
    }

    void deleteTask(String name) {
        if (head == null) return;
        TaskNode temp = head;
        while (temp != null && !temp.taskName.equalsIgnoreCase(name)) {
            temp = temp.next;
        }
        if (temp == null) return;
        if (temp.prev != null) temp.prev.next = temp.next;
        else head = temp.next;
        if (temp.next != null) temp.next.prev = temp.prev;
    }

    String showTasks() {
        StringBuilder sb = new StringBuilder("Your Tasks:\n\n");
        TaskNode temp = head;
        if (temp == null) {
            return "No tasks available.\n";
        }
        while (temp != null) {
            sb.append("").append(temp.taskName)
              .append("   |   Priority: ").append(temp.priority).append("\n");
            temp = temp.next;
        }
        return sb.toString();
    }
}

// GUI Class
public class Akhila extends JFrame {
    private DoublyTaskList taskList = new DoublyTaskList();
    private JTextArea taskArea;
    private JTextField taskField, priorityField, deleteField;

    public Akhila() {
        setTitle("Task Management System ");
        setSize(650, 500);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        //  Gradient Background (Custom Panel)
        JPanel gradientPanel = new JPanel() {
            @Override
            protected void paintComponent(Graphics g) {
                super.paintComponent(g);
                Graphics2D g2d = (Graphics2D) g;
                Color color1 = new Color(135, 206, 250); // Sky blue
                Color color2 = new Color(255, 182, 193); // Pink
                GradientPaint gp = new GradientPaint(0, 0, color1, 0, getHeight(), color2);
                g2d.setPaint(gp);
                g2d.fillRect(0, 0, getWidth(), getHeight());
            }
        };
        gradientPanel.setLayout(new BorderLayout());
        setContentPane(gradientPanel);

        // --- Top Panel (Add task) ---
        JPanel topPanel = new JPanel(new FlowLayout());
        topPanel.setBackground(new Color(255, 239, 213)); // Light Peach

        JLabel lblTask = new JLabel("Task:");
        lblTask.setFont(new Font("Arial", Font.BOLD, 14));
        taskField = new JTextField(12);

        JLabel lblPriority = new JLabel("Priority:");
        lblPriority.setFont(new Font("Arial", Font.BOLD, 14));
        priorityField = new JTextField(4);

        JButton addButton = new JButton("➕ Add Task");
        addButton.setBackground(new Color(60, 179, 113)); // Medium Sea Green
        addButton.setForeground(Color.WHITE);
        addButton.setFocusPainted(false);
        addButton.setFont(new Font("Arial", Font.BOLD, 13));

        topPanel.add(lblTask);
        topPanel.add(taskField);
        topPanel.add(lblPriority);
        topPanel.add(priorityField);
        topPanel.add(addButton);

        // --- Middle (Task display area) ---
        taskArea = new JTextArea();
        taskArea.setEditable(false);
        taskArea.setFont(new Font("Consolas", Font.PLAIN, 14));
        taskArea.setBackground(new Color(255, 250, 240)); // Floral White
        taskArea.setForeground(new Color(75, 0, 130));   // Indigo
        taskArea.setBorder(BorderFactory.createLineBorder(new Color(100, 149, 237), 2)); // Cornflower Blue
        JScrollPane scrollPane = new JScrollPane(taskArea);

        // --- Bottom Panel (Delete task) ---
        JPanel bottomPanel = new JPanel(new FlowLayout());
        bottomPanel.setBackground(new Color(230, 230, 250)); // Lavender

        JLabel lblDelete = new JLabel("Task to Delete:");
        lblDelete.setFont(new Font("Arial", Font.BOLD, 14));

        deleteField = new JTextField(12);

        JButton deleteButton = new JButton(" Delete Task");
        deleteButton.setBackground(new Color(220, 20, 60)); // Crimson
        deleteButton.setForeground(Color.WHITE);
        deleteButton.setFocusPainted(false);
        deleteButton.setFont(new Font("Arial", Font.BOLD, 13));

        bottomPanel.add(lblDelete);
        bottomPanel.add(deleteField);
        bottomPanel.add(deleteButton);

        // --- Add panels to frame ---
        add(topPanel, BorderLayout.NORTH);
        add(scrollPane, BorderLayout.CENTER);
        add(bottomPanel, BorderLayout.SOUTH);

        // --- Button Actions ---
        addButton.addActionListener(e -> {
            String task = taskField.getText().trim();
            String pr = priorityField.getText().trim();
            if (!task.isEmpty() && !pr.isEmpty()) {
                try {
                    int priority = Integer.parseInt(pr);
                    taskList.addTask(task, priority);
                    taskField.setText("");
                    priorityField.setText("");
                    refreshTasks();
                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(this, "⚠ Priority must be a number.");
                }
            } else {
                JOptionPane.showMessageDialog(this, "⚠ Enter both task and priority.");
            }
        });

        deleteButton.addActionListener(e -> {
            String task = deleteField.getText().trim();
            if (!task.isEmpty()) {
                taskList.deleteTask(task);
                deleteField.setText("");
                refreshTasks();
            }
        });

        //  Add some default tasks
        taskList.addTask("Prepare Report", 1);
        taskList.addTask("Check Emails", 2);
        taskList.addTask("Team Meeting", 3);
        refreshTasks();
    }

    private void refreshTasks() {
        taskArea.setText(taskList.showTasks());
    }

    //  Correct main method
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            new Akhila().setVisible(true);
        });
    }
}



