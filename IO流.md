# IO 流



![image-20211203182750366](C:\Users\故事与酒\AppData\Roaming\Typora\typora-user-images\image-20211203182750366.png)

## 文件

1. 什么是文件呢?

* 文件就是存储数据的地方 他可以保存很多的东西例如图片,视频等等但是图片和视频是用二进制的格式保存的呢

2. 什么又是文件流呢?

* 文件流就是一种与文件相关联的东西
* 这样理解吧 文件就是你的包裹 而 文件流就是那个快递员 假设一下你的包裹是专员派送 那么那个包裹就是你的文件了 那个专员派送小哥就是你的文件流



## 文件的常规操作 (java)

1. 注意在java中文件在不在本地他不管 对于java而言文件就只是一种对象 文件是否真实存在只有在操作文件的时候才会发挥作用

```java
package File_Over;

import java.io.File;
import java.io.IOException;

@SuppressWarnings({"alll"})
public class CreatFile {
    public static void main(String[] args) throws IOException {

        String fileName = "e:\\JavaCode\\FileTest\\overFile.txt";
        String fileDir = "e:\\JavaCode\\FileTest";
        String fileName2 = "\\overFile2.txt";
        String fileName3 = "\\overFile3.txt";

        File file = new File(fileName); //第一种方法用绝对的路径来创建文件

        File dir = new File(fileDir); // 目录也是一种特殊的文件
        File file1 = new File(dir, fileName2); // 利用父级文件 加上自己的文件就可以来创建文件了

        File file2 = new File(fileDir,fileName3);

        if (!file.exists()){
            file.createNewFile();
        }else{
            System.out.println(  file.getCanonicalFile()+"文件存在了");
        }

        if (!file1.exists()){
            file1.createNewFile();
        }else{
            System.out.println(file1.getCanonicalFile()+"文件存在了");
        }

        if (!file2.exists()){
            file2.createNewFile();
        }else{
            System.out.println(  file2.getCanonicalFile()+ " 文件已经存在了");
        }

    }
}

```



* 上面就是创建文件的三种常用方法





##  获取文件相关信息的方法

```java
package File_Over;

import java.io.File;
import java.io.IOException;

public class File_Method {
    public static void main(String[] args) throws IOException {
        String filePath = "e:\\JavaCode\\FileTest\\overfile.txt";

        File file = new File(filePath);

        System.out.println(file.exists());
        System.out.println(file.isDirectory());
        System.out.println(file.getCanonicalFile());
        System.out.println(file.getAbsoluteFile());
        System.out.println(file.canRead());
        System.out.println(file.canWrite());
        System.out.println(file.delete());
        System.out.println("文件被删除了");


        String dirPath = "e:\\JavaCode\\FileTest\\temp";
        File dir = new File(dirPath);
        if (! dir.exists()){
            dir.mkdirs();
            System.out.println("创建了一个目录");
        }
        System.out.println(dir.getAbsoluteFile());
    }

}

```





## 流

1. 流就相当于一个运输的工具把他能运输的东西运输到你指定的地方
2. IO流就分为输入流 和输出流 

![image-20211203154325830](C:\Users\故事与酒\AppData\Roaming\Typora\typora-user-images\image-20211203154325830.png)



* 大概就是这个样子的 下面详细来介绍



## 字节流

3. 用于读字节的输入输出流  用字节来读取数据是不适合文本尤其是中文 

* InputStream  and  OutputStream

```java
package File_Over;

import java.io.*;

public class ByteStream_over {
    public static void main(String[] args) throws Exception {
        String filePath = "e:\\JavaCode\\FileTest\\byteStream.txt";
        InputStream fis = null;
        OutputStream fos = null;
        File file = new File(filePath);
        if (file.exists()){
            file.createNewFile();
        }
        //先写入一些数据在来读取数据
        fos = new FileOutputStream(file);
        fos.write("hi,Java".getBytes());

        fis = new FileInputStream(file);

        int line ;
        byte[]  strs = new byte[8];
        while ( (line = fis.read(strs)) != -1 ){
            System.out.println(new String (strs,0,line));
        }

    }
}

```



## 字符流

* Writer and Reader

