Assignment Title: Online Chat Application

1. Server Implementation:
import java.io.*;
import java.net.*;
import java.util.*;

//ChatServer class to manage connections from multiple clients.
//The server handles incoming connections, assigns a unique user ID to each connected client,
//and maintains a list of connected users.

public class ChatServer {
    private static final int PORT = 2427; // Port number for the server
    private static Set<ClientHandler> clientHandlers = new HashSet<>(); // Set to maintain connected clients
    private static int userId = 0; // Counter to assign unique user IDs

 // Starts the chat server to accept incoming connections.   
    public static void main(String[] args) {
        new ChatServer().startServer();
    }

    public void startServer() {
        System.out.println("Chat server started...");
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            while (true) {
                Socket clientSocket = serverSocket.accept(); // Accept incoming client connection
                userId++;
                ClientHandler clientHandler = new ClientHandler(clientSocket, userId); 
                clientHandlers.add(clientHandler); // Add the client to the list of connected clients
                clientHandler.start(); // Start the client handler thread
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
  
   //Broadcasts a message to all connected clients except the sender.
   //Here we have message to broadcast and user ID of the sender
    static void broadcastMessage(String message, int userId) {
        for (ClientHandler clientHandler : clientHandlers) {
            if (clientHandler.getUserId() != userId) {
                clientHandler.sendMessage("User " + userId + ": " + message);
            }
        }
    }

  //Removes a client from the list of connected clients    
    static void removeClient(ClientHandler clientHandler) {
        clientHandlers.remove(clientHandler);
        System.out.println("User " + clientHandler.getUserId() + " disconnected");
    }
}

2. Client Implementation:
import java.io.*;
import java.net.*;

 //ChatClient class handles connecting to the chat server,
 //sendingssages to the server, and receiving messages from the server.

public class ChatClient {
    // Host address of the server
    private static final String SERVER_HOST = "localhost";
    // Port number of the server
    private static final int SERVER_PORT = 2427;

    public static void main(String[] args) {
        // Create a new ChatClient instance and start the client
        new ChatClient().startClient();
    }

    //Method to start the chat client.
     //It connects to the server, sets up input/output streams, and handles message sending and receiving.
    
    public void startClient() {
        try (
            // Establish a socket connection to the server
            Socket socket = new Socket(SERVER_HOST, SERVER_PORT);
            // Reader to get input from the user
            BufferedReader userInput = new BufferedReader(new InputStreamReader(System.in));
            // Writer to send messages to the server
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()))
        ) {
            // Inform the user that they are connected to the server
            System.out.println("Congratulations! You're now Connected to the chat server.");
            System.out.println("Type your messages below and Press Enter to send.");

            // Start a separate thread to receive messages from the server
            Thread receiverThread = new Thread(() -> {
                try {
                    String serverMessage;
                    // Continuously read messages from the server and print them
                    while ((serverMessage = in.readLine()) != null) {
                        System.out.println(serverMessage);
                    }
                } catch (IOException e) {
                    System.out.println("Error reading message from server: " + e.getMessage());
                }
            });

            // Start the receiver thread
            receiverThread.start();

            // Main thread to read user input and send it to the server
            String userInputLine;
            while ((userInputLine = userInput.readLine()) != null) {
                out.println(userInputLine);
            }
        } catch (IOException e) {
            System.out.println("Error connecting to the chat server: " + e.getMessage());
        }
    }
}

3. Port Generator Implementation
import java.util.Random;

//PortGenerator class to generate a random port number within a specified range
public class PortGenerator {
    public static void main(String[] args) {
        int minPort = 1024; // Minimum port number (exclusive) 
        int maxPort = 49151; // Maximum port number (inclusive)

        int randomPort = generateRandomPort(minPort, maxPort);
        System.out.println("Random port: " + randomPort);
    }

    // Generates a random port number within the specified range
    //
    public static int generateRandomPort(int minPort, int maxPort) {
        Random random = new Random(); //Return a random port number within the specified range
        return random.nextInt(maxPort - minPort) + minPort;
    }
}

4. User Interface:


 
README.MD
# Simple-Online-Chat-Application
## Overview
This simple online chat application allows multiple users to connect to a central server, send messages, and receive messages from other users. It uses Java's socket programming for client-server communication.
## Features
- **Server**: Manages connections from multiple clients, assigns unique user IDs, and maintains a list of connected users.
- **Client**: Connects to the server, sends messages to the server, and receives broadcast messages from other users.
- **User Interface**: A simple text-based interface for message input and display.

