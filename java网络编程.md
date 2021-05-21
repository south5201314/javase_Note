# 网络通信要素概述

## IP 和 端口号

* IP 地址：`InetAddress`。
* 端口号标识正在计算机上运行的进程（程序）。
* 端口号与IP地址的组合得出一个网络套接字：`Socket`。

### `InetAddress`类

* Internet上的主机有两种方式表示地址：

  * 域名(hostName)：www.atguigu.com

  * IP 地址(hostAddress)：202.108.35.210

* 获取 `InetAddress`实例：
  * `public static InetAddress getLocalHost()`
  * `public static InetAddress getByName(String host)`

* 常用方法
  * `public String getHostAddress()`：返回 IP 地址字符串（以文本表现形式）。
  * `public String getHostName()`：获取此 IP 地址的主机名。
  * `public boolean isReachable(int timeout)`：测试是否可以达到该地址。

## TCP/IP协议

* 传输控制协议TCP(Transmission Control Protocol) 。
  * 使用TCP协议前，须先建立TCP连接，形成传输数据通道。
  * 传输前，采用“三次握手”方式，点对点通信，是可靠的 。
  * TCP协议进行通信的两个应用进程：客户端、服务端。
  * 在连接中可进行大数据量的传输。
  * 传输完毕，需释放已建立的连接，效率低。
* 用户数据报协议UDP(User Datagram Protocol)。
  * 将数据、源、目的封装成数据包，不需要建立连接。
  * 每个数据报的大小限制在64K内 。
  * 发送不管对方是否准备好，接收方收到也不确认，故是不可靠的 。
  * 可以广播发送。
  * 发送数据结束时无需释放资源，开销小，速度快。

* Socket分类：
* 流套接字（stream socket）：使用TCP提供可依赖的字节流服务。
* 数据报套接字（datagram socket）：使用UDP提供“尽力而为”的数据报服务。

### TCP

#### 构造器

* `public Socket(InetAddress address,int port)`：创建一个流套接字并将其连接到指定 IP 地址的指定端口号。
* `public Socket(String host,int port)`：创建一个流套接字并将其连接到指定主机上的指定端口号。

#### 常用方法

* `public InputStream getInputStream()`：返回此套接字的输入流。可以用于接收网络消息。
* `public OutputStream getOutputStream()`：返回此套接字的输出流。可以用于发送网络消息。
* `public InetAddress getInetAddress()`：此套接字连接到的远程 IP 地址；如果套接字是未连接的，则返回 `null`。 
* `public InetAddress getLocalAddress()`：获取套接字绑定的本地地址。 即本端的IP地址。
* `public int getPort()`：此套接字连接到的远程端口号；如果尚未连接套接字，则返回 0。
* `public int getLocalPort()`：返回此套接字绑定到的本地端口。 如果尚未绑定套接字，则返回 -1。即本端的端口号。
* `public void close()`：关闭此套接字。套接字被关闭后，便不可在以后的网络连接中使用（即无法重新连接或重新绑定）则需要创建新的套接字对象。 关闭此套接字也将会关闭该套接字的 `InputStream`和 `OutputStream`。
* `public void shutdownInput()`：如果在套接字上调用 `shutdownInput()` 后从套接字输入流读取内容，则流将返回 EOF（文件结束符）。 即不能在从此套接字的输入流中接收任何数据。
* `public void shutdownOutput()`：禁用此套接字的输出流。对于 TCP 套接字，任何以前写入的数据都将被发送，并且后跟 TCP 的正常连接终止序列。 如果在套接字上调用 `shutdownOutput()` 后写入套接字输出流， 则该流将抛出 `IOException`。 即不能通过此套接字的输出流发送任何数据。

### UDP

* 类 `DatagramSocket` 和 `DatagramPacket` 实现了基于 UDP 协议网络程序。
* UDP数据报通过数据报套接字 `DatagramSocket` 发送和接收，系统不保证 UDP数据报一定能够安全送到目的地，也不能确定什么时候可以抵达。
* `DatagramPacket` 对象封装了UDP数据报，在数据报中包含了发送端的IP 地址和端口号以及接收端的IP地址和端口号。 
* UDP协议中每个数据报都给出了完整的地址信息，因此无须建立发送方和 接收方的连接。如同发快递包裹一样。

#### DatagramSocket 类

