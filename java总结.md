# java 总结

## 面向对象基础

### 方法

* 方法分为很多种 有静态方法 普通方法 构造方法 大致而言方法是给外部的代码提供了一些可以调用内部代码的接口

  1. 先从普通方法说起 

  * 普通方法 可以看成一个对象的行为动作

  2. 构造方法是用来实例化对象 和 初始化一些字段和属性的
  3. 静态方法 是 隶属于类的  每一个相同类的实例都可以调用这个方法 而且就算没有实例也可以调用

* 这里注意: 在java中区分方法是利用方法签名的 何为方法签名呢 :  方法签名就是**方法名 + 形参列表** 方法签名并没有包括**返回值类型**

* 方法参数的传递 : 方法参数的传递 本质就是值的复制传递  但是呢 对于不同的类型有着不一样的结果  基本数据类型就是直接将数据的值复制 而 引用数据类型是将地址复制 也就是坐标复制,**数据并没有复制**

* 这里讲讲方法重载和方法的重写

      1. 方法的重载主要解决的问题的方法的逻辑大致一样只是形参的类型不一样 这时就可以考虑使用方法的重载
         2. 方法的重写是要有条件的 **那就是继承** 没有继承就谈不上重写 重写是子类根据不用来实现不同的逻辑
         3. 方法重载的条件比较轻松 就只要求方法签名一致(也就是**方法名** + **方法形参列表**)
         4. 方法重写的方法就有点麻烦了 不仅要求方法签名一致还要求方法返回值类型一致 而且抛出的异常不能多于父类 而且 作用域不能缩小

### 继承

继承的主要目的是提高代码的复用性 提高效率

换句话说就是 一些基本的方法和属性 可以生来就有 自己就只用专心的来搞自己独有的方法和扩展原来的方法(**更加具体可以参考面向对象编程的笔记**)





### 类的多态

因为继承 和 重写 的缘故 让多态成为了java的一大特色

* 在继承了父类的子类是  如果将更多功能的子类压缩成父类的类型 那么重写的方法会被覆盖 这个时候每个方法的具体实现逻辑取决于子类的实现逻辑 
* 就造成一个假象 父类的相同方法居然可以实现不同的逻辑 但是这里有一点要值得强调 编译器只看编译类型 并且 字段(属性)不存在重写(动态绑定机制) 所以用父类的类型引用子类时访问的属性是父类里面的



### 抽象类

抽象类 就是为了重写而生的

* 抽象类的抽象方法可以只有方法签名 不需要方法体,但是遗憾的是抽象类具有了这个牛逼的特性后就失去了实例化的能力
* 抽象类毕竟还是一个类 所以他还是可以有属性和普通方法的   
* 抽象类可以没有抽象的方法 但是有了抽象方法必须要是抽象类

### 接口

接口可以理解为更加抽象的抽象类 他直接可以没有属性(字段)

1. 他是接口所以他的方法天生就是抽象的就不需要关键词abstract
2. 但是实现了接口的方法就必须来实现他全部的方法 就很难受
3. 所以可以在接口里定义一个默认(default)的方法让一个需要用的类来重写这个方法



### 包

就是便于管理我们写的java源文件 使得可以有同名的类名可以调用

JVM 怎么寻找类呢

1.  首先是在当前的包看有没有该类
2. 然后是在import看有没有该类
3. 最后看java.lang下有没有该类
4. 都没没就只能报错了



### 作用域

这里单独在讲讲作用域

作用域的那几个关键词是以包作用域为基础扩大或缩小范围的

**public** ------- 就是以包为作用域扩大至全部的外部代码都能用这个类

**private**------ 以包为作用域缩小至只有本类可以访问

**protected**------ 以包为作用域扩大至 只要是我的子类都可以访问

**friendly** -------- 这个就是默认  就是包作用域 在同一个包下随便访问



### final

单独来讲讲final 

1. 被他修饰的类不能被继承
2. 被他修饰的属性不能被修改
3. 被他修饰的方法不能被重写



### jar格式

jar的全名是java Archive:存档

jar的本质其实是一个压缩包zip 只是在jar中多了一些配置信息 这些配置信息是让jar能在JVM上运行的关键 暂时还不是很懂



### 模板

模板是对jar格式的一种进一步优化 因为在实际的运用中并不需要完整的java类库 

但是jar会把全部都打包 显得有些臃肿  从而模板开始出来 将jar文件分成多个jmod文件 然后需要那些就是组合那些

jmod中描述了各个模块的依赖关系 所以既精简了题集 还可以限制对外开放的类提高了代码的封装性







## 核心类

* String

  第一个肯定是String晒 这个类极大的方便了对于字符串的需求 

  1. **在这个类中提供了许多的方法这里简单枚举一下详情请看常用类和包装类**

     * 第一要说的就是他的equals() 方法,由于在系统底层对于String的类型是当做常量对待 所以在代码层面我们的得到的其实是一个指向对应的地址 所以

       用== 来判断两个字符串相等显然是不合理的了 因为== 对于引用数据类型是判断地址是否相等即是不是同一个对象

     * contains(String) 看看括号里的字符串在当前的字符串中存不存在

     * indexOf(String)  返回子字符串首次出现的索引

     * lastIndexOf(String)  最后出现的索引

     * startsWith(String)  是否以子字符串开头

     * EndWith(String) 是否以子字符串为结尾

     * subString(int,int) 截取子字符串以索引为标志

     * trim() 去掉开头和结尾的字符串

     * strip() 和 trim相同但是类中文的空格会去掉

     * replace(String,String) 进行替换

     * replaceAll() 进行全部替换

     * split()用于分割

     * format()  格式化 是String的静态方法 第一个参数是对于已经格式好了的字符串进行格式化

     * formatted()不是静态方法 是作用于已经格式化好了的方法参数用于填好占位符

     * 利用toCharArray() 可以实现String 转换成char数组

     * valueOf()  可以将其他类型转换成字符串  **在包装类中也提供了字符串转换为对应包装类的类型**----静态方法

     *   getBytes()  将字符串转换为字节数组 在io流是很有用

