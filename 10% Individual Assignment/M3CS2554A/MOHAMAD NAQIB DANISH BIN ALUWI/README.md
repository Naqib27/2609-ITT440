# MOHAMAD NAQIB DANISH BIN ALUWI

# ITT440 - Network Programming

**Group:** M3CS2554A

**Name:** Mohamad Naqib Danish bin Aluwi

**Student ID:** 2024411992

**Lecturer:** Sir Shahadan Bin Saad

**Video Link:** https://youtu.be/W0z-MQZF7aM?si=y_1SgQQ-5-CHHB1i


NETWORK PROGRAMMING WITH PYTHON
Development of a Python-Based TCP Client-Server Communication System

# 1.0 Introduction

Network programming is an important area of computer science that enables computers and applications to communicate and exchange information through a network. Python provides a built-in socket library that allows developers to create network applications using communication protocols such as Transmission Control Protocol (TCP).
This project focuses on developing a Python-based TCP client-server communication system. The server receives messages from clients, processes commands, and returns appropriate responses. The system also uses multithreading to handle multiple client connections and timestamps to record incoming messages.

# 2.0 Problem Statement

Communication between computers requires a reliable method of sending, receiving, and processing data. Without an appropriate network communication system, applications cannot exchange information efficiently.
Therefore, this project develops a simple TCP client-server application that demonstrates how network connections are established, how messages are transferred, and how a server responds to client requests. It also demonstrates how multiple clients can be handled using Python's threading functionality.

# 3.0 Objectives

The objectives of this project are:
1.	To understand the basic principles of network programming using Python.
2.	To establish TCP connections between a client and a server.
3.	To implement message sending and receiving using the socket library.
4.	To process commands and return responses to clients.
5.	To apply multithreading to support multiple client connections.
6.	To display timestamps for monitoring client communications.
   
# 4.0 Setup and Environment

The project uses the following software and technologies:
Component	Description
Programming language	Python
Development environment	Microsoft Visual Studio Community
Networking library	socket
Threading library	threading
Date and time library	datetime
Communication protocol	TCP
IP address	127.0.0.1
Port number	12345
The server uses the loopback address 127.0.0.1, which allows the application to be tested on the same computer without requiring an external network. The client connects to the server through port 12345.

# 5.0 Technical Methodology
  	
The system follows a client-server architecture. The client sends a message to the server, and the server processes the message before returning a response.
The implementation consists of the following stages:

5.1 Socket Creation
The server creates a TCP socket using socket.socket(socket.AF_INET, socket.SOCK_STREAM). The AF_INET parameter specifies IPv4 addressing, while SOCK_STREAM specifies a stream-oriented TCP connection.
5.2 Server Initialization
The bind() function associates the socket with the selected IP address and port. The listen() function prepares the server to accept incoming connections, while accept() waits for a client to connect.
5.3 Client Request Processing
When a client connects, the server calls the handle_client() function. This function receives messages, removes newline characters, records the current time, and determines which response should be returned.
5.4 Multithreading
The threading.Thread() function creates a separate thread for each connected client. This allows the server to handle client connections independently rather than processing every client through a single communication handler.
5.5 Message Response
The server processes commands such as hello, time, help, and exit. Other messages receive an echo-style response. The response is encoded into UTF-8 bytes and transmitted using sendall().

# 6.0 Program Implementation

The following code implements the server component of the project.
import socket
import threading
from datetime import datetime

HOST = "127.0.0.1"
PORT = 12345

