# Java IO知识总结

## 简介

IO即为`input/output`，输入和输出。数据输入到计算机内存的过程即输入，反之输出到外部存储（比如数据库，文件，远程主机）的过程即输入。数据传输过程类似于水流，因此称为IO流。IO流在Java中分为输入流和输出流，而根据数据的处理方式又分为字节流和字符流。



Java IO流的40多个类都是从如下4个抽象类基类中派生出来的。

- `InputStream/Reader`：所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- `OutputStream/Writer`：所有输出流的基类，前者是字节输出流，后者是字符输出流。

## 字节流

### InputStream（字节输入流）

`InputStream`用于从源头（通常是文件）读取数据（字节信息）到内存中，`java.io.InputStream`抽象类是所有字节输入流的父类。

`InputStream`常用方法：

- `read()`：返回输入流中下一个字节的数据。返回的值介于 0 到 255 之间。如果未读取任何字节，则代码返回 `-1` ，表示文件结束。
- `read(byte b[])`：从输入流中读取一些字节存储到数组`b`中。如果数组`b`的长度为零，则不读取。如果没有可用字节读取，返回`-1`。如果有可用字节读取，则最多读取的字节数最多等于`b.length`,返回读取的字节数。这个方法等价于`read(b,0,b.length)`。
- `read(byte b[], int off, int len)`：在`read(byte b[])`方法的基础上增加了`off`参数（偏移量）和`len`参数（要读取的最大字节数）。
- `skip(long n)`：忽略输入流中的n个字节，返回实际忽略的字节数。
- `available()`：返回输入流中可读取的字节数。
- `close()`：关闭输入流释放相关的系统资源。

从Java 9开始，`InputStream`新增加了多个实用的方法：

- `readAllBytes()`: 读取输入流中的所有字节，返回字节数组。
- <u>`readNBytes(byte[] b, int off, int len)`：阻塞直到读取`len`个字节。</u>
- `transferTo(OutputStream out)`：将所有字节从一个输入流传递到一个输出流。

`FileInputStream` 是一个比较常用的字节输入流对象，可直接指定文件路径，可以直接读取单字节数据，也可以读取至字节数组中。

`FileInputStream` 代码示例：

```java
try (InputStream is = Files.newInputStream(Paths.get("input.txt"))){
    int read;
    while ((read = is.read()) != -1) {
        System.out.println(read);
    }
} catch (IOException e) {
    throw new RuntimeException(e);
}
```

常用字节输入流对象

- `FileInputStream`
- `BufferedOutputStream`

### OutputStream

`OutputStream`用于将数据（字节信息）写入到目的地（通常是文件），`java.io.OutputStream`抽象类是所有字节输出流的父类。

`OutputStream` 常用方法：

- `write(int b)`：将特定字节写入输出流。
- `write(byte b[ ])` : 将数组`b` 写入到输出流，等价于 `write(b, 0, b.length)` 。
- `write(byte[] b, int off, int len)` : 在`write(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
- `flush()`：刷新此输出流并强制写出所有缓冲的输出字节。
- `close()`：关闭输出流释放相关的系统资源。

`FileOutputStream` 是最常用的字节输出流对象，可直接指定文件路径，可以直接输出单字节数据，也可以输出指定的字节数组。

```java
  try {
      BufferedOutputStream bos = new BufferedOutputStream(Files.newOutputStream(Paths.get("output.txt")));

      byte[] bytes = "你好 io".getBytes(StandardCharsets.US_ASCII);

      System.out.println(Arrays.toString(bytes));

      String s = new String(bytes);
      System.out.println(s);

      bos.write(bytes);
      bos.flush();
      bos.close();
  } catch (IOException e) {
      throw new RuntimeException(e);
  }
```

常用字节输出流对象

- `FileInputStream`
- `BufferedOutputStream`

## 字符流

不管是文件读写还是网络发送接收，信息的最小存储单元都是字节。 **那为什么 I/O 流操作要分为字节流操作和字符流操作呢？**

乱码问题这个很容易就可以复现，我们只需要将上面提到的 `FileInputStream` 代码示例中的 `input.txt` 文件内容改为中文即可，原代码不需要改动。

可以很明显地看到读取出来的内容已经变成了乱码。

因此，I/O 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。



> ❗ 如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。



字符流默认采用的是 `Unicode` 编码，我们可以通过构造方法自定义编码。

常用字符编码所占字节数？

- `utf8` :英文占 1 字节，中文占 3 字节，
- `unicode`：任何字符都占 2 个字节，
- `gbk`：英文占 1 字节，中文占 2 字节。



### Reader

`Reader`用于从源头（通常是文件）读取数据（字符信息）到内存中，`java.io.Reader`抽象类是所有字符输入流的父类。

`Reader` 用于读取文本， `InputStream` 用于读取原始字节。

`Reader` 常用方法：

- `read()` : 从输入流读取一个字符。
- `read(char[] cbuf)` : 从输入流中读取一些字符，并将它们存储到字符数组 `cbuf`中，等价于 `read(cbuf, 0, cbuf.length)` 。
- `read(char[] cbuf, int off, int len)`：在`read(char[] cbuf)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字符数）。
- `skip(long n)`：忽略输入流中的 n 个字符 ,返回实际忽略的字符数。
- `close()` : 关闭输入流并释放相关的系统资源。

`InputStreamReader` 是字节流转换为字符流的桥梁，其子类 `FileReader` 是基于该基础上的封装，可以直接操作字符文件。

```java
// 字节流转换为字符流的桥梁
public class InputStreamReader extends Reader {
}
// 用于读取字符文件
public class FileReader extends InputStreamReader {
}
```

