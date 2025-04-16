
ServerSocketand Socket
Streams for communication

# 1 setup Socket 

When we want to open a network connection to another machine, we need the IP address (or URL) of that machine as well as the port number on which the respective server is running (e.g., for http, the default port is 80).


The java class Socket comes with constructors that take both as argument:
Socket s = new Socket("127.0.0.1", 8082); //or
Socket s = new Socket("www.google.com",80); //no "http://" here!

This connects us to the server running at the specified target or throws an exception otherwise.
We can then use s.getInputStream()ands.getOutputStream() and the respective read and write methods to communicate with the server.
When we are done, we have to close the socket again via s.close(). This automatically closes both streams as well.


# 2 Implementing the server


When we want to implement a server, we need instances of class ServerSocket which get a port as parameter in their constructor:
ServerSocketserver = new ServerSocket(8082);

Afterwards, we invoke server.accept(); which blocks until a client has connected and then returns an instance of Socket. We can then use this object to communicate with the client via the streams again.

![[Pasted image 20250416205737.png]]

EchoServer
```java
//exception handling omitted
ServerSocket server = newServerSocket(8082);
Socket client = server.accept();

int i;

while((i = client.getInputStream().read()) != -1) {
	client.getOutputStream().write(i);
}

server.close();
```


EchoClient
```java
Socket s = newSocket("127.0.0.1", 8082);
s.getOutputStream().write(42);
s.getOutputStream().write(123);
System.out.println(s.getInputStream().read());
s.close();
```