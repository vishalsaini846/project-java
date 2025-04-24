import java.awt.*;
import java.awt.event.*;
import java.sql.*;

public class RegistrationForm extends Frame {
    private TextField usernameField, passwordField, emailField, nameField;
    private Button submitButton, viewButton;
    private TextArea detailsArea;

    private static final String DB_URL = "jdbc:mysql://localhost:3306/user_registration";
    private static final String DB_USER = "root";
    private static final String DB_PASS = "password";

    public RegistrationForm() {
        setTitle("User Registration");
        setSize(500, 400);
        setLayout(new BorderLayout());

        Panel inputPanel = new Panel(new GridLayout(5, 2));
        inputPanel.add(new Label("Username:"));
        usernameField = new TextField();
        inputPanel.add(usernameField);
        
        inputPanel.add(new Label("Password:"));
        passwordField = new TextField();
        passwordField.setEchoChar('*');
        inputPanel.add(passwordField);
        
        inputPanel.add(new Label("Email:"));
        emailField = new TextField();
        inputPanel.add(emailField);
        
        inputPanel.add(new Label("Full Name:"));
        nameField = new TextField();
        inputPanel.add(nameField);

        Panel buttonPanel = new Panel();
        submitButton = new Button("Register");
        viewButton = new Button("View Users");
        buttonPanel.add(submitButton);
        buttonPanel.add(viewButton);

        detailsArea = new TextArea();
        detailsArea.setEditable(false);

        add(inputPanel, BorderLayout.NORTH);
        add(buttonPanel, BorderLayout.CENTER);
        add(detailsArea, BorderLayout.SOUTH);

        submitButton.addActionListener(e -> registerUser());
        viewButton.addActionListener(e -> showUsers());

        addWindowListener(new WindowAdapter() {
            public void windowClosing(WindowEvent e) {
                dispose();
            }
        });
    }

    private void registerUser() {
        try {
            Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASS);
            String sql = "INSERT INTO users (username, password, email, full_name) VALUES (?, ?, ?, ?)";
            PreparedStatement pstmt = conn.prepareStatement(sql);
            
            pstmt.setString(1, usernameField.getText());
            pstmt.setString(2, passwordField.getText());
            pstmt.setString(3, emailField.getText());
            pstmt.setString(4, nameField.getText());
            
            pstmt.executeUpdate();
            pstmt.close();
            conn.close();
            
            clearFields();
            showUsers();
        } catch (SQLException e) {
            detailsArea.setText("Error: " + e.getMessage());
        }
    }

    private void showUsers() {
        try {
            Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASS);
            Statement stmt = conn.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM users");
            
            StringBuilder sb = new StringBuilder();
            while (rs.next()) {
                sb.append("ID: ").append(rs.getInt("id"))
                  .append(", Name: ").append(rs.getString("full_name"))
                  .append(", Username: ").append(rs.getString("username"))
                  .append(", Email: ").append(rs.getString("email"))
                  .append("\n");
            }
            
            detailsArea.setText(sb.toString());
            rs.close();
            stmt.close();
            conn.close();
        } catch (SQLException e) {
            detailsArea.setText("Error: " + e.getMessage());
        }
    }

    private void clearFields() {
        usernameField.setText("");
        passwordField.setText("");
        emailField.setText("");
        nameField.setText("");
    }

    public static void main(String[] args) {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            RegistrationForm form = new RegistrationForm();
            form.setVisible(true);
        } catch (ClassNotFoundException e) {
            System.out.println("MySQL JDBC Driver not found");
        }
    }
}
