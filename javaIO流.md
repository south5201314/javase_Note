[TOC]

# File类

## 常用构造器

* `public File(String pathname)`：以`pathname`为路径创建`File`对象，可以是绝对路径或者相对路径。
* `public File(String parent, String child)`：以`parent`为父路径，`child`为子路径创建`File`对象。
* `public File(File parent, String child)`：根据父`File`对象和子文件路径创建`File`对象。

## 常用方法

* `public String getAbsolutePath()`：获取绝对路径。
* `public String getPath()`：获取路径。
* `public String getName()` ：获取名称。
* `public String getParent()`：获取上层文件目录路径，若无，返回`null`。
* `public long length()`：获取文件长度（即：字节数），不能获取目录的长度。
* `public long lastModified()`：获取最后一次的修改时间，毫秒值。
* `public String[] list()` ：获取指定目录下的所有文件或者文件目录的名称数组。
* `public File[] listFiles()` ：获取指定目录下的所有文件或者文件目录的`File`数组。
* `public boolean renameTo(File dest)`：把文件重命名为指定的文件路径。
* `public boolean isDirectory()`：判断是否是文件目录。
* `public boolean isFile()` ：判断是否是文件。
* `public boolean exists()` ：判断是否存在 。
* `public boolean canRead()` ：判断是否可读。
* `public boolean canWrite()` ：判断是否可写。
* `public boolean isHidden()` ：判断是否隐藏。
* `public boolean createNewFile()` ：创建文件。若文件存在，则不创建，返回false  
* `public boolean mkdir()` ：创建文件目录。如果此文件目录存在，就不创建了。 如果此文件目录的上层目录不存在，也不创建。
* `public boolean mkdirs()` ：创建文件目录。如果上层文件目录不存在，一并创建。
* public boolean delete()：删除文件或者文件夹。文件目录内不能包含文件或者文件目录。

# IO流原理及分类

* 按操作数据单位不同分为：字节流(8 bit)，字符流(16 bit)。
* 按数据流的流向不同分为：输入流，输出流。
* 按流的角色的不同分为：节点流，处理流。

## 字节流

### 字节输入类

* `InputStream`：字节输入流抽象类(基类)。
  * `FileInputStream`：字节输入节点流(典型实现类)。

#### 常用方法

* `int read()`：从输入流中读取数据的下一个字节。返回 0 到 255 范围内的 `int` 字节值。如果因 为已经到达流末尾而没有可用的字节，则返回值 -1。

* `int read(byte[] b)`：从此输入流中将最多 `b.length` 个字节的数据读入 `byte` 数组中并返回实际读取的字节数。如果已经到达流末尾而没有可用的字节，则返回值 -1。

* `int read(byte[] b, int off, int len)`：将输入流中最多 `len` 个数据字节读入 `byte` 数组。尝试读取 `len` 个字节，但读取 的字节也可能小于该值。以整数形式返回实际读取的字节数。如果因为流位于 文件末尾而没有可用的字节，则返回值 -1。

* `public void close() throws IOException`：关闭此输入流并释放与该流关联的所有系统资源。

### 字节输出类

* `OutputStream`:字节输出流抽象类(基类)。
  * `FileOutputStream`：字节输出节点流(典型实现类)。

#### 常用方法

* `void write(int b)`：将指定的字节写入此输出流。write 的常规协定是：向输出流写入一个字节。要写 入的字节是参数 b 的八个低位。b 的 24 个高位将被忽略。 即写入0~255范围的。
* `void write(byte[] b)` ：将 `b.length` 个字节从指定的 byte 数组写入此输出流。write(b) 的常规协定是：应该与调用 `write(b, 0, b.length)` 的效果完全相同。
* `void write(byte[] b,int off,int len)` 将指定 `byte` 数组中从偏移量 `off` 开始的 `len` 个字节写入此输出流。
* `public void flush()throws IOException` ：刷新此输出流并强制写出所有缓冲的输出字节，调用此方法指示应将这些字节立 即写入它们预期的目标。
* `public void close() throws IOException` ：关闭此输出流并释放与该流关联的所有系统资源。