```java
package File_Over;

import java.io.*;

public class CharStream {
    public static void main(String[] args) throws IOException {
        String filePath = "e:\\JavaCode\\FileTest\\byteStream.txt";
        File file = new File(filePath);
        file.createNewFile();
        Reader reader = new FileReader(file);
        Writer writer = new FileWriter(file);

        writer.write("你好,java!!!");
        writer.close();

        int read;
        while((read = reader.read()) != -1){
            System.out.print((char) read);
        }


    }
}

```





## 文本文件的拷贝 和 图片的拷贝

```java
package File_Over;

import java.io.*;

public class FileCopy {
    public static void main(String[] args) throws Exception {
        //用字符来拷贝效率高
        String filePath = "E:\\JavaCode\\分享资料\\Utility.java";
        Reader reader = new FileReader(filePath);
        String destPath = "e:\\JavaCode\\FileTest\\haha.java";
        Writer writer = new FileWriter(destPath);

        int line;
        char[] strs = new char[1024];
        while( (line = reader.read(strs)) != -1 ){
            writer.write(strs);
        }
        writer.close(); //对于字符流 一定要关流否则就不会将文件写入磁盘
        System.out.println("拷贝完成!!");


    }
}

```



```java
package File_Over;

import java.io.*;

public class pctureCopy {
    public static void main(String[] args) throws Exception {

        String srcPath = "E:\\JavaCode\\分享资料\\wb.png";
        InputStream isf = new FileInputStream(srcPath);
        String destPath = "e:\\JavaCode\\FileTest\\ha.png";
        OutputStream osf = new FileOutputStream(destPath);
        byte[] bs = new byte[1024];
        int len;

        while ((len = isf.read(bs)) != -1){
            osf.write(bs,0,len);
        }
        System.out.println("成功了");

    }
}

```

## 节点流 和 包装流(处理流)

* 节点流就是底层流 只能对一种的数据对象进行处理 (文件 数组 管道)
* 包装流 就是将节点流包装了一下 进行了缓存的设置 使他的效率更高  方法也更多
* 包装流有分为 字符包装流 和字节包装流 

```java
public BufferedInputStream(InputStream in) {
        this(in, DEFAULT_BUFFER_SIZE);
    }

```

* 这是一个包装输入流的构造器 可以发现他的参数是一个 InputStream 的对象 那就意味着 这个包装流可以接收一个InputStream 的子类 其他的同理
* 注意的是在调用close方法的时候就只需要关闭包装流即可 在底层 in 这个流对象的

```java
public void close() throws IOException {
        byte[] buffer;
        while ( (buffer = buf) != null) {
            if (U.compareAndSetReference(this, BUF_OFFSET, buffer, null)) {
                InputStream input = in;
                in = null;
                if (input != null) 
                    input.close(); //这里就是将 in 这个引用指向的索引给关闭了  所以就不能再去关闭底层流对象了
                return;
            }
            // Else retry in case a new buf was CASed in fill()
        }
    }
```



## 序列化

1. 什么是序列化呢

* 序列化就是将数据和数据类型保存到文件里

2. 什么是反序列化呢

* 反序列化就是从文件中将序列化的数据 读取到程序里

3. 怎么实现序列化呢?

* 一个类要实现序列化就要实现一个接口 Serializable 或者是他的一个子接口 Externalizable   但还是建议用Serializable 接口 因为他是纯标记接口 

4. 既然要讲对象存入到文件中 那么操作这种数据类型的也就是对象流了晒

## 对象流

ObjectInputStream and ObjectOutputSream 

```java
package File_Over;

import java.io.*;

public class ObjectStream_over {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        //序列化
        String srcPath = "e:\\JavaCode\\FileTest\\cat.txt";

        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(srcPath));

        oos.writeObject(new Cat(10,"tom","bule"));


        //反序列化
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(srcPath));
        Cat cat = (Cat)ois.readObject();
        System.out.println(cat);
    }
}

class Cat implements Serializable {
    int age;
    String name;
    String color;

    public Cat(int age, String name, String color) {
        this.age = age;
        this.name = name;
        this.color = color;
    }

    @Override
    public String toString() {
        return "Cat{" +
                "age=" + age +
                ", name='" + name + '\'' +
                ", color='" + color + '\'' +
                '}';
    }
}

```

