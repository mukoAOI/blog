



创建 socket、绑定、监听、接受连接和发送/接收数据等功能。用于在服务端创建一个 Socket 并接受客户端连接。

```c++
#include <iostream>
#include <cstring>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>

class SimpleSocket {
public:
    SimpleSocket(int domain, int service, int protocol, int port, u_long interface);
    int acceptConnection();
    ssize_t sendData(int clientSocket, const char* data, size_t size);
    ssize_t receiveData(int clientSocket, char* buffer, size_t size);
    ~SimpleSocket();

private:
    int serverSocket;
    struct sockaddr_in address;
    int addrlen;
};

SimpleSocket::SimpleSocket(int domain, int service, int protocol, int port, u_long interface) {
    // 创建 Socket
    serverSocket = socket(domain, service, protocol);
    if (serverSocket == 0) {
        perror("Socket creation failed");
        exit(EXIT_FAILURE);
    }

    // 配置 Socket 地址结构
    address.sin_family = domain;
    address.sin_port = htons(port);
    address.sin_addr.s_addr = interface;

    // 绑定 Socket
    if (bind(serverSocket, (struct sockaddr*)&address, sizeof(address)) < 0) {
        perror("Socket bind failed");
        close(serverSocket);
        exit(EXIT_FAILURE);
    }

    // 监听连接
    if (listen(serverSocket, 3) < 0) {
        perror("Socket listen failed");
        close(serverSocket);
        exit(EXIT_FAILURE);
    }

    addrlen = sizeof(address);
}

int SimpleSocket::acceptConnection() {
    int clientSocket = accept(serverSocket, (struct sockaddr*)&address, (socklen_t*)&addrlen);
    if (clientSocket < 0) {
        perror("Accept failed");
        close(serverSocket);
        exit(EXIT_FAILURE);
    }
    return clientSocket;
}

ssize_t SimpleSocket::sendData(int clientSocket, const char* data, size_t size) {
    return send(clientSocket, data, size, 0);
}

ssize_t SimpleSocket::receiveData(int clientSocket, char* buffer, size_t size) {
    return recv(clientSocket, buffer, size, 0);
}

SimpleSocket::~SimpleSocket() {
    close(serverSocket);
}

int main() {
    // 创建一个服务端 Socket 对象，监听端口 8080
    SimpleSocket server(AF_INET, SOCK_STREAM, 0, 8080, INADDR_ANY);

    std::cout << "Server is listening on port 8080..." << std::endl;

    int clientSocket = server.acceptConnection();
    std::cout << "Client connected!" << std::endl;

    // 发送和接收数据
    const char* message = "Hello from server";
    server.sendData(clientSocket, message, strlen(message));

    char buffer[1024] = {0};
    server.receiveData(clientSocket, buffer, 1024);
    std::cout << "Message from client: " << buffer << std::endl;

    close(clientSocket);

    return 0;
}

```