## Requirements
- Java Development Kit (JDK) 8 or higher.

## How to Run

### Running the Server
1. Open a terminal/command prompt.
2. Navigate to the directory containing `ChatServer.java`.
3. Compile the `ChatServer` class:
   javac ChatServer.java
4. Run the ChatServer class:
   java ChatServer
### Implementation Details
# Server
- The ChatServer class listens on a specified port (default 2427) for incoming client connections.
- It assigns a unique user ID to each connected client and maintains a list of active client handlers.
- It broadcasts messages from any client to all other connected clients.
# Client
- The ChatClient class connects to the server using the specified host and port.
- It provides a simple text-based interface for the user to type and send messages.
- It runs a separate thread to listen for and display messages from the server.
# User Interface
- The user interface is text-based, using the console for input and output.
- Users can type their messages and press Enter to send them to the server.
- Messages from other users are displayed in the console.

## Error Handling
- Handles errors during client-server communication.
- Removes clients from the server list when they disconnect or encounter errors.
  
### Example from Screenshots of the Text-Based User Interface
Here are example screenshots of the text-based user interface for both the server and the client.
Congratulations! You're now Connected to the chat server.
*Type your messages below and Press Enter to send.
Hello
User 4: Hi! What's up?
Everything
User 7: Hello Java!
tell me more
User 1: 
I'm fine
User 1: 
User 8:*

README.MD FILE
https://github.com/pkasirim/Simple-Online-Chat-Application.git


# Simple-Online-Chat-Application
## Overview
This simple online chat application allows multiple users to connect to a central server, send messages, and receive messages from other users. It uses Java's socket programming for client-server communication.
## Features
- **Server**: Manages connections from multiple clients, assigns unique user IDs, and maintains a list of connected users.
- **Client**: Connects to the server, sends messages to the server, and receives broadcast messages from other users.
- **User Interface**: A simple text-based interface for message input and display.

## Requirements
- Java Development Kit (JDK) 8 or higher.

## How to Run

### Running the Server
1. Open a terminal/command prompt.
2. Navigate to the directory containing `ChatServer.java`.
3. Compile the `ChatServer` class:
   javac ChatServer.java
4. Run the ChatServer class:
   java ChatServer
### Implementation Details
# Server
- The ChatServer class listens on a specified port (default 2427) for incoming client connections.
- It assigns a unique user ID to each connected client and maintains a list of active client handlers.
- It broadcasts messages from any client to all other connected clients.
# Client
- The ChatClient class connects to the server using the specified host and port.
- It provides a simple text-based interface for the user to type and send messages.
- It runs a separate thread to listen for and display messages from the server.
# User Interface
- The user interface is text-based, using the console for input and output.
- Users can type their messages and press Enter to send them to the server.
- Messages from other users are displayed in the console.

## Error Handling
- Handles errors during client-server communication.
- Removes clients from the server list when they disconnect or encounter errors.
  
### Example from Screenshots of the Text-Based User Interface
Here are example screenshots of the text-based user interface for both the server and the client.
Congratulations! You're now Connected to the chat server.
*Type your messages below and Press Enter to send.
Hello
User 4: Hi! What's up?
Everything
User 7: Hello Java!
tell me more
User 1: 
I'm fine
User 1: 
User 8:*

**This implementation ensures that the chat application is usable, with a straightforward and intuitive text-based user interface for sending and receiving messages. The provided screenshots illustrate the expected output in the console when running the server and client applications.**

### References 
- [GeeksforGeeks: How to Implement a Simple Chat Application Using Sockets in Java](https://www.geeksforgeeks.org/simple-chat-application-using-sockets-in-java/)
- [GitHub: Simple-Chat-Project](https://github.com/RichDaly/Simple_Chat_Project)
- [GitHub: Simple-Chat-Project](https://github.com/RichDaly/Simple_Chat_Project)
- [Java Socket Programming - GeeksforGeeks](https://www.geeksforgeeks.org/socket-programming-in-java/)
- [Java Networking - Oracle Documentation](https://docs.oracle.com/javase/tutorial/networking/sockets/)

