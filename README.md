# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
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
P
## PROGRAM - ARP
## server-side.py
import socket

def start_server():
    host = '127.0.0.1'
    port = 8000

    address_table = {
        "165.165.80.80": "6A:08:AA:C2",
        "165.165.79.1": "8A:BC:E3:FA"
    }

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server:
        server.bind((host, port))
        server.listen()
        print("Server running on port 8000...")

        conn, addr = server.accept()
        with conn:
            print("Connected by:", addr)

            while True:
                ip = conn.recv(1024).decode()
                if not ip or ip.lower() == "exit":
                    break

                print("Requested IP:", ip)

                mac = address_table.get(ip, "MAC Address Not Found")
                conn.send(mac.encode())

    print("Server closed.")

if __name__ == "__main__":
    start_server()
## client-side.py
import socket

def start_client():
    host = '127.0.0.1'
    port = 8000

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as client:
        client.connect((host, port))

        while True:
            ip = input("Enter Logical Address (or 'exit' to quit): ")

            client.send(ip.encode())

            if ip.lower() == "exit":
                break

            mac = client.recv(1024).decode()
            print("MAC Address:", mac)

    print("Client closed.")

if __name__ == "__main__":
    start_client()
## OUPUT - ARP
<img width="869" height="606" alt="image" src="https://github.com/user-attachments/assets/c5fd52fb-cdd3-482e-a155-93c0a5501810" />

## PROGRAM - RARP
## server-side.py
import socket

def start_server():
    host = '127.0.0.1'
    port = 9000

    mac_table = {
        "6A:08:AA:C2": "192.168.1.100",
        "8A:BC:E3:FA": "192.168.1.99"
    }

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server:
        server.bind((host, port))
        server.listen()
        print("Server listening on port 9000...")

        conn, addr = server.accept()
        with conn:
            print("Connected by:", addr)

            while True:
                mac = conn.recv(1024).decode()

                if not mac or mac.lower() == "exit":
                    break

                print("Received MAC:", mac)

                ip = mac_table.get(mac, "IP Address Not Found")
                conn.send(ip.encode())

    print("Server closed.")

if __name__ == "__main__":
    start_server()
## client-side.py
import socket

def start_client():
    host = '127.0.0.1'
    port = 9000

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as client:
        client.connect((host, port))

        while True:
            mac = input("Enter MAC Address (or 'exit' to quit): ")

            client.send(mac.encode())

            if mac.lower() == "exit":
                break

            ip = client.recv(1024).decode()
            print("Logical Address:", ip)

    print("Client closed.")

if __name__ == "__main__":
    start_client()
## OUPUT -RARP

<img width="1050" height="103" alt="Screenshot 2026-02-17 131420" src="https://github.com/user-attachments/assets/23dad46e-0f1f-477f-b42e-80ec08a19ef0" />

<img width="1137" height="135" alt="image" src="https://github.com/user-attachments/assets/fd8b48b3-4ad3-46f4-a701-8507465b5a51" />


## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