## 字符流

### 字符输入流

* `Reader`：字符输入流抽象类(基类)。
  
* `FileReader`：字符输入节点流(典型实现类)。
  
#### 常用方法

  *  `int read()`： 读取单个字符。作为整数读取的字符，范围在 0 到 65535 之间 (0x00-0xffff)（2个 字节的Unicode码），如果已到达流的末尾，则返回 -1 。
  * `int read(char[] cbuf)` ：将字符读入数组。如果已到达流的末尾，则返回 -1。否则返回本次读取的字符数。 
  * `int read(char[] cbuf,int off,int len)` ：将字符读入数组的某一部分。存到数组`cbuf`中，从`off`处开始存储，最多读`len`个字 符。如果已到达流的末尾，则返回 -1。否则返回本次读取的字符数。
  * `public void close() throws IOException` ：关闭此输入流并释放与该流关联的所有系统资源。


### 字符输出流

* `Writer`：字符输出流的抽象类(基类)。
  * `FileWriter`：字符输出节点流(典型实现类)。

#### 常用方法

* `void write(int c)` ：写入单个字符。要写入的字符包含在给定整数值的 16 个低位中，16 高位被忽略。 即写入0 到 65535 之间的Unicode码。
* `void write(char[] cbuf)`： 写入字符数组。
* `void write(char[] cbuf,int off,int len)` ：写入字符数组的某一部分。从`off`开始，写入`len`个字符。
* `void write(String str)` ：写入字符串。
* `void write(String str,int off,int len)` 写入字符串的某一部分。
* `void flush()`： 刷新该流的缓冲，则立即将它们写入预期目标。
* `public void close() throws IOException`： 关闭此输出流并释放与该流关联的所有系统资源。

## 处理流

### 缓冲流(重点)

*  为了提高数据读写的速度，Java API提供了带缓冲功能的流类，在使用这些流时，会创建一个内部缓冲区数组，缺省使用8192个字节(8Kb)的缓冲区。

  ```java
  public class BufferedInputStream extends FileInputStream {
      private static int defaultCharBufferSize = 8192;
  }
  ```

* 当读取数据时，数据按块读入缓冲区，其后的读操作则直接访问缓冲区。
* 当使用缓冲输入流读取文件时，缓冲输入流会一次性从文件中读取8192个(8Kb)字节(`bytey[8192]`/`char[8192]`)，存在缓冲区中，直到缓冲区装满了，才重新从文件中读取下一个8192个字节。
* 向流中写入字节时，不会直接写到文件，先写到缓冲区中直到缓冲区写满， 缓冲输出流才会把缓冲区中的数据一次性写到文件里。使用方法 `flush()`可以强制将缓冲区的内容全部写入输出流。
* 关闭流的顺序和打开流的顺序相反。只要关闭最外层流即可，关闭最外层流也会相应关闭内层节点流。
* `flush()`方法的使用：手动将`buffer`中内容写入文件。
* 如果是带缓冲区的流对象的`close()`方法，不但会关闭流，还会在关闭流之前刷新缓冲区，关闭后不能再写出。

* `BufferedInputStream` 和 `BufferedOutputStream` 
* `BufferedReader` 和 `BufferedWriter`
  * 新增方法
    * `public String readLine() throws IOException`：读取一行，但不包含换行符。
    * `public void newLine() throws IOException`：写入一个换行符。

### 转换流

* `InputStreamReader`：将`InputStream`转换为`Reader`：字节输入流转换为字符输入流。
* `OutputStreamWriter`：将`Writer`转换为`OutputStream`：字符输出流转换为字节输出流。
* 很多时候我们使用转换流来处理文件乱码问题。实现编码和 解码的功能。

### 标准输入、输出流

* `System.in`和`System.out`分别代表了系统标准的输入和输出设备。

