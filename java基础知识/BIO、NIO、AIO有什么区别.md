在 Java 中，I/O 操作主要分为三种类型：BIO（Blocking I/O）、NIO（Non-blocking I/O）和 AIO（Asynchronous I/O）。它们在处理方式和应用场景上有显著的区别。

### 1. BIO（Blocking I/O）

**特点**：
- **阻塞模式**：在 I/O 操作（如读写）没有完成之前，线程会被阻塞，无法进行其他操作。
- **一个连接一个线程**：每个客户端连接都会占用一个线程，连接数多时，系统开销大，易导致线程资源耗尽。

**适用场景**：
- 适用于连接数较少且对实时性要求不高的应用，例如早期的服务器端程序。

**示例**：
```java
import java.io.*;
import java.net.*;

public class BIOServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8080);
        while (true) {
            Socket socket = serverSocket.accept(); // 阻塞等待客户端连接
            new Thread(() -> {
                try {
                    InputStream in = socket.getInputStream();
                    OutputStream out = socket.getOutputStream();
                    byte[] buffer = new byte[1024];
                    int read = in.read(buffer); // 阻塞等待数据
                    while (read != -1) {
                        out.write(buffer, 0, read);
                        read = in.read(buffer); // 阻塞等待数据
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}
```

### 2. NIO（Non-blocking I/O）

**特点**：
- **非阻塞模式**：I/O 操作可以在准备好之前返回，线程不会被阻塞，可以继续做其他事情。
- **多路复用**：通过 `Selector` 机制，可以使用一个线程管理多个连接，提高资源利用率。

**适用场景**：
- 适用于连接数多且连接时间长的应用，例如聊天服务器、在线游戏等。

**示例**：
```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;

public class NIOServer {
    public static void main(String[] args) throws IOException {
        Selector selector = Selector.open();
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        serverSocketChannel.bind(new InetSocketAddress(8080));
        serverSocketChannel.configureBlocking(false);
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

        while (true) {
            selector.select();
            Set<SelectionKey> selectedKeys = selector.selectedKeys();
            Iterator<SelectionKey> iterator = selectedKeys.iterator();
            while (iterator.hasNext()) {
                SelectionKey key = iterator.next();
                if (key.isAcceptable()) {
                    ServerSocketChannel serverChannel = (ServerSocketChannel) key.channel();
                    SocketChannel socketChannel = serverChannel.accept();
                    socketChannel.configureBlocking(false);
                    socketChannel.register(selector, SelectionKey.OP_READ);
                } else if (key.isReadable()) {
                    SocketChannel socketChannel = (SocketChannel) key.channel();
                    ByteBuffer buffer = ByteBuffer.allocate(1024);
                    int read = socketChannel.read(buffer);
                    if (read > 0) {
                        buffer.flip();
                        socketChannel.write(buffer);
                    } else if (read == -1) {
                        socketChannel.close();
                    }
                }
                iterator.remove();
            }
        }
    }
}
```

### 3. AIO（Asynchronous I/O）

**特点**：
- **异步模式**：I/O 操作是异步的，即发起 I/O 操作后，立即返回并继续其他操作，I/O 操作完成时会通知相应的回调函数。
- **事件驱动**：通过回调机制处理 I/O 操作完成的事件，适合处理大量短连接或需要高并发的场景。

**适用场景**：
- 适用于高并发、长连接和对实时性要求高的应用，例如大规模的文件传输服务。

**示例**：
```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.AsynchronousServerSocketChannel;
import java.nio.channels.AsynchronousSocketChannel;
import java.nio.channels.CompletionHandler;

public class AIOServer {
    public static void main(String[] args) throws IOException {
        AsynchronousServerSocketChannel serverChannel = AsynchronousServerSocketChannel.open();
        serverChannel.bind(new InetSocketAddress(8080));

        serverChannel.accept(null, new CompletionHandler<AsynchronousSocketChannel, Void>() {
            @Override
            public void completed(AsynchronousSocketChannel socketChannel, Void attachment) {
                serverChannel.accept(null, this); // 接受下一个连接
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                socketChannel.read(buffer, buffer, new CompletionHandler<Integer, ByteBuffer>() {
                    @Override
                    public void completed(Integer result, ByteBuffer buffer) {
                        buffer.flip();
                        socketChannel.write(buffer, buffer, new CompletionHandler<Integer, ByteBuffer>() {
                            @Override
                            public void completed(Integer result, ByteBuffer buffer) {
                                if (buffer.hasRemaining()) {
                                    socketChannel.write(buffer, buffer, this);
                                } else {
                                    buffer.clear();
                                    socketChannel.read(buffer, buffer, this);
                                }
                            }

                            @Override
                            public void failed(Throwable exc, ByteBuffer buffer) {
                                try {
                                    socketChannel.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        });
                    }

                    @Override
                    public void failed(Throwable exc, ByteBuffer buffer) {
                        try {
                            socketChannel.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                });
            }

            @Override
            public void failed(Throwable exc, Void attachment) {
                System.out.println("Failed to accept connection.");
            }
        });

        // 防止主线程退出
        while (true) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 总结

- **BIO（Blocking I/O）**：简单直接，适用于小规模、低并发的应用。
- **NIO（Non-blocking I/O）**：适合高并发场景，通过单线程管理多个连接，减少资源占用。
- **AIO（Asynchronous I/O）**：更高级的异步非阻塞 I/O 模型，适合超高并发和长连接的应用。

根据具体的应用场景和需求，选择合适的 I/O 模型可以显著提高系统性能和资源利用率。