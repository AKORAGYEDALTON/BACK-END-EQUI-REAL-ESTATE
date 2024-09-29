# BACK-END-EQUI-REAL-ESTATE
Creation of a back end for The Equi-realites, using Python Django In cordination with flutter
Flutter Code (welcome_page.dart)

import 'package:flutter/material.dart';

class WelcomePage extends StatefulWidget {
  @override
  _WelcomePageState createState() => _WelcomePageState();
}

class _WelcomePageState extends State<WelcomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(
          'Welcome!',
          style: TextStyle(fontSize: 48),
        ),
      ),
    );
  }
}


Python Code ((link unavailable))

import socket
import json

# Create a socket object
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind the socket to a address and port
sock.bind(('localhost', 8080))

# Listen for incoming connections
sock.listen(1)

print('Waiting for connection...')

# Wait for connection
conn, addr = sock.accept()

print('Connected by', addr)

# Send data to Flutter app
def send_data(data):
    conn.sendall(json.dumps(data).encode())

# Receive data from Flutter app
def receive_data():
    return conn.recv(1024).decode()

# Activate welcome page
send_data({'action': 'activate_welcome_page'})

# Receive confirmation
response = receive_data()
print(response)

# Close the connection
conn.close()


Flutter Main Function (main.dart)

import 'package:flutter/material.dart';
import 'package:socket_io/manager.dart';
import 'package:socket_io/socket_io.dart';
import 'welcome_page.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome Page',
      home: WelcomePage(),
    );
  }
}

class SocketService {
  static ServerSocket serverSocket;

  static void initSocket() {
    serverSocket = ServerSocket(
      'localhost',
      8080,
      handleData: (data) {
        if (data['action'] == 'activate_welcome_page') {
          Navigator.push(
            navigatorKey.currentContext,
            MaterialPageRoute(builder: (context) => WelcomePage()),
          );
        }
      },
    );
  }
}


