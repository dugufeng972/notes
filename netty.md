## NIO基础

> NIO：non-blocking io非阻塞IO

### 三大组件

1. Channel & Buffer
   
   > Channel类似于stream，它是读写数据的双向通道，可以从channel将数据读入buffer，也可以将buffer的数据写入channel，而stream要么是输入，要么是输出，channel比stream更底层

常见的Channel有：

- `FileChannel`
- `DatagramChannel`
- `SocketChannel`
- `ServerSocketChannel`

buffer则用来缓冲读写数据，常见的buffer有：

- `ByteBuffer`
  - `MappedByteBuffer`
  - `DirectByteBuffer`
  - `HeapByteBuffer`
- `ShortBuffer`
- `IntBuffer`
- `LongBuffer`
- `FloatBuffer`
- `DoubleBuffer`
- `CharBuffer`

Buffer使用案例：
<img src="./pic/netty/屏幕截图 2025-08-21 210916.png">

2. Selector
   
   > selector的作用就是配合一个线程来管理多个channel，获取这些channel上发生的事件，这些channel工作在非阻塞模式下的线程上，一个channel发生了阻塞selector会让线程转移去执行另一个已经就绪的channel(线程池模式下，正在执行的channel发生了阻塞，线程也会跟着阻塞)，不会让线程吊死在一个channel上。适合连接数特别多，但流量低的场景。

<img src="./pic/netty/屏幕截图 2025-08-21 204703.png">

> 调用selector的select()会阻塞直到channel发生了读写就绪事件，这些事件发生，select方法就会返回这些事件交给thread来处理

### ByteBuffer

buffer中的数据读过之后，如果条件符合会自动被覆盖

使用：

