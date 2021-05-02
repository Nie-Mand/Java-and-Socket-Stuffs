## Exercice 23 [TCP + Multithreaded]

### Server.java
```java
import java.io.*;
import java.net.*;

class Server {
    public static void print(Object o) {
        System.out.println(o.toString());
    }
    public static void main(String[] args) throws Exception {
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
            // If the Client sends "bye" exit
            while (!message.equals("bye")) {
                // Recieve a Message
                message = in.readUTF();
                print("[CLIENT]: " + message);
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

}

```

### Client.java
```java
import java.io.*;
import java.net.*;

class Client {
    public static void print(Object o) {
        System.out.println(o.toString());
    }

    public static void main(String[] args) {

        try {
            Socket socket = new Socket("localhost", 6666);
            DataOutputStream out = new DataOutputStream(socket.getOutputStream());
            print("A Client has Connecting");

            // Just a Buffer to get Text, you can use Scanner
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

            String message = "";
            // If the Client sends "bye" exit
            while (!message.equals("bye")) {
                message = br.readLine();
                // Send a Message
                out.writeUTF(message);
                out.flush();
            }

            // Close Connection
            socket.close();
        } catch (Exception e) {
            print(e);
        }

    }
}
```
