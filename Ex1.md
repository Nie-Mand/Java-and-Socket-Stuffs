## Exercice 1 [TCP Client Server-Connection]

### Server.java
```java
import java.io.*;
import java.net.*;

class Server {
    private ServerSocket server;
    protected Socket socket;
    protected DataInputStream in;
    protected DataOutputStream out;

    public Server(int port) throws Exception {
        try {
            server = new ServerSocket(port);
            socket = server.accept();
            in = new DataInputStream(socket.getInputStream());
            out = new DataOutputStream(socket.getOutputStream());
            System.out.println("Server is Running");
        } catch (Exception e) {
            throw new Exception(e);
        }
    }

    public void Send(String msg) throws Exception {
        try {
            out.writeUTF(msg);
            out.flush();
        } catch (Exception e) {
            throw new Exception(e);
        }
    }

    public String Receive() throws Exception {
        try {
            String msg = in.readUTF();
            return msg;
        } catch (Exception e) {
            throw new Exception(e);
        }
    }

    public void Close() throws Exception {
        try {
            in.close();
            socket.close();
            server.close();
        } catch (Exception e) {
            throw new Exception(e);
        }
    }
}
```

### Client.java
```java
import java.io.*;
import java.net.*;

class Client {
    protected Socket socket;
    protected DataInputStream in;
    protected DataOutputStream out;

    public Client(String domain, int port) throws Exception {
        try {
            socket = new Socket(domain, port);
            in = new DataInputStream(socket.getInputStream());
            out = new DataOutputStream(socket.getOutputStream());
            System.out.println("Client is Connecting on " + domain);
        } catch (ConnectException e) {
            throw new Exception("Couldn't Connect to the Server");
        }
    }

    public void Send(String msg) throws Exception {
        try {
            out.writeUTF(msg);
            out.flush();
        } catch (Exception e) {
            throw new Exception(e);
        }
    }

    public String Receive() throws Exception {
        try {
            String msg = in.readUTF();
            return msg;
        } catch (Exception e) {
            throw new Exception(e);
        }
    }

    public void Close() throws Exception {
        try {
            in.close();
            socket.close();
        } catch (Exception e) {
            throw new Exception(e);
        }
    }
}
```

## Testing Stuffs

### ServerTest.java
```java
import java.io.*;

public class ServerTest {
    public static void print(Object o) {
        System.out.println(o.toString());
    }

    public static void main(String[] args) throws Exception {

        try {
            Server server = new Server(6666);

            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            String received = "", sent = "";

            while (!received.equals("bye")) {
                received = server.Receive();
                print("[CLIENT]: " + received);

                sent = br.readLine();
                server.Send(sent);
            }
            server.Close();
        } catch (Exception e) {
            print(e);
        }

    }

}
```

### ClientTest.java
```java
import java.io.*;

class ClientTest {
    public static void print(Object o) {
        System.out.println(o.toString());
    }

    public static void main(String[] args) {

        try {
            Client client = new Client("localhost", 6666);

            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            String received = "", sent = "";

            while (!received.equals("bye")) {
                sent = br.readLine();
                client.Send(sent);

                received = client.Receive();
                print("[SERVER]: " + received);
            }

            client.Close();

        } catch (Exception e) {
            print(e);
        }
    }
}
```
