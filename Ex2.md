## Exercice 2 [UDP]

**Using DatagramSockets instead**

### Server.java
```java
import java.net.*;

class Server {
    public static void print(Object o) {
        System.out.println(o.toString());
    }

    public static void main(String[] args) throws Exception {
        try {

            // Create the Socket and a Packet Object
            DatagramSocket socket = new DatagramSocket(3000);
            byte[] buf = new byte[1024];
            DatagramPacket packet = new DatagramPacket(buf, 1024);

            String message = "";
            while (!message.equals("bye")) {
                // Recieve a Message
                socket.receive(packet);
                // Buffer to String
                message = new String(packet.getData(), 0, packet.getLength());
                print(message);
            }

            // Close Connection
            socket.close();

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

            // Create the Socket and a Packet Object
            DatagramSocket socket = new DatagramSocket();
            InetAddress ip = InetAddress.getByName("127.0.0.1");
            DatagramPacket packet;

            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            String message = "";
            while (!message.equals("bye")) {
                message = br.readLine();
                // Send a Message
                packet = new DatagramPacket(message.getBytes(), message.length(), ip, 3000);
                socket.send(packet);
            }

            // Close Connection
            socket.close();
        } catch (Exception e) {
            print(e);
        }

    }
}
```