* 默认输入设备是：键盘，输出设备是：显示器。

* `System.in`的类型是`InputStream`。

* `System.out`的类型是`PrintStream`，其是`OutputStream`的子类 `FilterOutputStream` 的子类。

* 重定向：通过`System`类的`setIn`，`setOut`方法对默认设备进行改变。

  * `public static void setIn(InputStream in)` 

  * `public static void setOut(PrintStream out)`

### 打印流

* 实现将基本数据类型的数据格式转化为字符串输出。
* 打印流：`PrintStream`和`PrintWriter`。
* 提供了一系列重载的`print()`和`println()`方法，用于多种数据类型的输出。
* `PrintStream`和`PrintWriter`的输出不会抛出`IOException`异常。
* `PrintStream`和`PrintWriter`有自动`flush`功能。
* `PrintStream` 打印的所有字符都使用平台的默认字符编码转换为字节。 在需要写入字符而不是写入字节的情况下，应该使用 `PrintWriter` 类。
* `System.out`返回的是`PrintStream`的实例。

### 数据流

* 为了方便地操作Java语言的基本数据类型和`String`的数据，可以使用数据流。
* 数据流有两个类：(用于读取和写出基本数据类型、`String`类的数据）
  * `DataInputStream` 和 `DataOutputStream`。
  * 分别“套接”在 `InputStream` 和 `OutputStream` 子类的流上。

#### 常用方法

读取的方法：

* `boolean readBoolean()` 
* `byte readByte()` 
* `char readChar()` 
* `float readFloat()` 
* `double readDouble()` 
* `short readShort()` 
* `long readLong()` 
* `int readInt()` 
* `String readUTF()` 
* `void readFully(byte[] b)`

写入的方法：将上述的方法的`read`改为相应的`write`即可。

### 对象流(重点)

* `ObjectInputStream`和`OjbectOutputSteam`

#### 序列化

* 序列化：用`ObjectOutputStream`类保存基本类型数据或对象的机制。
* 反序列化：用`ObjectInputStream`类读取基本类型数据或对象的机制。
* `ObjectOutputStream`和`ObjectInputStream`不能序列化`static`和`transient`修 饰的成员变量。
* 对象序列化机制允许把内存中的Java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传 输到另一个网络节点。
* 需要序列化的对象所属的类必须实现`Serializable`接口，并声明一个序列化版本标识的静态变量：`private static final long serialVersionUID`；如果类没有显式定义标识，java虚拟机会自动根据类中的结构(属性、方法等)生成。若类的实例变量做了修改，serialVersionUID 可能发生变化。

#### 对象流的使用

* 序列化

  * 创建一个 `ObjectOutputStream`。

  * 调用 `ObjectOutputStream` 对象的 `writeObject(Object o)` 方法输出可序列化对象。
  * 注意写出一次，操作`flush()`一次。

* 反序列化
  * 创建一个 `ObjectInputStream`。
  * 调用 `Object readObject()` 方法读取流中的对象。

* 如果某个类的属性不是基本数据类型或 `String` 类型，而是另一个 引用类型，那么这个引用类型必须是可序列化的，否则拥有该类型的 `Field` (字段)的类也不能序列化。

## 随机存取文件流

* `RandomAccessFile` 声明在`java.io`包下，但直接继承于`java.lang.Object`类。并 且它实现了`DataInput`、`DataOutput`这两个接口，也就意味着这个类既可以读也 可以写。 

* `RandomAccessFile` 类支持 “随机访问” 的方式，程序可以直接跳到文件的任意地方来读、写文件：

  * 支持只访问文件的部分内容。

  * 可以向已存在的文件后追加内容 。

* `RandomAccessFile` 对象包含一个记录指针，用以标示当前读写处的位置。 

* `RandomAccessFile` 类对象可以自由移动记录指针。

* `long getFilePointer()`：获取文件记录指针的当前位置。

* `void seek(long pos)`：将文件记录指针定位到 pos 位置。

* 创建 `RandomAccessFile` 类实例需要指定一个 mode 参数，该参数指 定 `RandomAccessFile` 的访问模式： 
* `r`: 以只读方式打开。
* `rw`：打开以便读取和写入。
* `rwd`:打开以便读取和写入；同步文件内容的更新。
* `rws`:打开以便读取和写入；同步文件内容和元数据的更新。
* 如果模式为只读`r`。则不会创建文件，而是会去读取一个已经存在的文件， 如果读取的文件不存在则会出现异常。 如果模式为`rw`读写。如果文件不 存在则会去创建文件，如果存在则不会创建。

# Java NIO

* Java API中提供了两套NIO，一套是针对标准输入输出NIO，另一套就是网络编程NIO。
  * `FileChannel`:处理本地文件
  * `SocketChannel`：TCP网络编程的客户端的Channel 
  * `ServerSocketChannel`：TCP网络编程的服务器端的Channel
  * `DatagramChannel`：UDP网络编程中发送端和接收端的Channel

## Path、Paths和Files核心API

### Path

* `static Path get(String first, String … more)` : 用于将多个字符串串连成路径。
* `static Path get(URI uri)`: 返回指定uri对应的`Path`路径。
* `String toString()` ： 返回调用 Path 对象的字符串表示形式。
* `boolean startsWith(String path)` : 判断是否以 `path` 路径开始。
* `boolean endsWith(String path)` : 判断是否以 `path` 路径结束。
* `boolean isAbsolute()` : 判断是否是绝对路径。
* `Path getParent()` ：返回`Path`对象包含整个路径，不包含 `Path` 对象指定的文件路径。
* `Path getRoot()` ：返回调用 `Path` 对象的根路径。
* `Path getFileName()` : 返回与调用 `Path` 对象关联的文件名。
* `int getNameCount()` : 返回`Path` 根目录后面元素的数量。
* `Path getName(int idx)` : 返回指定索引位置 idx 的路径名称。
* `Path toAbsolutePath()` : 作为绝对路径返回调用 `Path` 对象。
* `Path resolve(Path p)` :合并两个路径，返回合并后的路径对应的`Path`对象。
* `File toFile()`: 将`Path`转化为`File`类的对象。

### Files类(操作文件或目录的工具类)

* `Path copy(Path src, Path dest, CopyOption … how)` : 文件的复制。
* `Path createDirectory(Path path, FileAttribute … attr)` : 创建一个目录。
* `Path createFile(Path path, FileAttribute … arr)` : 创建一个文件。
* `void delete(Path path)` : 删除一个文件/目录，如果不存在，执行报错。
* `void deleteIfExists(Path path) :` Path对应的文件/目录如果存在，执行删除。
* `Path move(Path src, Path dest, CopyOption…how)` : 将 `src` 移动到 `dest` 位置。
* `long size(Path path)` : 返回 `path` 指定文件的大小。
* `boolean exists(Path path, LinkOption … opts)` : 判断文件是否存在。
* `boolean isDirectory(Path path, LinkOption … opts)` : 判断是否是目录。
* `boolean isRegularFile(Path path, LinkOption … opts)` : 判断是否是文件。
* `boolean isHidden(Path path)` : 判断是否是隐藏文件。
* `boolean isReadable(Path path)` : 判断文件是否可读。
* `boolean isWritable(Path path)` : 判断文件是否可写。
* `boolean notExists(Path path, LinkOption … opts)` : 判断文件是否不存在。
* `SeekableByteChannel newByteChannel(Path path, OpenOption…how)` : 获取与指定文件的连接，how 指定打开方式。
* `DirectoryStream newDirectoryStream(Path path)` ：打开 `path` 指定的目录。
*  `InputStream newInputStream(Path path, OpenOption…how)`：获取 `InputStream` 对象。
* `OutputStream newOutputStream(Path path, OpenOption…how)` : 获取 `OutputStream` 对象。