```python
def handle_client(connection, address):
    print(f"\n[{address}] Client connected.")

    try:
        with connection:
            reader = connection.makefile(
                "r", encoding="utf-8"
            )

            while True:
                message = reader.readline()

                if not message:
                    break

                message = message.rstrip("\r\n")
                current_time = datetime.now().strftime(
                    "%H:%M:%S"
                )

                print(
                    f"[{current_time}] {address}: {message}"
                )

                if message.lower() == "exit":
                    response = (
                        "Goodbye! You have disconnected."
                    )
                    connection.sendall(
                        (response + "\n").encode("utf-8")
                    )
                    break

                if message.lower() == "hello":
                    response = (
                        "Hello! Welcome to the Python server."
                    )
                elif message.lower() == "time":
                    response = (
                        "Server time: "
                        + datetime.now().strftime(
                            "%d-%m-%Y %H:%M:%S"
                        )
                    )
                elif message.lower() == "help":
                    response = (
                        "Commands: hello, time, help, exit"
                    )
                else:
                    response = "Server received: " + message

                connection.sendall(
                    (response + "\n").encode("utf-8")
                )

    except (ConnectionError, OSError) as error:
        print(f"Connection error with {address}: {error}")

    finally:
        print(f"[{address}] Client disconnected.")


def start_server():
    server = socket.socket(
        socket.AF_INET,
        socket.SOCK_STREAM
    )

    server.setsockopt(
        socket.SOL_SOCKET,
        socket.SO_REUSEADDR,
        1
    )

    try:
        server.bind((HOST, PORT))
        server.listen()

        print("=" * 45)
        print("PYTHON NETWORK PROGRAMMING SERVER")
        print("=" * 45)
        print(f"Server address: {HOST}")
        print(f"Server port: {PORT}")
        print("Status: Listening for connections...")
        print("Press Ctrl+C to stop the server.")

        while True:
            connection, address = server.accept()

            client_thread = threading.Thread(
                target=handle_client,
                args=(connection, address),
                daemon=True
            )

            client_thread.start()

    except OSError as error:
        print(f"Server error: {error}")

    except KeyboardInterrupt:
        print("\nServer stopped by administrator.")

    finally:
        server.close()
if __name__ == "__main__":
    start_server()
 ```

# 7.0 Network Communication Logic

The server uses TCP to establish a connection-oriented communication channel. Before exchanging messages, the client must successfully connect to the server's IP address and port.
The program uses newline characters to identify the end of each message. The server reads incoming data with reader.readline(), while the client must send each message with a newline character at the end. This method allows the receiver to identify individual messages.
The server supports four commands:
Command	Function
hello	Returns a welcome message.
time	Returns the server's current date and time.
help	Displays the available commands.
exit	Sends a goodbye message and disconnects the client.
If the client sends another message, the server returns the message with the prefix Server received:.

# 8.0 Results and Analysis

When the server starts successfully, it displays its IP address, port number, and listening status. When a client connects, the server displays the client's address and records incoming messages with timestamps.
For example, when a client sends hello, the expected response is:
Hello! Welcome to the Python server.
When the client sends help, the server returns:
Commands: hello, time, help, exit
When the client sends time, the server returns the current date and time. When the client sends exit, the server responds with a goodbye message and closes that client's connection.
These results demonstrate that the server can receive client requests, process different commands, and return responses through a TCP connection. However, the server code alone is not sufficient to demonstrate a complete exchange; a separate client program is required for testing.

# 9.0 Discussion

The project demonstrates several fundamental concepts in network programming. First, TCP provides reliable, ordered delivery of data between connected applications. Second, the socket library provides the functions required to establish connections and exchange information.
The use of multithreading improves the server's ability to handle multiple clients because each connection is assigned a separate thread. The datetime library also helps monitor communication by recording the time at which messages are received.
Despite these advantages, the system has some limitations. The server uses 127.0.0.1, meaning it is intended for local testing on the same computer. It does not include authentication, encryption, or advanced protection against malicious input. In addition, multithreading alone does not guarantee unlimited scalability when many clients connect simultaneously.
Future improvements could include allowing connections from other computers on a local network, adding user authentication, encrypting communications, and introducing a graphical user interface.

# 10.0 Conclusion

In conclusion, this project successfully demonstrates the basic implementation of network programming using Python. By using the socket, threading, and datetime libraries, the system can establish TCP connections, receive and process client messages, return appropriate responses, and record communication timestamps.
The project provides practical experience with client-server architecture, TCP communication, and concurrent connection handling. It also serves as a foundation for developing more advanced network applications, including chat systems, remote services, and network monitoring tools.

# 11.0 Reference

Gunawan, T. S., Abdullah, N. A. J., Kartiwi, M., & Ihsanto, E. (2020). Social network analysis using Python data mining. In *2020 8th International Conference on Cyber and IT Service Management (CITSM)* (pp. 1–6). IEEE. https://doi.org/10.1109/CITSM50537.2020.9268866

Ortega, J. M. (2023). *Python for security and networking: Leverage Python modules and tools in securing your network and applications*. Packt Publishing.

Ratan, A. (2018). *Practical network automation: A beginner's guide to automating and optimizing networks using Python, Ansible, and more*. Packt Publishing.

Töpke, C. (2023). *Network programming and automation essentials: Get started in the realm of network automation using Python and Go*. Packt Publishing.

Zhou, Y., Wang, D., & Zhang, M. (2006). Designing functional networks through evolutionary programming. In *2006 6th World Congress on Intelligent Control and Automation* (pp. 3250–3254). IEEE. https://doi.org/10.1109/WCICA.2006.1712968


