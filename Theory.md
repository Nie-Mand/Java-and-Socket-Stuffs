# Javatpoint :

Java Socket programming is used for communication between the applications running on different JRE.

Java Socket programming can be connection-oriented or connection-less.

Socket and ServerSocket classes are used for connection-oriented socket programming and DatagramSocket and DatagramPacket classes are used for connection-less socket programming.

The client in socket programming must know two information:

+ IP Address of Server, and
+ Port number.

Here, we are going to make one-way client and server communication. In this application, client sends a message to the server, server reads the message and prints it. Here, two classes are being used: Socket and ServerSocket. The Socket class is used to communicate client and server. Through this class, we can read and write message. The ServerSocket class is used at server-side. The accept() method of ServerSocket class blocks the console until the client is connected. After the successful connection of client, it returns the instance of Socket at server-side.

![some image](https://static.javatpoint.com/core/images/socket-programming.png)

### Socket class
A socket is simply an endpoint for communications between the machines. The Socket class can be used to create a socket.

Important methods
Method	| Description
---|---
public InputStream getInputStream()	| returns the InputStream attached with this socket.
public OutputStream getOutputStream()	| returns the OutputStream attached with this socket.
public synchronized void close()	| closes this socket

### ServerSocket class
The ServerSocket class can be used to create a server socket. This object is used to establish communication with the clients.

Important methods
Method	| Description
---|---
public Socket accept()	| returns the socket and establish a connection between server and client.
public synchronized void close()	| closes the server socket.


### Java DatagramSocket and DatagramPacket

ava DatagramSocket class
Java DatagramSocket class represents a connection-less socket for sending and receiving datagram packets.

A datagram is basically an information but there is no guarantee of its content, arrival or arrival time.

Commonly used Constructors of DatagramSocket class
+ DatagramSocket() throws SocketEeption: it creates a datagram socket and binds it with the available Port Number on the localhost machine.
+ DatagramSocket(int port) throws SocketEeption: it creates a datagram socket and binds it with the given Port Number.
+ DatagramSocket(int port, InetAddress address) throws SocketEeption: it creates a datagram socket and binds it with the specified port number and host address.

Java DatagramPacket class
Java DatagramPacket is a message that can be sent or received. If you send multiple packet, it may arrive in any order. Additionally, packet delivery is not guaranteed.

Commonly used Constructors of DatagramPacket class
+ DatagramPacket(byte[] barr, int length): it creates a datagram packet. This constructor is used to receive the packets.
+ DatagramPacket(byte[] barr, int length, InetAddress address, int port): it creates a datagram packet. This constructor is used to send the packets.