```java
public class TestByteBuffer {
    public static void main(String[] args) {
        // 1. 输入流
        try (FileChannel channel = new FileInputStream("data.txt").getChannel()){
            // 准备缓冲区
            ByteBuffer buffer = ByteBuffer.allocate(10);
            while (true) {
                // 从channel读取数据，写入buffer中
                int len = channel.read(buffer);
                System.out.println("读取的字节数"+len);
                // len为-1时表示全部读取
                if (len == -1) break;
                // 打印buffer的内容
                buffer.flip();  // 切换到读模式
                while (buffer.hasRemaining()) { // 是否还有数据，不会脏读
                    byte b = buffer.get();  // 读取数据
                    System.out.println((char) b);
                }
                buffer.clear(); // 切换为写模式
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

#### ByteBuffer结构

ByteBuffer有以下重要属性：

- capacity
- position
- limit 

开始时：
<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 144549.png">

写模式下，下图表示写入了4个字节后的状态
<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 144813.png">

filp动作发生后
<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 144911.png">

读取4个字节后
<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 145202.png">

clear动作后
<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 145237.png">

compact方法，是把未读完的部分向前压缩，然后切换至写模式
<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 145657.png">

#### ByteBuffer常用方法

1. 分配容量 
   
   1. allocate -- 分配的是java堆内存，读写效率较低，受到gc的影响
   
   2. allocateDirect -- 分配的是直接内存，读写效率搞（少一次拷贝），不是gc影响，分配的效率低，可能会造成内存泄漏

2. 写入数据
   
   1. 调用channel的read方法
   2. 调用buffer自己的put方法

3. 读取方法
   
   1. 调用channel的write方法
   2. 调用buffer自己的get方法

4. 字符串和ByteBuffer之间的转换

```java
public class TestByteBufferString {
    public static void main(String[] args) {
        // 1. 字符串转为bytebuffer
        // 定义buffer
        ByteBuffer buffer = ByteBuffer.allocate(10);
        // 字符串通过字符数组存入buffer
        buffer.put("hello".getBytes());

        // 2. 字符串转为bytebuffer
        // 自动转成读模式
        ByteBuffer buffer1 = StandardCharsets.UTF_8.encode("hello");

        // 3. wrap方法来string转buffer
        // 自动转成读模式
        ByteBuffer buffer2 = ByteBuffer.wrap("hello".getBytes());

        // buffer转string
        // buffer必须要切换为读模式
        String str1 = StandardCharsets.UTF_8.decode(buffer1).toString();
        System.out.println(str1);
    }
}
```

#### 分散读取

集中读，将一个文件的内容同时读取到不同的buffer中

```java
public class TestScatteringReads {
    public static void main(String[] args) {
        // words.txt文件中内容是：onetwothree
        try (FileChannel channel = new RandomAccessFile("words.txt", "r").getChannel()) {
            // 通过设置buffer容量来控制每个buffer读取多少
            ByteBuffer buffer1 = ByteBuffer.allocate(3);
            ByteBuffer buffer2 = ByteBuffer.allocate(3);
            ByteBuffer buffer3 = ByteBuffer.allocate(5);
            // 集中从channel中读
            long r = channel.read(new ByteBuffer[]{buffer1, buffer2, buffer3});
            System.out.println(r);
        } catch (IOException e) {
        }
    }
}
```

集中写，将多个buffer中的内容写入到channel中

```java
public class TestScatteringWrites {
    public static void main(String[] args) {
        ByteBuffer buffer1 = StandardCharsets.UTF_8.encode("hello");
        ByteBuffer buffer2 = StandardCharsets.UTF_8.encode("netty");
        ByteBuffer buffer3 = StandardCharsets.UTF_8.encode("world");

        try (FileChannel channel = new RandomAccessFile("words.txt", "rw").getChannel()) {
            // 集中写
            long write = channel.write(new ByteBuffer[]{buffer1, buffer2, buffer3});
        } catch (IOException e) {
        }
    }
}
```

#### 黏包，半包分析

> 案例：
>   网络上有多条数据发送给服务器，数据之间使用 \n 进行分隔
>   Hello,world\n
>   I'm zhangsan\n
>   How are you?\n
>   但是由于某种原因这些数据在接收时，被进行了重新组合，变成了下面的两个bytebuffer（黏包，半包）
>   Hello,world\nI'm zhangsan\nHo
>   w are you?\n

- 黏包原因：为了效率，网络将上述三条消息整合到一起发送出去
- 半包原因：服务器的buffer大小是有限的，整合到一起的数据超出了buffer大小，所以被迫读入到两个bytebuffer中去了

### 文件编程

#### FileChannel

> `FileChannel`只能工作在阻塞模式下

> ==形象的理解：== 将文件内容通过管道(channel)写入池中(buffer)

**获取**

- 不能直接打开FileChannel，必须通过FileInputStream、FileOutputStream或者RandomAccessFile来获取FileChannel，它们都有getChannel方法
  - 通过FileInputStream获取channel只读
  - 通过FileOutStream获取channel只能写
  - 通过RandomAccessFile是否能读写根据构造RandomAccessFile时的读写模式决定

**读取**
会从channel读取数据填充ByteBuffer，返回值表示读取到了多少字节，-1表示到达了文件的末尾

```java
int readBytes = channel.read(buffer);
```

**关闭**
channel必须关闭，不过调用了FileInputStream、FileOutStream或者RandomAccessFile的close方法会间接的调用channel的close方法

**位置**
获取当前位置

```java
long pos = channel.position();
// 设置位置
long newPos = 20;
channel.position(newPos);
```

设置当前位置时，如果设置为文件的末尾：

- 这是读取会返回-1
- 写入

**获取大小**
使用size方法获取文件的大小

**强制写入**
写入管道的数据操作系统出于性能考虑，会将数据缓存，不是立刻写入磁盘。可以调用force(true)方法将文件内容和元数据(文件的权限等信息)立刻写入磁盘。

#### 两个Channel传输数据

> 本质上：是通过两个管道向背后的文件传输数据

```java
public class TestFileChannelTransferTo {
    public static void main(String[] args) {
        try {
            // 获取连向data.txt文件的输入管道
            FileChannel from = new FileInputStream("data.mp4").getChannel();
            // 获取连向to.txt文件的输出管道
            FileChannel to = new FileOutputStream("to.mp4").getChannel();
            // 效率高，会进行零拷贝优化
            //from.transferTo(0, from.size(), to);
            // channel空间有限，如果数据过大，可以使用下面的方法
            long size = from.size();
            for (long left = size; left > 0;) {
                System.out.println("position:"+(size - left) + "剩余:" + left);
                left -= from.transferTo((size - left), left, to);
            }
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

#### Path

> 路径类
> jdk7引入了Path和Paths类
> 
> - Path用来表示文件路径
> - Paths是工具类，用来获取Path实例
> 
> 支持特殊符号：
> 
> - . :代表了当前路径
> - .. :代表了上一级路径

```java
public class TestPath {
    public static void main(String[] args) {
        // 相对路径
        Path path1 = Paths.get("./TestByteBuffer.java");
        System.out.println(path1);
        // 绝对路径
        Path path2 = Paths.get("C:\\DumpStack.log");
        // 表示C:\Users\dwl\projects
        Path path3 = Paths.get("C:\\Users\\dwl", "projects");

        Path path4 = Paths.get("C:\\Users\\dwl\\..");
        System.out.println(path4.normalize());  // C:\Users
    }
}
```

#### Files

检查文件是否存在

**遍历文件夹**

```java
public class TestFilesWalkFileTree {
    public static void main(String[] args) {
        // 遍历文件夹
        try {
            // 匿名内部类，不能访问外部非final属性，可以使用如下的包装类，或者使用final修饰的引用类型（方便更改）
            AtomicInteger dirCount = new AtomicInteger();
            AtomicInteger fileCount = new AtomicInteger();
            Files.walkFileTree(Paths.get("C:\\msys64\\ucrt64"), new SimpleFileVisitor<Path>(){
                // 还有其它方法，这里只展示两个
                // 进入文件夹时执行
                @Override
                public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) throws IOException {
                    System.out.println("====>"+dir);
                    dirCount.incrementAndGet();
                    return super.preVisitDirectory(dir, attrs);
                }
                // 访问文件时执行
                @Override
                public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
                    System.out.println(file);
                    fileCount.incrementAndGet();
                    return super.visitFile(file, attrs);
                }
            });
            System.out.println("dirCount:"+dirCount+"\n"+"fileCount:"+fileCount);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

### 网络编程

#### 阻塞 vs 非阻塞

**阻塞演示**

> 在处理一个比较耗时的连接时不会去处理其它连接

```java
// 服务端
public class Server {
    public static void main(String[] args) {
        try {
            // 创建buffer
            ByteBuffer buffer = ByteBuffer.allocate(16);
            // 1. 创建服务器
            ServerSocketChannel ssc = ServerSocketChannel.open();
            // 2. 绑定端口
            ssc.bind(new InetSocketAddress(8080));
            // 3. 连接集合
            List<SocketChannel> channels = new ArrayList<>();
            while (true) {
                // accept建立与客户端的连接
                System.out.println("等待连接...");
                SocketChannel sc = ssc.accept();    // 没有连接会阻塞
                System.out.println("已连接...");
                channels.add(sc);
                for (SocketChannel channel : channels) {
                    // 接收数据
                    channel.read(buffer);   // 阻塞方法，读取通道中的数据，没有数据则阻塞，这里阻塞会导致其它请求不会被处理，直到这次请求完成
                    buffer.flip();
                    while (buffer.hasRemaining()) {
                        System.out.print(buffer.get());
                    }
                    buffer.clear();
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

    }
}
```

**非阻塞**

> 这里非阻塞代码有个问题是，没有请求时一直在跑循环，造成资源浪费

```java
public class Server {
    public static void main(String[] args) {
        try {
            // 创建buffer
            ByteBuffer buffer = ByteBuffer.allocate(16);
            // 1. 创建服务器
            ServerSocketChannel ssc = ServerSocketChannel.open();
            ssc.configureBlocking(false);   // 设置为非阻塞模式
            // 2. 绑定端口
            ssc.bind(new InetSocketAddress(8080));
            // 3. 连接集合
            List<SocketChannel> channels = new ArrayList<>();
            while (true) {
                // accept建立与客户端的连接
                SocketChannel sc = ssc.accept();    // 不会阻塞，如果没有连接sc为null
                if (sc != null) {
                    channels.add(sc);
                    sc.configureBlocking(false);    // 设置非阻塞模式
                }
                for (SocketChannel channel : channels) {
                    // 接收数据
                    int read = channel.read(buffer);   // 非阻塞，如果读取到数据返回读取的数据长度
                    if (read > 0) {
                        buffer.flip();
                        while (buffer.hasRemaining()) {
                            System.out.print(buffer.get());
                        }
                        buffer.clear();
                    }
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

#### selector

> selector是非阻塞的，在没有想要的事件触发时代码是阻塞的，解决了上在没有请求是也在执行while循环的问题

==多路复用==：单线程配合Selector完成对多个Channel可读写事件的监控，这称之为多路复用

- 多路复用仅针对于网络IO、普通文件IO没法利用多路复用

selector的select方法在没有发生监听事件时会阻塞

select什么时候不阻塞

- 事件发生时
  - 客户端发起连接请求，会触发accept事件
  - 客户端发送数据、客户端异常关闭时（如果不取消read监听，会持续触发），都会触发read事件，另外如果发送的数据大于buffer缓冲区，会触发多次读取事件
  - channel 可写，会触发 write 事件
  - 在 linux 下 nio bug 发生时
- 调用 selector.wakeup()
- 调用 selector.close()
- selector 所在线程 interrupt

会触发selector的事件类型：

- accept - 在有连接请求时触发
- connet - 在客户端侧，连接建立后触发
- read - 可读事件
- write - 可写事件

演示案例 -- accept事件类型，也有cancel取消事件

```java
public class TestSelector {
    public static void main(String[] args) {
        try {
            // 1. 创建selector来管理多个channel
            Selector selector = Selector.open();

            ByteBuffer buffer = ByteBuffer.allocate(16);
            ServerSocketChannel ssc = ServerSocketChannel.open();
            // 取消阻塞
            ssc.configureBlocking(false);

            // 2. 建立selector和channel的联系（注册）
            // sscKey在触发事件后，通过这个值可以知道事件类型和哪个channel
            SelectionKey sscKey = ssc.register(selector, 0, null);
            // 设置只关注accept事件
            sscKey.interestOps(SelectionKey.OP_ACCEPT);

            ssc.bind(new InetSocketAddress(8080));
            while (true) {
                // 3. select方法，没有事件发生，线程阻塞，有事件线程才会运行
                selector.select();
                // 4. 处理事件，selectedKeys返回所有被触发的事件key（set集合），因为要删除操作所以使用迭代器遍历
                Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
                while (iterator.hasNext()) {
                    // 获取key
                    SelectionKey key = iterator.next();
                    // 通过key获取对应channel
                    ServerSocketChannel channel = (ServerSocketChannel) key.channel();
                    // 下面处理事件，执行业务代码
                    // 如果不处理，下次selector.select();不会阻塞
                    SocketChannel sc = channel.accept();
                    // ...
                    // 不处理事件，取消监听事件
                    key.cancel();
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

处理可读事件

```java
public class TestSelector {
    public static void main(String[] args) {
        try {
            // 1. 创建selector来管理多个channel
            Selector selector = Selector.open();

            ByteBuffer buffer = ByteBuffer.allocate(16);
            ServerSocketChannel ssc = ServerSocketChannel.open();
            ssc.configureBlocking(false);

            // 2. 建立selector和channel的联系（注册）
            // sscKey在触发事件后，通过这个值可以知道事件类型和哪个channel
            SelectionKey sscKey = ssc.register(selector, 0, null);
            // 设置只关注accept事件
            sscKey.interestOps(SelectionKey.OP_ACCEPT);

            ssc.bind(new InetSocketAddress(8080));
            while (true) {
                // 3. select方法，没有事件发生，线程阻塞，有事件线程才会运行
                selector.select();
                // 4. 处理事件，selectedKeys返回所有被触发的事件key（set集合），因为要删除操作所以使用迭代器遍历
                // selector只会向selectedKeys中添加不会主动删除，需要自己删除
                Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
                while (iterator.hasNext()) {
                    // 获取key
                    SelectionKey key = iterator.next();
                    // 主动删除这个key，防止下次遍历到这个key，执行错误的分支
                    iterator.remove();
                    // 区分事件类型
                    // 实例中有简化，事实上只有accept事件
                    if (key.isAcceptable()) {
                        // 通过key获取对应channel，处理该事件
                        ServerSocketChannel channel = (ServerSocketChannel) key.channel();

                        SocketChannel sc = channel.accept();
                        sc.configureBlocking(false);
                        // 注册为关注read事件
                        SelectionKey scKey = sc.register(selector, 0, null);
                        scKey.interestOps(SelectionKey.OP_READ);    // 这里执行完，回到selector.select(); 等待可读事件或者处理其它事件
                        System.out.println("成功建立连接，并监听read事件");
                    } else if (key.isReadable()) {  // 处理上面注册的read事件
                        try {
                            ByteBuffer buffer1 = ByteBuffer.allocate(16);
                            SocketChannel channel = (SocketChannel) key.channel();
                            System.out.println("触发read事件，等待数据...");
                            // 读取channel
                            int read = channel.read(buffer1);
                            // 客户端没有写入数据直接断开时（客户端socket调用close），取消监听可读事件
                            if (read == -1) {
                                key.cancel();
                            } else {
                                // 业务代码
                            }
                        } catch (IOException e) {
                            e.printStackTrace();
                            // 防止出现异常该事件无法完成，进而导致异常循环触发出现
                            /* 因为例如：客户端网络断开 会导致服务端tcp收到一个read事件，selector不会阻塞进入下一个循环，并在selectedKeys中添加一个key
                                由于selectedKeys对应的channel出现异常（因为客户端断开）selector会认为该channel仍然有事件要处理
                                这里解释不清，但是出现异常时，必须要cancel事件（停止监听）
                             */
                            key.cancel();
                        }
                    }
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

**处理可写事件**

在没有使用可写事件时

```java
public class WriteServer {
    public static void main(String[] args) {
        try {
            ServerSocketChannel ssc = ServerSocketChannel.open();
            ssc.configureBlocking(false);

            Selector selector = Selector.open();
            ssc.register(selector, SelectionKey.OP_WRITE);

            ssc.bind(new InetSocketAddress(8080));
            while (true) {
                selector.select();
                Iterator<SelectionKey> iter = selector.selectedKeys().iterator();
                while (iter.hasNext()) {
                    SelectionKey key = iter.next();
                    iter.remove();
                    if (key.isAcceptable()) {
                        SocketChannel sc = ssc.accept();
                        sc.configureBlocking(false);
                        // sc.register(selector, SelectionKey.OP_WRITE);
                        // 向客户端发送的数据
                        StringBuilder sb = new StringBuilder();
                        for (int i = 0; i < 300000; i++) {
                            sb.append("a");
                        }
                        ByteBuffer buffer = StandardCharsets.UTF_8.encode(sb.toString());

                        // 这里有个问题：就是客户端的buffer满了，会导致服务端无法继续发送，直到客户端清除buffer
                        // 在无法发生期间，程序会在这里不断循环，导致资源浪费
                        while (buffer.hasRemaining()) {
                            int write = sc.write(buffer);
                        }
                    }
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

使用可写事件时

```java
// write事件
public class WriteServer {
    public static void main(String[] args) {
        try {
            ServerSocketChannel ssc = ServerSocketChannel.open();
            ssc.configureBlocking(false);

            Selector selector = Selector.open();
            ssc.register(selector, SelectionKey.OP_WRITE);

            ssc.bind(new InetSocketAddress(8080));
            while (true) {
                selector.select();
                Iterator<SelectionKey> iter = selector.selectedKeys().iterator();
                while (iter.hasNext()) {
                    SelectionKey key = iter.next();
                    iter.remove();
                    if (key.isAcceptable()) {
                        SocketChannel sc = ssc.accept();
                        sc.configureBlocking(false);
                        SelectionKey scKey = sc.register(selector, 0, null);
                        // 关注可读事件
                        scKey.interestOps(SelectionKey.OP_READ);
                        // 向客户端发送的数据
                        StringBuilder sb = new StringBuilder();
                        for (int i = 0; i < 300000; i++) {
                            sb.append("a");
                        }
                        ByteBuffer buffer = StandardCharsets.UTF_8.encode(sb.toString());
                        int write = sc.write(buffer);
                        // 判断是否还有剩余
                        if (buffer.hasRemaining()) {
                            // 关注可写事件和可读事件
                            scKey.interestOps(scKey.interestOps() + SelectionKey.OP_WRITE);
                            // 把未写完的buffer挂载到附件上
                            scKey.attach(buffer);
                        }
                    } else if (key.isWritable()) {  // 等客户端那边清理的buffer，会发送一个write事件
                        // 继续写
                        ByteBuffer buf = (ByteBuffer) key.attachment();
                        SocketChannel sc1 = (SocketChannel) key.channel();
                        int w = sc1.write(buf); // 这里再次写满了，会到上面selector.select();等待
                        // 如果写完了，清理附件和关注事件
                        if (!buf.hasRemaining()) {
                            key.attach(null);
                            // 清理关注事件
                            key.interestOps(key.interestOps() - SelectionKey.OP_WRITE);
                        }
                    }
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

#### 消息边界问题

> 像中文这样由多个字节表示一个字符的消息，可能由于buffer大小的问题导致一个字符会被分成两段，无法正常读取消息内容

解决思路：

- 一种思路是固定消息长度，数据包大小一样，服务器按预定长度读取，缺点是浪费带宽
- 另一种思路是按分隔符拆分，缺点是效率低
- TLV格式，即Type类型、Length长度、Value数据，类型和长度已知的情况下，就可以方便获取消息大小，分配合适的buffer，缺点是buffer需要提前分配，如果内容过大，则影响server吞吐量

演示第二种思路

```java
public class TestSelector {
    // 根据 \n 分隔字符，存入buffer
    private static void split(ByteBuffer source) {
        source.flip();
        for (int i = 0; i < source.limit(); i++) {
            if (source.get(i) == '\n') {
                int length = i + 1 - source.position();
                ByteBuffer target = ByteBuffer.allocate(length);
                // 从source读，向target写
                for (int j = 0; j < length; j++) {
                    target.put(source.get());
                }
                //
                System.out.println(StandardCharsets.UTF_8.decode(target));
            }
        }
        source.compact();
    }
    public static void main(String[] args) {
        try {
            // 1. 创建selector来管理多个channel
            Selector selector = Selector.open();

//            ByteBuffer buffer = ByteBuffer.allocate(16);
            ServerSocketChannel ssc = ServerSocketChannel.open();
            ssc.configureBlocking(false);

            // 2. 建立selector和channel的联系（注册）
            // sscKey在触发事件后，通过这个值可以知道事件类型和哪个channel
            SelectionKey sscKey = ssc.register(selector, 0, null);
            // 设置只关注accept事件
            sscKey.interestOps(SelectionKey.OP_ACCEPT);

            ssc.bind(new InetSocketAddress(8080));
            while (true) {
                // 3. select方法，没有事件发生，线程阻塞，有事件线程才会运行
                selector.select();
                // 4. 处理事件，selectedKeys返回所有被触发的事件key（set集合），因为要删除操作所以使用迭代器遍历
                // selector只会向selectedKeys中添加不会主动删除，需要自己删除
                Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
                while (iterator.hasNext()) {
                    // 获取key
                    SelectionKey key = iterator.next();
                    // 主动删除这个key，防止下次遍历到这个key，执行错误的分支
                    iterator.remove();
                    // 区分事件类型
                    // 实例中有简化，事实上只有accept事件
                    if (key.isAcceptable()) {
                        // 通过key获取对应channel，处理该事件
                        ServerSocketChannel channel = (ServerSocketChannel) key.channel();

                        SocketChannel sc = channel.accept();
                        sc.configureBlocking(false);
                        // 注册为关注read事件
                        // 把 buffer 作为附件关联到 read selector上
                        ByteBuffer buffer = ByteBuffer.allocate(16);
                        SelectionKey scKey = sc.register(selector, 0, buffer);
                        scKey.interestOps(SelectionKey.OP_READ);    // 这里执行完，回到selector.select(); 等待可读事件或者处理其它事件
                        System.out.println("成功建立连接，并监听read事件");
                    } else if (key.isReadable()) {  // 处理上面注册的read事件
                        try {
                            SocketChannel channel = (SocketChannel) key.channel();
                            // 获取附件
                            ByteBuffer buffer = (ByteBuffer) key.attachment();
                            System.out.println("触发read事件，等待数据...");
                            // 读取channel
                            int read = channel.read(buffer);
                            // 客户端没有写入数据直接断开时（客户端socket调用close），取消监听可读事件
                            if (read == -1) {
                                key.cancel();
                            } else {
                                // 业务代码
                                split(buffer);
                                // 如果客户端传来的是没有分隔符且长度大于buffer容量时，去buffer进行扩容
                                // 且用newBuffer替换旧buffer
                                if (buffer.position() == buffer.limit()) {
                                    ByteBuffer newBuffer = ByteBuffer.allocate(buffer.capacity() * 2);
                                    buffer.flip();
                                    // 将旧buffer中内容写入新buffer
                                    newBuffer.put(buffer);
                                    // 替换旧的附件
                                    key.attach(newBuffer);
                                }
                            }
                        } catch (IOException e) {
                            e.printStackTrace();
                            // 防止出现异常该事件无法完成，进而导致异常循环触发出现
                            /* 因为例如：客户端网络断开 会导致服务端tcp收到一个read事件，selector不会阻塞进入下一个循环，并在selectedKeys中添加一个key
                                由于selectedKeys对应的channel出现异常（因为客户端断开）selector会认为该channel仍然有事件要处理
                                这里解释不清，但是出现异常时，必须要cancel事件（停止监听）
                             */
                            key.cancel();
                        }
                    }
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

#### 多线程优化

### NIO vs BIO

#### 1. stream vs channel

- stream 不会自动缓冲数据，channel会利用系统提供的发送缓冲区，接收缓冲区（更为底层）
- stream 仅支持阻塞API，channel同时支持阻塞、非阻塞API，网络channel可配合selector实现多路复用
- 二者均为全双工，即读写可以同时进行

#### 2. IO模型

> 当调用一次channel.read或者stream.read后，会切换至操作系统内核态来完成真正数据读取，而读取又分为两个阶段，分别为：
> 
> - 等待数据阶段
> - 复制数据阶段

<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 125421.png">

同步阻塞、同步非阻塞、多路复用、异步非阻塞
阻塞IO
<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 130118.png">

非阻塞IO
<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 130203.png">

多路复用

java同步：线程自己去获取结果（一个线程）
java异步：线程自己不去获取结果，而是由其它线程送结果（至少两个线程）

同步阻塞：前面举的阻塞IO就是，同步的，且是阻塞的
同步非阻塞：前面非阻塞IO

异步非阻塞
<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 131717.png">
解释看黑马netty

### 零拷贝

传统IO问题
<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 132022.png">

NIO优化
通过DirectByteBuf

- ByteBuffer.allocate(10) 返回的是一个HeapByteBuffer 使用的是java内存
- ByteBuffer.allocateDirect(10) 返回的是一个DirectByteBuffer 使用的操作系统内存

<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 140543.png">

大部分于优化前相同，唯一有一点：java可以使用DirectByteBuffer将堆外内存映射到jvm内存中来直接访问使用

进一步优化，java中对应着两个channel调用transferTo/tranSferFrom方法拷贝数据
<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 140833.png">

1. java调用transferTo方法后，要从java程序的用户态切换至内核太，使用DMA将数据读入内核缓冲区，不会使用cpu
2. 数据从内核缓冲区传输到socket缓冲区，cpu会参与拷贝
3. 最后使用DMA将socket缓冲区的数据写入网卡，不会使用cpu

再进一步优化
<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 141335.png">

1. java调用transferTo方法后，要从java程序的用户态切换至内核太，使用DMA将数据读入内核缓冲区，不会使用cpu
2. 只会将一些offset和length信息拷入socket缓冲区，几乎无消耗
3. 使用DMA将内核缓冲区的数据写入网卡，不会使用cpu

整个过程仅只发生了一次用户态与内核态的切换，数据拷贝了2次，这就是所谓的零拷贝。

零拷贝的优点：

- 更少的用户态与内核态的切换
- 不利用cpu计算，减少cpu缓存伪共享
- 零拷贝适合小文件传输

### 异步

AIO用来解决数据复制阶段的阻塞问题

- 同步意味着，在进行读写操作时，线程需要等待结果，还是相当于闲置
- 异步意味着，在进行读写操作时，线程不必等待结果，而是将来由操作系统来通过回调方式由另一个线程来获取结果

<img src="./pic/java查缺补漏/屏幕截图 2026-02-09 144532.png">

Linux系统的异步IO底层实现还是用多路复用模拟了异步IO，性能没有优势

演示

```java
public class AioFileChannel {
    public static void main(String[] args) throws IOException {
        try {
            AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths.get("data.txt"), StandardOpenOption.READ);
            // 参数1 ByteBuffer
            // 参数2 读取的起始位置
            // 参数3 附件
            // 参数4 回调对象
            ByteBuffer buffer = ByteBuffer.allocate(16);
            channel.read(buffer, 0, buffer, new CompletionHandler<Integer, ByteBuffer>() {
                // read成功调用
                @Override
                public void completed(Integer result, ByteBuffer attachment) {
                    attachment.flip();  // 将附件转为读模式，附件就是buffer
                    // 打印
                    System.out.println("读取了"+result+"字符");
                    attachment.clear();

                }
                // read失败调用
                @Override
                public void failed(Throwable exc, ByteBuffer attachment) {

                }
            });
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        System.in.read();   // 这里阻塞一下主线程，防止子线程未完成主线程就结束了
    }
}
```

## Netty

1. 概述
   
   > Netty是一个异步的、基于事件驱动的网络应用框架，用于快速开发可维护、高性能的网络服务器和客户端

2. netty优势
   
   > dd

### Netty入门案例

用Netty写的一个服务端

```java
package com.ddw.nettylearn.c5;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;
import io.netty.handler.codec.string.StringDecoder;

public class HelloServer {
    public static void main(String[] args) {
        // 1. 服务端启动器，负责组装netty组件，启动服务器
        new ServerBootstrap()
                // 2.
                .group(new NioEventLoopGroup())
                // 3. 选择服务器的server socket channel实现
                .channel(NioServerSocketChannel.class)
                // 4. 决定了worker能做哪些事情
                .childHandler(
                        // 5. channel代表和客户端进行数据读写的通道Initializer初始化，负责添加别的handler
                        new ChannelInitializer<NioSocketChannel>() {
                    @Override
                    protected void initChannel(NioSocketChannel ch) throws Exception {
                        // 6. 添加具体handler，这个handler功能是将传递过来的ByteBuf转换为字符串
                        ch.pipeline().addLast(new StringDecoder());
                        // 添加自定义handler
                        ch.pipeline().addLast(new ChannelInboundHandlerAdapter(){
                            // 读事件触发的方法
                            @Override
                            public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
                                // 打印上一步转换的字符串
                                System.out.println(msg);
                            }
                        });
                    }
                }).bind(8080);
    }
}

```

用netty写的一个客户端

```java
package com.ddw.nettylearn.c5;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioSocketChannel;
import io.netty.handler.codec.string.StringEncoder;

import java.net.InetSocketAddress;

public class HelloClient {
    public static void main(String[] args) throws InterruptedException {
        // 1. 客户端启动类
        new Bootstrap()
                // 2. 添加EventLoop
                .group(new NioEventLoopGroup())
                // 3. 选择客户端channel实现
                .channel(NioSocketChannel.class)
                // 4. 添加处理器
                .handler(new ChannelInitializer<NioSocketChannel>(){
                    // 在连接建立后被调用，调用初始化
                    @Override
                    protected void initChannel(NioSocketChannel ch) throws Exception {
                        // 将字符串转成ByteBuf
                        ch.pipeline().addLast(new StringEncoder());
                    }
                })
                // 5. 连接到服务器
                .connect(new InetSocketAddress("localhost", 8080))
                // 阻塞方法，直到连接建立
                .sync()
                // 代表连接对象
                .channel()
                // 6. 向服务器发生数据
                .writeAndFlush("hello, netty!");
    }
}


```

handler分为两类：

- Inblund：为入站

- Outbound：为出站
  
  

### Netty组件

#### Netty组件 --- EventLoop

1. 事件循环对象 -- EventLoop

> EventLoop本质是一个单线程执行器（同时维护了一个Selector），里面有run方法处理Channel上源源不断的io事件
> 
> 它的集成关系比较复杂：
> 
> - 一条线继承自ScheduledExecutorService因此包含了线程池中所有的方法
> 
> - 另一条线是继承自netty自己的OrderedEventExecutor
>   
>   - 提供了boolean inEventLoop(Thread thread)方法判断一个线程是否属于此EventLoop
>   
>   - 提供了parent方法来看看自己属于哪个EventLoopGroup

2. 事件循环组 -- EventLoopGroup

> EventLoopGroup是一组EventLoop，Channel一般会调用EventLoopGroup的register方法来绑定其中一个EventLoop，后续这个Channel的io事件都由此EventLoop来处理（保证了io事件处理时的线程安全）
> 
> - 继承自netty自己的EventExecutorGroup
>   
>   - 实现了Iterable接口提供遍历EventLoop的能力
>   
>   - 另有next方法获取集合中下一个EventLoop

```java
public class EventLoopServer {
    public static void main(String[] args) {
        new ServerBootstrap()
                .group(new NioEventLoopGroup())
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<NioSocketChannel>() {
                    @Override
                    protected void initChannel(NioSocketChannel ch) throws Exception {
                        ch.pipeline().addLast(new ChannelInboundHandlerAdapter(){
                            @Override
                            public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
                                ByteBuf buf = (ByteBuf) msg;
                                System.out.println(buf.toString(Charset.defaultCharset()));
                            }
                        });
                    }
                }).bind(8080);
    }
}


// 客户端
public class EventLoopClient {
    public static void main(String[] args) throws InterruptedException {
        // 1. 客户端启动类
        Channel channel = new Bootstrap()
                // 2. 添加EventLoop
                .group(new NioEventLoopGroup())
                // 3. 选择客户端channel实现
                .channel(NioSocketChannel.class)
                // 4. 添加处理器
                .handler(new ChannelInitializer<NioSocketChannel>(){
                    // 在连接建立后被调用
                    @Override
                    protected void initChannel(NioSocketChannel ch) throws Exception {
                        ch.pipeline().addLast(new StringEncoder());

                    }
                })
                // 5. 连接到服务器
                .connect(new InetSocketAddress("localhost", 8080))
                .sync()
                .channel();
        System.out.println(channel);
        System.out.println("--");
    }
}
```

3. EventLoopGroup -- 工作细分

> 这里工作细分主要是：
> 
> 1. 创建多个group来分别处理ServerSocketChannel accept事件和socketChannel上的读写
> 
> 2. 创建一个独立的EventLoopGroup，来处理耗时长的逻辑，防止耗时长的逻辑阻碍了其它channel的操作

```java
public class EventLoopServer {
    public static void main(String[] args) {
        // 细分2：创建一个独立的EventLoopGroup，来处理耗时长的逻辑，防止耗时长的逻辑阻碍了其它channel的操作
        // 本质上是创建一个线程池，EventLoopGroup是继承中是有线程池的
        DefaultEventLoop group = new DefaultEventLoop();
        new ServerBootstrap()
                // 细分1：group可以创建多个group来进行工作细化
                // 第一个只负责ServerSocketChannel accept事件，第二个只负责socketChannel上的读写
                .group(new NioEventLoopGroup(), new NioEventLoopGroup())
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<NioSocketChannel>() {
                    @Override
                    protected void initChannel(NioSocketChannel ch) throws Exception {
                        // 添加一个处理方法，并命名为handler1，这个处理方法默认是交给.group中声明的第二个NioEventLoopGroup来运行
                        ch.pipeline().addLast("handler1", new ChannelInboundHandlerAdapter(){
                            @Override
                            public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
                                ByteBuf buf = (ByteBuf) msg;
                                System.out.println(buf.toString(Charset.defaultCharset()));
                                ctx.fireChannelRead(msg);   // 将消息传递给下一个处理方法
                            }
                            // 再添加一个处理方法，并命名为handler2，用新的EventLoopGroup group来处理，不使用上面NioEventLoopGroup
                        }).addLast(group, "handler2", new ChannelInboundHandlerAdapter(){
                            @Override
                            public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
                                // 耗时长的逻辑
                            }
                        });
                    }
                }).bind(8080);
    }
}
```

4. handler执行中如何换人

> handler是由EventLoop执行的，handler可以有多个，两个相邻的不同的handler可能是不同的EventLoop执行的，这两个handler在交接任务时是怎么切换EventLoop（线程池）的

<img title="" src="./pic/java查缺补漏/屏幕截图 2026-02-23 183414.png" alt="">

#### Netty组件 -- Channel

channel的主要作用

- close()可以用来关闭channel

- closeFuture()用来处理channel的关闭
  
  - sync方法作用是同步等待channel关闭
  
  - 而addListener方法是异步等待channel关闭

- pipeline()方法添加处理器

- write()方法将数据写入

- writeAndFlush()方法将数据写入并刷出
  
  

##### 1. channelFuture连接问题

> 在连接时，必须使用sync()，因为connect方法是异步非阻塞的

```java
public class EventLoopClient {
    public static void main(String[] args) throws InterruptedException {
        // 1. 客户端启动类
        ChannelFuture channelFuture = new Bootstrap()
                // 2. 添加EventLoop
                .group(new NioEventLoopGroup())
                // 3. 选择客户端channel实现
                .channel(NioSocketChannel.class)
                // 4. 添加处理器
                .handler(new ChannelInitializer<NioSocketChannel>() {
                    // 在连接建立后被调用
                    @Override
                    protected void initChannel(NioSocketChannel ch) throws Exception {
                        ch.pipeline().addLast(new StringEncoder());
                    }
                })
                // 5. 连接到服务器
                // 异步非阻塞的，main发起了调用，真正执行connect是nio线程
                .connect(new InetSocketAddress("localhost", 8080));
        // 阻塞直至连接成功
        channelFuture.sync();
        // 获取channel
        Channel channel = channelFuture.channel();

        System.out.println(channel);
        System.out.println("--");
    }
}
```

##### 2. channelFuture -- 处理结果

> channelFuture的连接是异步非阻塞的，可以使用sync()方法阻塞线程等待连接成功
> 
> 也可以使用addListener(回调对象) 方法异步处理结果

```java
channelFuture.addListener(new ChannelFutureListener() {
    // 在nio线程连接建立后，会调用operationComplete
    @Override
    public void operationComplete(ChannelFuture future) throws Exception {
        Channel channel = future.channel();
        channel.writeAndFlush("OK");
    }
});
```

##### 3. channelFuture -- 处理关闭

> channel的关闭也是异步的，有时需要在channel关闭后需要做一些收尾工作，为防止收尾工作在channel关闭之前执行，所以需要如下操作

```java
// 同步处理关闭
// 获取ClosedFuture对象
ChannelFuture closeFuture = channel.closeFuture();
// 等待关闭时执行操作
System.out.println("waiting close...");
channel.close();
closeFuture.sync(); // 会阻塞直到，channel.close()执行完
// 关闭后的操作
System.out.println("channel closed!");

// 异步处理关闭
// 获取ClosedFuture对象
ChannelFuture closeFuture = channel.closeFuture();
closeFuture.addListener(new ChannelFutureListener() {
    @Override
    public void operationComplete(ChannelFuture channelFuture) throws Exception {
        System.out.println("处理关闭之后的操作");
    }
});
```

关闭NioEventLoopGroup

```java
public class EventLoopClient {
    public static void main(String[] args) throws InterruptedException {
        // 把group抽出来声明，方便关闭
        NioEventLoopGroup group = new NioEventLoopGroup();
        // 1. 客户端启动类
        ChannelFuture channelFuture = new Bootstrap()
                // 2. 添加EventLoop
                .group(group)
                // 3. 选择客户端channel实现
                .channel(NioSocketChannel.class)
                // 4. 添加处理器
                .handler(new ChannelInitializer<NioSocketChannel>() {
                    // 在连接建立后被调用
                    @Override
                    protected void initChannel(NioSocketChannel ch) throws Exception {
                        ch.pipeline().addLast(new StringEncoder());

                    }
                })
                // 5. 连接到服务器
                // 异步非阻塞的，main发起了调用，真正执行connect是nio线程
                .connect(new InetSocketAddress("localhost", 8080));
        // 阻塞直至连接成功
        channelFuture.sync();
//        channelFuture.addListener(new ChannelFutureListener() {
//            // 在nio线程连接建立后，会调用operationComplete
//            @Override
//            public void operationComplete(ChannelFuture future) throws Exception {
//                Channel channel = future.channel();
//                channel.writeAndFlush("OK");
//            }
//        });
        // 获取channel
        Channel channel = channelFuture.channel();

        //
        ChannelFuture closeFuture = channel.closeFuture();
        closeFuture.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture channelFuture) throws Exception {
                System.out.println("处理关闭之后的操作");
                // 优雅停止group
                group.shutdownGracefully();
            }
        });
        // 等待关闭时执行操作
//        System.out.println("waiting close...");
//        channel.close();
//        closeFuture.sync(); // 会阻塞直到，channel.close()执行完
//        // 关闭后的操作
//        System.out.println("channel closed!");

        System.out.println(channel);
        System.out.println("--");
    }
}
```

#### Netty的 --- Future 和 Promise

> 首先netty中的Future与jdk中的Future同名，netty的future继承自jdk的future，而promise又对netty future进行了扩展
> 
> - jdk future只能同步等待任务结束（或成功、或失败）才能得到结果
> 
> - netty future可以同步等待任务结束得到结果，也可以异步方式得到结果，但都是要等任务结束
> 
> - netty promise不仅有netty future的功能，而且脱离了任务对立存在，只作为两个线程间传递结果的容器

<img title="" src="./pic/java查缺补漏/屏幕截图 2026-02-24 120920.png" alt="">

netty的future使用

```java
public class TestNettyFuture {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        NioEventLoopGroup group = new NioEventLoopGroup();
        EventLoop eventLoop = group.next();
        Future<Integer> future = eventLoop.submit(() -> {
            System.out.println("执行计算");
            Thread.sleep(5000);
            return 70;
        });
//        System.out.println("结果是"+future.get());
        // 异步执行，当future接收到结果后执行operationComplete
        future.addListener(new GenericFutureListener<Future<? super Integer>>() {
            @Override
            public void operationComplete(Future<? super Integer> future) throws Exception {
                System.out.println("结果是"+future.getNow());
            }
        });
    }
}
```

netty的promise使用，就像一个可以自己设置的结果的future

```java
public class TestNettyPromise {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 准备EventLoop对象
        EventLoop eventLoop = new NioEventLoopGroup().next();
        // 主动创建promise结果容器
        DefaultPromise<Integer> promise = new DefaultPromise<>(eventLoop);

        new Thread(()->{
            // 任意一个线程执行计算，计算完毕后向promise填充结果
            try {
                Thread.sleep(5000);
                // 执行正常时传递结果
                promise.setSuccess(80);
            } catch (InterruptedException e) {
                e.printStackTrace();
                // 发生错误时，传递错误结果
                promise.setFailure(e);
            }
        }).start();
        // 接收结果
        System.out.println("结果是"+promise.get());
    }
}
```

#### Netty的 --- Handler & Pipeline

> ChannelHandler用来处理Channel上的各种事件，分为入站、出战两种。所有ChannelHandler被连成一串，就是Pipeline
> 
> - 入站处理器通常是ChannelInboundHandlerAdapter的子类，主要用来读取客户端数据，写回结果
> 
> - 出战处理器通常是ChannelOutboundHandlerAdapter的子类，主要对写回结果进行加工
