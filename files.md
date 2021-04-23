# Files and Java Stuffs

### Main App
```java
import javax.swing.*;

class App {
    public static void main(String Args[]) {

        JFrame app = new JFrame("Files Manager");
        app.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        app.setSize(400, 500);
        app.setLayout(new BoxLayout(app.getContentPane(), BoxLayout.Y_AXIS));

        JTabbedPane tabs = new JTabbedPane();
        tabs.add("Create", new Creator().getPanel());
        tabs.add("Read", new Reader().getPanel());
        tabs.add("Delete", new Killer().getPanel());
        app.add(tabs);

        app.setVisible(true);
    }
}
```
### Some Utilities (CRDing Files, Popup)
```java
import javax.swing.*;
import java.io.*;
import java.util.Scanner;

class utils {
    JPanel panel;

    public static void log(Object o) {
        System.out.println(o.toString());
    }

    protected void _popup(int result, String msg) {
        JOptionPane.showMessageDialog(null, msg, result == 0 ? "ERROR" : "SUCCESS", result);
    }

    protected void _createFile(String fileName, String fileContent) {
        try {
            FileWriter file = new FileWriter(fileName);
            file.write(fileContent);
            file.close();
        } catch (Exception e) {
            log("Error: " + e);
        }
    }

    protected String _readFile(String fileName) {
        String result = "";
        try {
            File file = new File(fileName);
            Scanner reader = new Scanner(file);
            while (reader.hasNextLine())
                result = result + "\n" + reader.nextLine();
            reader.close();
        } catch (Exception e) {
            log("Error: " + e);
        }
        return result;
    }

    protected void _killFile(String fileName) {
        File file = new File(fileName);
        if (file.delete())
            log("Deleted!");
        else
            log("Error: ");
    }

    public JPanel getPanel() {
        return panel;
    }
}
```
### Creator Class
```java
import javax.swing.*;
import java.awt.event.*;

class Creator extends utils {

    public Creator() {
        panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));

        JLabel labelOfName = new JLabel("Enter File Name to Create");
        JTextField fileName = new JTextField(40);
        fileName.setMaximumSize(fileName.getPreferredSize());
        JLabel labelOfContent = new JLabel("Enter Its Content");
        JTextArea fileContent = new JTextArea(3, 40);

        JButton add = new JButton("Create");
        add.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent ev) {
                log("Creating the File..");
                String name = fileName.getText();
                String content = fileContent.getText();
                try {
                    _createFile(name, content);
                    _popup(-1, name + ": created successfully");
                } catch (Exception e) {
                    log("Error: " + e);
                    _popup(0, "Error: " + e);
                }
            }
        });

        panel.add(labelOfName);
        panel.add(fileName);
        panel.add(labelOfContent);
        panel.add(fileContent);
        panel.add(add);
    }
}
```
### Reader Class 
```java
import javax.swing.*;
import java.awt.event.*;

class Reader extends utils {

    public Reader() {
        panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));

        JLabel labelOfName = new JLabel("Enter File Name to Create");
        JTextField fileName = new JTextField(40);
        fileName.setMaximumSize(fileName.getPreferredSize());
        JButton add = new JButton("Read");
        JLabel resultingContent = new JLabel();

        add.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent ev) {
                log("Reading the File..");
                String name = fileName.getText();
                try {
                    String content = _readFile(name);
                    resultingContent.setText(content);
                } catch (Exception e) {
                    log("Error: " + e);
                    _popup(0, "Error: " + e);
                }
            }
        });

        panel.add(labelOfName);
        panel.add(fileName);
        panel.add(add);
        panel.add(resultingContent);
    }
}
```

### Killer Class
```java
import javax.swing.*;
import java.awt.event.*;

class Killer extends utils {

    public Killer() {
        panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));

        JLabel labelOfName = new JLabel("Enter File Name to Delete");
        JTextField fileName = new JTextField(40);
        fileName.setMaximumSize(fileName.getPreferredSize());
        JButton add = new JButton("Delete");

        add.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent ev) {
                log("Deleting the File..");
                String name = fileName.getText();
                try {
                    _killFile(name);
                    _popup(-1, name + ": deleted successfully");
                } catch (Exception e) {
                    log("Error: " + e);
                    _popup(0, "Error: " + e);
                }
            }
        });

        panel.add(labelOfName);
        panel.add(fileName);
        panel.add(add);
    }
}
```
