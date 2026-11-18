# 2c.SIMULATING ARP /RARP PROTOCOLS

## AIM
To write a python program for simulating ARP protocols using TCP.

## ALGORITHM:
**ARP (Address Resolution Protocol) Algorithm**
1. Start the program.
2. Create a TCP socket connection between client and server.
3. Get the IP address from the user.
4. Send the IP address to the server.
5. The server searches the IP address in the ARP table.
6. Retrieve the corresponding MAC address if the IP address is found.
7. Send the MAC address to the client.
8. Display the MAC address received from the server.
9. Repeat the process until the user exits.
10. Close the socket connection and stop the program.

**RARP (Reverse Address Resolution Protocol) Algorithm**
1. Start the program.
2. Create a TCP socket connection between client and server.
3. Get the MAC address from the user.
4. Send the MAC address to the server.
5. The server searches the MAC address in the RARP table.
6. Retrieve the corresponding IP address if the MAC address is found.
7. Send the IP address to the client.
8. Display the IP address received from the server.
9. Repeat the process until the user exits.
10. Close the socket connection and stop the program.

## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.

## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.

## PROGRAM - ARP
**arp_server.py**
~~~
import socket

# IP to MAC address mapping
arp_table = {
    "192.168.1.100": "6A:08:AA:C2:11:01",
    "192.168.1.99": "8A:BC:E3:FA:22:02",
    "192.168.1.1": "00:1A:2B:3C:4D:5E"
}

# Create TCP socket
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# Bind server
server.bind(("127.0.0.1", 8000))

# Listen for clients
server.listen(5)

print("ARP Server is waiting for connection...")

while True:
    client, address = server.accept()
    print("Client connected:", address)

    with client:
        while True:
            data = client.recv(1024).decode().strip()

            if not data:
                break

            print("Received IP Address:", data)

            mac = arp_table.get(data, "Not Found")

            client.sendall(mac.encode())

    print("Client disconnected")
~~~

**arp_client.py**
~~~
import socket

# Create TCP socket
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    # Connect to ARP server
    client.connect(("127.0.0.1", 8000))

    print("Connected to ARP Server")

    while True:
        ip = input("\nEnter IP Address (or exit): ").strip()

        if ip.lower() == "exit":
            break

        # Send IP address
        client.sendall(ip.encode())

        # Receive MAC address
        mac = client.recv(1024).decode()

        print("MAC Address:", mac)

except ConnectionRefusedError:
    print("Error: Start the ARP server first.")

except OSError as error:
    print("Socket Error:", error)

finally:
    client.close()
    print("ARP Client disconnected")
~~~

## OUPUT - ARP
<img width="857" height="327" alt="image" src="https://github.com/user-attachments/assets/bf39af21-07b9-4f94-b03a-f86547981aab" />
<img width="855" height="192" alt="image" src="https://github.com/user-attachments/assets/8f3be516-aff8-463f-9b9f-cdcc5afff3f8" />

## PROGRAM - RARP
**rarp_server.py**
~~~
import socket

# MAC to IP address mapping
rarp_table = {
    "6A:08:AA:11:01": "192.168.1.100",
    "8A:BC:E3:FA:22:02": "192.168.1.99",
    "00:1A:2B:3C:4D:5E": "192.168.1.1"
}

# Create TCP socket
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# Bind server
server.bind(("127.0.0.1", 9000))

# Listen for clients
server.listen(5)

print("RARP Server is waiting for connection...")

while True:
    client, address = server.accept()
    print("Client connected:", address)

    with client:
        while True:
            data = client.recv(1024).decode().strip()

            if not data:
                break

            print("Received MAC Address:", data)

            ip = rarp_table.get(data.upper(), "Not Found")

            client.sendall(ip.encode())

    print("Client disconnected")
~~~

**rarp_client.py**
~~~
import socket

# Create TCP socket
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    # Connect to RARP server
    client.connect(("127.0.0.1", 9000))

    print("Connected to RARP Server")

    while True:
        mac = input("\nEnter MAC Address (or exit): ").strip()

        if mac.lower() == "exit":
            break

        # Send MAC address
        client.sendall(mac.encode())

        # Receive IP address
        ip = client.recv(1024).decode()

        print("Logical IP Address:", ip)

except ConnectionRefusedError:
    print("Error: Start the RARP server first.")

except OSError as error:
    print("Socket Error:", error)

finally:
    client.close()
    print("RARP Client disconnected")
~~~

## OUPUT -RARP
<img width="861" height="331" alt="image" src="https://github.com/user-attachments/assets/0bfd7479-c3d8-4fe5-a7da-570d4967a33b" />
<img width="871" height="217" alt="image" src="https://github.com/user-attachments/assets/cf9079c0-ae9a-4795-b206-3ffbce1eb23a" />


## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully executed.