* StringBuilder  and  StringBuffer

  这两个都是可变长度的字符串  唯一的不同就是StringBuffer是线程安全就是加了锁的

  对于String的好处在于不用在常量池中因为字符串的修改就重新的创建对象造成了对内存的浪费

  * 里面的几个常用的方法同样是简单枚举
    * append() 在末尾添加
    * insert() 指定位置插入
    * 其他方法....

* 包装类

  包装类其实就是对基本数据类型进行打包 变成引用数据类型

  * **包装类的很多方法同样看包装类和常用类**

* 枚举类

  就是通过关键词Enum来实现的 他的构造器是私有的 换句话说就是他不能在外面被实例化

  * 常用的方法简单枚举
    * name() 返回常量的名字(底层的原理不是很理解)
    * ToString() 默认是返回常量名
    * ordinal() 返回常量的顺序
    * values() 返回一个包含所有常量的数组

* 记录类 

  他是用来保存一组不变的数据

  ```java
  record Point(int x, int y){};
  
  ```

* BigInteger  and  BigDecimal 
  * BigInteger 是一个对于很大很大的整数一个专门的类 里面是使用int[] 来实现的
    * 具体的方法
    * add() 加
    * subtract 减
    * multiply 乘
    * divide 除
    * pow 次方
  * BinDecimal  是高精度 在乎的是精度
    * 常用的方法
    * scale() 返回小数的位数
    * stripTrailingZeros() 去掉末尾的零
    * n.divideAndReminder(m)  ----- n / m 的商和余数 
    * 注意在BigDecimal 进行比较的时候 使用CompareTo来比较  因为使用equals() 要进行位数的比较 位数不想等就不会相等

* 还有工具类
  * Math类就是一个数学工具人 很多静态的方法
  * Random 生成随机数的类
  * SecureRandom 生成安全随机数 没有种子

## 异常

首先解释一下 在java中分为异常 和 错误

错误是不能捕获和解决的,会直接让程序崩溃

但是异常则可以捕获

* 异常又分为编译异常 和 运行异常
* 编译异常你必须显式的处理
* 而运行异常可以不显式处理 因为他默认是**将异常抛出给调用方 交给调用方处理**
* 常见的异常 可以看异常**的总结**

**对于异常的捕获方法**

* 第一种就是甩手掌柜 直接将异常扔出去给调用方  (throws)
* 第二种就是自己处理 使用try catch finally  里面的catch 和 finally 不同时存在
* 当然也可以有多个catch

**捕获异常的原理**

* 在try代码块中 一旦发现一个异常 将异常抛出 然后就不会执行剩下的代码而是专心的处理异常
* 当多种异常的处理方案是一个的时候 为了优化代码 可以不用使用多个catch 可以在一个catch中使用 **|** 这个符号

**我们也可以自己来抛出异常**

在java这个万物皆可以对象世界里异常也是一个实例而已

可以使用关键词throw 将异常抛出

当然还可以自定义异常 我建议是继承运行异常 因为他有默认的处理异常的机制(throws)---- 交给上级处理



还有一种机制是**断言**

就好像我立flag  如果成功了 那么逼就装到了 但是一旦错了 就会无情的抛出异常 打脸了 但是我们可以知道异常发生在了哪里



然后就是**日志系统**了

以后学习在来详细说明!!!



## 反射

* 将反射的时候就不得不讲一个名字和关键字很象的一个类 Class 这个类和这个关键字class(没错就是定义类的关键字超级象 但是 这只是形式上象结果他还是个普通的类)
* 这不过 这个 类的实例化是由我们的老大哥JVM调用的   它里面就只是保存了其他类的结构
* 哈哈,这里举个不恰当的例子 那就是 Class的实例就像一个照妖镜一样把你的真实的结构都可以看出来
* 其实Class类实例就是保存了其他类的结构信息而已没有任何神秘的

* 而反射就是利用这里Class的这一特性在框架上的引用 , **即就是通过Class类的实例来反射出一个存储在他内部结构信息类的实例!!!**

```java
//给好奇宝宝看看Class类

public final class Class<T> implements java.io.Serializable,
                              GenericDeclaration,
                              Type,
                              AnnotatedElement,
                              TypeDescriptor.OfField<Class<?>>,
                              Constable {....}
```

* 在反射的机制里 万物都是对象 方法也是一个对象 字段(属性)也是一个对象,构造器也是对象,具体的方法可以参考**反射的笔记**

## 泛型

* 泛型就是广泛的类型 但是看起来是美好的 泛型是类型的变量 其实只不过是用了类型转换的

```java
public Test <T>{
    public Test( T t){
        System.out.println("我是构造器!!" + T);
    }
    
    public static void main (String[] args){
        Test<String> test = new Test<>("haha");
    }
}


//就相当于
public Test <Object>{
    public Test( Object t){
        System.out.println("我是构造器!!" + (String)t);
    }
    
    public static void main (String[] args){
        Test<String> test = new Test<>("haha");
    }
}
```

* **其实实际上这是编译器帮我们把T换成了Object 然后进行了安全的强制类型转换**

其他细节参考**泛型笔记**