* 构造器

  * `public DatagramSocket(int port)`：创建数据报套接字并将其绑定到本地主机上的指定端口。套接字将被绑定到通配符地址，IP 地址由内核来选择。

  * `public DatagramSocket(int port,InetAddress laddr)`：创建数据报套接字，将其绑定到指定的本地地址。 本地端口必须在 0 到 65535 之间（包括两者）。如果 IP 地址为 0.0.0.0，套接字将被绑定到通配符地 址，IP 地址由内核选择。

* 常用方法
  * `public void close()`：关闭此数据报套接字。
  * `public void send(DatagramPacket p)`：从此套接字发送数据报包。`DatagramPacket` 包含的信息指示：将要发送的数据、其长度、远程主机的 IP 地址和远程主机的端口号。
  * `public void receive(DatagramPacket p)`：从此套接字接收数据报包。当此方法返回时，`DatagramPacket` 的缓冲区填充了接收的数据。数据报包也包含发送方的 IP 地址和发送方机器上的端口号。 此方法在接收到数据报前一直阻塞。数据报包对象的 `length` 字段包含所接收信息的长度。如果信息比包的长度长，该信息将被截短。
  * `public InetAddress getLocalAddress()`：获取套接字绑定的本地地址。
  * `public int getLocalPort()`：返回此套接字绑定的本地主机上的端口号。
  * `public InetAddress getInetAddress()`：返回此套接字连接的地址。如果套接字未连接，则返回 `null`。
  * `public int getPort()`：返回此套接字的端口。如果套接字未连接，则返回 -1。

#### DatagramPacket类

* 构造器
  * `public DatagramPacket(byte[] buf,int length)`：构造 `DatagramPacket`，用来接收长度为 `length` 的数据包。 `length` 参数必须小于等于 `buf.length`。
  * `public DatagramPacket(byte[] buf,int length,InetAddress address,int port)`：构造数据报包，用来将长度为 `length` 的包发送到指定主机上的指定端口号。`length` 参数必须小于等于 `buf.length`。

* 常用方法
  * `public InetAddress getAddress()`：返回某台机器的 IP 地址，此数据报将要发往该机器或者是从该机器接收到的。
  * `public int getPort()`：返回某台远程主机的端口号，此数据报将要发往该主机或 者是从该主机接收到的。 
  * `public byte[] getData()`：返回数据缓冲区。接收到的或将要发送的数据从缓冲区中的偏移量 `offset` 处开始，持续 `length` 长度。
  * `public int getLength()`：返回将要发送或接收到的数据的长度。

# URL类

* URL(Uniform Resource Locator)：统一资源定位符，它表示 Internet 上某一 资源的地址。
* URL的基本结构由5部分组成： <传输协议>://<主机名>:<端口号>/<文件名>#片段名?参数列表。

## 构造器

* `public URL (String spec)`：通过一个表示URL地址的字符串可以构造一个URL对象。例 如：`URL url = new URL ("http://www.sunflower.com/")`。
* `public URL(URL context, String spec)`：通过基 URL 和相对 URL 构造一个 URL 对象。 例如：`URL downloadUrl = new URL(url, “download.html") `。
* `public URL(String protocol, String host, String file)`： 例如：`new URL("http", "www.sunflower.com", “download. html")`。
* `public URL(String protocol, String host, int port, String file)`：例如: `URL gamelan = new URL("http", "www.sunflower.com", 80, “download.html")`。

## 常用方法

* `public String getProtocol()`：获取该URL的协议名。
* `public String getHost()`：获取该URL的主机名。
* `public String getPort()`：获取该URL的端口号。
* `public String getPath()`：获取该URL的文件路径。
* `public String getFile()`：获取该URL的文件名。
* `public String getQuery()`：获取该URL的查询名。

# URLConnection类

* URL的方法 `openStream()`：能从网络上读取数据。
* `URLConnection`：表示到`URL`所引用的远程对象的连接。当与一个`URL`建立连接时， 首先要在一个 `URL` 对象上通过方法 `openConnection()` 生成对应的 `URLConnection` 对象。如果连接过程失败，将产生`IOException`。

## 常用方法

* `public Object getContent( ) throws IOException`
* `public int getContentLength( )`
* `public String getContentType( )`
* `public long getDate( )`
* `public long getLastModified( )`
* `public InputStream getInputStream( )throws IOException`
* `public OutputSteram getOutputStream( )throws IOException`