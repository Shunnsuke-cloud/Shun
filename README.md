import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.*;
import java.util.Properties;

public class CalorieManagementApp extends JFrame {
    private static final String USER_PROPERTIES_FILE = "user.properties";
    private Properties properties = new Properties();

    public CalorieManagementApp() {
        setTitle("カロリー管理アプリ");
        setSize(600, 400);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // メインメニュー
        JPanel mainMenuPanel = new JPanel();
        mainMenuPanel.setLayout(new GridLayout(4, 1, 10, 10));

        JButton registerButton = new JButton("新規登録");
        JButton foodRegisterButton = new JButton("食品登録");
        JButton mealRecordButton = new JButton("食事記録");
        JButton goalSettingButton = new JButton("目標設定");

        registerButton.addActionListener(e -> showRegistrationScreen());
        foodRegisterButton.addActionListener(e -> showFoodRegistrationScreen());
        mealRecordButton.addActionListener(e -> showMealRecordScreen());
        goalSettingButton.addActionListener(e -> showGoalSettingScreen());

        mainMenuPanel.add(registerButton);
        mainMenuPanel.add(foodRegisterButton);
        mainMenuPanel.add(mealRecordButton);
        mainMenuPanel.add(goalSettingButton);

        add(mainMenuPanel, BorderLayout.CENTER);
        setVisible(true);
    }

    private void showRegistrationScreen() {
        JFrame registrationFrame = new JFrame("新規登録");
        registrationFrame.setSize(400, 300);
        registrationFrame.setLayout(new GridLayout(4, 2, 10, 10));

        registrationFrame.add(new JLabel("ユーザー名:"));
        JTextField nameField = new JTextField();
        registrationFrame.add(nameField);

        registrationFrame.add(new JLabel("メールアドレス:"));
        JTextField emailField = new JTextField();
        registrationFrame.add(emailField);

        registrationFrame.add(new JLabel("パスワード:"));
        JPasswordField passwordField = new JPasswordField();
        registrationFrame.add(passwordField);

        JButton registerButton = new JButton("登録");
        registerButton.addActionListener(e -> {
            String username = nameField.getText();
            String email = emailField.getText();
            String password = new String(passwordField.getPassword());

            if (validateRegistration(username, email, password)) {
                saveUserDetails(username, email, password);
                JOptionPane.showMessageDialog(registrationFrame, "登録が完了しました。");
                registrationFrame.dispose();
            } else {
                JOptionPane.showMessageDialog(registrationFrame, "入力データが不正です。");
            }
        });

        registrationFrame.add(registerButton);
        registrationFrame.setVisible(true);
    }

    private boolean validateRegistration(String username, String email, String password) {
        return !username.isEmpty() && email.contains("@") && password.length() >= 6;
    }

    private void saveUserDetails(String username, String email, String password) {
        properties.setProperty("username", username);
        properties.setProperty("email", email);
        properties.setProperty("password", password);
        try (FileOutputStream out = new FileOutputStream(USER_PROPERTIES_FILE)) {
            properties.store(out, "User Information");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private void showFoodRegistrationScreen() {
        JFrame foodFrame = new JFrame("食品登録");
        foodFrame.setSize(400, 300);
        foodFrame.setLayout(new GridLayout(4, 2, 10, 10));

        foodFrame.add(new JLabel("食品名:"));
        JTextField foodNameField = new JTextField();
        foodFrame.add(foodNameField);

        foodFrame.add(new JLabel("カロリー:"));
        JTextField calorieField = new JTextField();
        foodFrame.add(calorieField);

        foodFrame.add(new JLabel("栄養素:"));
        JTextField nutrientField = new JTextField();
        foodFrame.add(nutrientField);

        JButton registerFoodButton = new JButton("登録");
        registerFoodButton.addActionListener(e -> {
            String foodName = foodNameField.getText();
            String calorie = calorieField.getText();
            String nutrients = nutrientField.getText();

            if (!foodName.isEmpty() && !calorie.isEmpty() && !nutrients.isEmpty()) {
                // 食品情報を保存する処理を追加
                JOptionPane.showMessageDialog(foodFrame, "食品が登録されました。");
                foodFrame.dispose();
            } else {
                JOptionPane.showMessageDialog(foodFrame, "すべてのフィールドを入力してください。");
            }
        });

        foodFrame.add(registerFoodButton);
        foodFrame.setVisible(true);
    }

    private void showMealRecordScreen() {
        // 食事記録画面の実装
        JOptionPane.showMessageDialog(this, "食事記録機能は未実装です。");
    }

    private void showGoalSettingScreen() {
        // 目標設定画面の実装
        JOptionPane.showMessageDialog(this, "目標設定機能は未実装です。");
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(CalorieManagementApp::new);
    }
}
