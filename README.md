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
        StringBuilder sb = new StringBuilder();
        TaskNode temp = head;
        if (temp == null) {
            return "✨ No tasks available.\n";
        }
        while (temp != null) {
            sb.append("📝 Task: ").append(temp.taskName)
              .append("   |   Priority: ").append(temp.priority).append("\n");
            temp = temp.next;
        }
        return sb.toString();
    }
}

// GUI Class
public class AkhilaMenu extends JFrame {
    private DoublyTaskList taskList = new DoublyTaskList();
    private JTextArea taskArea;
    private JTextField taskField, priorityField, deleteField;

    public AkhilaMenu() {
        setTitle("🌟 Task Management System 🌟");
        setSize(600, 450);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // 🎨 Main background
        getContentPane().setBackground(new Color(245, 245, 255));

        // --- Top Panel (Add task) ---
        JPanel topPanel = new JPanel(new FlowLayout());
        topPanel.setBackground(new Color(200, 220, 255));

        JLabel lblTask = new JLabel("Task:");
        lblTask.setFont(new Font("Arial", Font.BOLD, 14));
        taskField = new JTextField(12);

        JLabel lblPriority = new JLabel("Priority:");
        lblPriority.setFont(new Font("Arial", Font.BOLD, 14));
        priorityField = new JTextField(4);

        JButton addButton = new JButton("➕ Add Task");
        addButton.setBackground(new Color(50, 205, 50));
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
        taskArea.setBackground(new Color(250, 250, 250));
        taskArea.setForeground(new Color(30, 30, 60));
        JScrollPane scrollPane = new JScrollPane(taskArea);

        // --- Bottom Panel (Delete task) ---
        JPanel bottomPanel = new JPanel(new FlowLayout());
        bottomPanel.setBackground(new Color(200, 220, 255));

        JLabel lblDelete = new JLabel("Task to Delete:");
        lblDelete.setFont(new Font("Arial", Font.BOLD, 14));

        deleteField = new JTextField(12);

        JButton deleteButton = new JButton("🗑 Delete Task");
        deleteButton.setBackground(new Color(220, 20, 60));
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

        // 📌 Add some default tasks
        taskList.addTask("Prepare Report", 1);
        taskList.addTask("Check Emails", 2);
        taskList.addTask("Team Meeting", 3);
        refreshTasks();
    }

    private void refreshTasks() {
        taskArea.setText(taskList.showTasks());
    }

    // ✅ Correct main method
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            new AkhilaMenu().setVisible(true);
        });
    }
}


