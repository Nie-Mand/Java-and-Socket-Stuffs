## Exercice 1 [TCP Client Server-Connection]

### Server.java
```java
import java.io.*;
import java.net.*;

class Server {
    public static void print(Object o) {
        System.out.println(o.toString());
    }

    public static void main(String[] args) throws Exception {
        try {
            /*
             * Create a ServerSocket, a Socket that listens to the Server, an Input Stream,
             * then Log when Ready
             */
            ServerSocket server = new ServerSocket(6666);
            Socket socket = server.accept();
            DataInputStream in = new DataInputStream(socket.getInputStream());
            print("Server is Running");

            String message = "";
            // If the Client sends "bye" exit
            while (!message.equals("bye")) {
                // Recieve a Message
                message = in.readUTF();
                print("[CLIENT]: " + message);
            }

            // Close Connection
            print("Server is Shutting down");
            in.close();
            socket.close();
            server.close();
        } catch (Exception e) {
            print(e);
        }
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
            // Create the Socket, Output Stream and Log when Connected
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
