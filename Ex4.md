# Exercice 4

## Operation.java
```java
import java.util.Scanner;

public class Operation {
    private int x, y;
    private String op;

    public Operation(int x, int y, String op) throws NegativeNumberException, UnsupportedOperationException {
        if ((x < 0) || (y < 0))
            throw new NegativeNumberException();
        if (!(op.equals("+") || op.equals("-") || op.equals("*") || op.equals("/")))
            throw new UnsupportedOperationException();
        this.x = x;
        this.y = y;
        this.op = op;
    }

    public int solution() throws NegativeNumberException {
        int result = 0;
        switch (op) {
            case "+":
                result = x + y;
                break;
            case "-":
                result = x - y;
                break;
            case "*":
                result = x * y;
                break;
            case "/":
                result = x / y;
                break;
        }
        if (result < 0)
            throw new NegativeNumberException();
        return result;
    }

    public static void print(Object o) {
        System.out.println(o.toString());
    }

    public static String get(Scanner r) throws NegativeNumberException, UnsupportedOperationException {
        int x, y;
        String op, action;
        do {
            print("Get Operation? [y/n]");
            action = r.next();
        } while (!(action.equals("y") || action.equals("n")));
        if (action.equals("n"))
            return "bye";
        print("Enter the first Positive Number");
        x = r.nextInt();
        if (x < 0)
            throw new NegativeNumberException();
        print("Enter the second Positive Number");
        y = r.nextInt();
        if (y < 0)
            throw new NegativeNumberException();
        print("Enter the Operation");
        op = r.next();
        if (!(op.equals("+") || op.equals("-") || op.equals("*") || op.equals("/")))
            throw new UnsupportedOperationException();
        return x + " " + op + " " + y;
    }
}

class NegativeNumberException extends Exception {
    NegativeNumberException() {
        super("You can only use Positive Numbers");
    }
}

class UnsupportedOperationException extends Exception {
    UnsupportedOperationException() {
        super("+-*/ Are the Only Supported Operations");
    }
}
```

## Server.java
```java
import java.io.*;
import java.net.*;

public class Server {
    public static void main(String[] args) {
        ServerSocket server = null;
        try {
            server = new ServerSocket(6666);
            print("Server is Running");
            while (true) {
                Socket client = server.accept();
                Handler clientSock = new Handler(client);
                new Thread(clientSock).start();
            }
        } catch (Exception e) {
            print(e);
        } finally {
            print("Server is Shutting down");
            try {
                server.close();
            } catch (Exception e) {
                print(e);
            }
        }
    }

    public static void print(Object o) {
        System.out.println(o.toString());
    }
}

class Handler implements Runnable {
    private final Socket socket;

    public Handler(Socket socket) {
        this.socket = socket;
    }

    public void run() {
        DataInputStream in = null;
        try {
            in = new DataInputStream(socket.getInputStream());
            String message = "";
            while (true) {
                message = in.readUTF();
                if (message.equals("bye"))
                    break;
                int result = 0;
                try {
                    result = extract(message);
                    print(message + " = " + result);
                } catch (Exception e) {
                    print(message + " = -1 [" + e.getMessage() + "]");
                }
            }

        } catch (Exception e) {
            print(e);
        } finally {
            try {
                in.close();
            } catch (Exception e) {
                print(e);
            }
        }
    }

    public static void print(Object o) {
        System.out.println(o.toString());
    }

    public static int extract(String message) throws NegativeNumberException, UnsupportedOperationException {
        int space = message.indexOf(' ');
        int space2 = message.indexOf(' ', space + 1);
        int x, y;
        String op;
        x = Integer.parseInt(message.substring(0, space));
        y = Integer.parseInt(message.substring(space2 + 1));
        op = message.substring(space + 1, space2);

        if ((x < 0) || (y < 0))
            throw new NegativeNumberException();
        if (!(op.equals("+") || op.equals("-") || op.equals("*") || op.equals("/")))
            throw new UnsupportedOperationException();
        return new Operation(x, y, op).solution();
    }
}
```

## Client.java
```java
import java.io.*;
import java.net.*;
import java.util.Scanner;

public class Client {
    public static void main(String[] args) {
        Scanner r = new Scanner(System.in);
        try {
            Socket socket = new Socket("localhost", 6666);
            DataOutputStream out = new DataOutputStream(socket.getOutputStream());
            print("I am Connected");

            String message = "";
            while (!message.equals("bye")) {
                try {
                    message = Operation.get(r);
                    out.writeUTF(message);
                    out.flush();
                } catch (Exception e) {
                    print(e);
                }
            }
            print("bye");
            socket.close();
            r.close();
        } catch (Exception e) {
            print(e);
        }
    }

    public static void print(Object o) {
        System.out.println(o.toString());
    }

}
```