## 标准输入 和 输出 流

* System.in 的类型是 InputStream  默认是键盘

```java
public static final InputStream in = null;
```

* System.out 的类型是 PrintStream  默认是显示器

```java
public static final PrintStream out = null;
```

```java
package File_Over;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.io.PrintStream;
import java.util.stream.Stream;

public class Stdio {
    public static void main(String[] args) throws IOException {
        //对于输入的设备我现在只有键盘就无法演示改变默认的输入设备键盘
        //所以下面就是输出设备的改变
        //确定输出的路径
        String destPath = "e:\\JavaCode\\FileTest\\stdio.txt";
        System.setOut(new PrintStream(destPath)); //这里重定向了System.out 的路径
        System.out.print("hi,java!! 哈哈");
        //这里说明一下 java 把输入和输出的设备看成了一个文件


    }
}

```

***这里说明一下 java 把输入和输出的设备看成了一个文件***



## 转换流

InputStreamReader and  OutputStreamWriter

* 转换流的作用是将字节流转换成字符流 可以提高效率外 还能指定编码

```java
package File_Over;

import java.io.*;

public class TrStream {
    public static void main(String[] args) throws IOException {
        //演示转换流 输入为例
        //设置路径
        String srcPath = "e:\\JavaCode\\FileTest\\dogv3.txt";
        InputStream inputStream = new FileInputStream(srcPath);

        InputStreamReader inputStreamReader = new InputStreamReader(inputStream,"gbk");  //将字节流转换为字符流
        String line;
        int i = 1;
        BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
        while ( (line = bufferedReader.readLine()) != null){
            System.out.println(i +" " +line);
            i++;
        }

    }
}

```



## 打印流

* 打印流就只有 输出流 没有输入流 想想就知道 打印嘛 肯定是输出呀

* 打印流分为PrintStream  and  PrintWriter

```java
package File_Over;

import java.io.PrintStream;

public class PrintStream_over {
    public static void main(String[] args) {
        //打印流和其他输出流并无什么大的差异
        //唯一我认为的区别就是他可以将显示器作为他的输出文件
        PrintStream printStream = new PrintStream(System.out);
        printStream.print("科比,大哥好");
        //看看print的源码就是一个writer的方法

    }
}

```

* 下面是print的源码

```java
 public void print(String s) {
        write(String.valueOf(s));
    }
//public class PrintStream extends FilterOutputStream  这个类下的方法
```



## Properties

* 这是专门来管理配置文件的(键值对 默认都是字符串)
* 使用的基本步骤是 先加载 在 根据需要执行方法

1. Properties 的常用方法

* load
* list
* getProperty
* setPrkoperty
* store // 将设置的键值对保存到配置文件里 还可以保存到其他的文件里

```java
package File_Over;

import java.io.*;
import java.util.Properties;
import java.util.stream.Stream;

public class Properties_over {
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();
        String srcPath = "src\\File_Over\\dog.properties";
        properties.load(new FileReader(srcPath));
        String destPath = "e:\\JavaCode\\FileTest\\dogv3.txt";
        PrintStream out = new PrintStream(destPath);
        properties.list(out);
          String name = properties.getProperty("name");
        System.out.println(name);


        properties.setProperty("master","iotu");
        properties.setProperty("xixi","haha");
//        properties.store(new FileWriter(srcPath),null);
        properties.store(new FileWriter(destPath,true),null);
    }
}

```



* Properties 的类其实是一个Hashtable的子类
* ![image-20211203185728093](C:\Users\故事与酒\AppData\Roaming\Typora\typora-user-images\image-20211203185728093.png)

​      

* 所以在执行load方法的时候其实是将配置文件里的键值对放到了Hashtable 维护的键值对里 (可以追追源码debug一下)
  * 而他的set方法就是将你添加的值放到了他维护的键值对(Map.Entry<Object, Object>)中所以在修改了值后要调用store方法

* 这里就是集合的知识了可以复习一下集合再来

# 总结一下

IO流重要的要理清里面的概念

* java 把流和文件当成两种对象
* 文件是就是你的包裹
* 流就是把包裹送给你的快递员
