# JAVA

## 2025.10.1

### Object 类的常见方法运用

### 1.Class类：

**`Class` 类用于表示类和接口。每个对象在运行时都有一个对应的`Class`对象，用于提供关于该对象所属类的信息。**

### 2.获取类/接口名称：

获取类名 ：*getClass()（用于将变量类型赋值给Class数据类型）*原数据类型调用

​															  **getSimpleName()（获取类的简称）** Class类调用

​															  **getSuperClass()（获取父类的名称）**

​									

获取接口名：*getInterface()（用于将变量类型赋值给Class数据类型）*原数据类型调用

​														     **getSimpleName()（获取接口的简称）** Class类调用（注意这里可能有多个接口，需要用Class数组存放）

### 3.HashCode：

**本身并无任何意义，原来是存放内存地址的，复写后按照复写的来返回**，**写代码时要遵循规则：若两个对象相等，则他们的HashCode必须相等**，**所以要重写HashCode方法而不能只重写equals方法**

### 4.equals：

底层代码用的是==，所以比较的是引用数据类型的内存地址，并不是很好用（无法做到比较内容的层面）,所以经常会复写equals方法，用来做内容的比较而非地址

​	下面的是一种常见的比较

```java
public boolean equals(Object obj)
{
    if(this.getClass() != obj.getClass())//比较类的类型
        return false;
    else if(this.hashCode() == obj.hashCode())//比较HashCode
        return true;
    else return this.name==((stu)obj).name&&this.age==((stu)obj).age;//obj要强转才能调用子类方法
 }
```


### 5.finalize：

当一个在堆开辟内存的数据变成垃圾时被调用，原函数中没有函数体，可以重写该方法用于释放堆中开辟的内存

​	这是一个示例

```java
@Override
    protected void  finalize()
{
    this.age=0;
    this.name=null;
    System.out.println("清除");
}
```

### 异常

### 1.异常的定义与分类：

异常是在程序执行期间发生的事件，该事件中断了程序指令的正常流程。异常由方法抛出，错误一般由虚拟机出问题造成，程序员无法解决，程序员需要解决的是由代码引起的异常，尽量避免检查异常（语法）

![image-20251001175007850](C:\Users\32524\AppData\Roaming\Typora\typora-user-images\image-20251001175007850.png)

### 2.异常的抛出												

```java
v1.0     ArithmeticException e = new ArithmeticException()		
		throw e
v2.0     throw new ArithmeticException("异常的内容");
		e.printStackTrace();//打印堆栈轨迹,异常出现的轨迹，从下往上为调用顺序
```

### 3.异常的声明

```java
public static int devided() throws InputMismatchException, ArithmeticException，.....//其它
    //：相当于免责声明，出现这些异常需要去找调用者，让调用者自己解决
```

### 4.异常的解决

​	1.try-catch 方法：

```java
try
{
    //尝试运行代码
}
catch(异常类型名1 异常对象名1)
{
    //解决办法
}
catch(异常类型名2 异常对象名2)
{
    //解决办法
}catch(异常类型名3 异常对象名3)
{
    //解决办法
}......
//值得注意的是，catch中父类异常不可放在子类异常之前,否则就是大的里面装小的，没什么意义
```

###    2.finally关键字：

​    值得注意的是，finally关键字的代码是一定会执行的(除非try或者catch中有System.exit(0)等退出程序的语句)，要与try-catch语句连用

```java
try
{
    //尝试运行代码
}
catch(异常类型名 异常对象名)
{
    //解决办法
}
finally()
{   
    //一定会运行的代码，经常用于清理资源   
}


//经典例子
//比较：
1. public static int getResult(){
        int number = 10;
        try 
        { 
            return number;
        } 
    	catch (Exception e)
        {
            return 1;
        } finally 
        {
            number++;
        }
//输出结果：10
//在try返回number时，识别到下方有一个finally语句，所以先把number用一个临时变量存储起来并且返回，然后再去执行finally中的语句
2. public static int getResult()
    {
        int number = 10;
        try 
        { 
            return number;
        } catch (Exception e) 
        {
            return 1;
        } finally 
        {
           return number+1;
        }
	}
//输出结果：11
//finally 块的优先级：在 Java 中，如果 try 块和 finally 块都有 return 语句，finally 块的 return 会覆盖 try 块的 return

```

### 5.自定义异常

​	1.为什么要自定义异常：方便检查代码哪错了

​	2.如何自定义一个异常

```java
//创建一个类，让他去继承某种异常，这样他就变成了一个异常

package Object;

public class UserNotFoundException extends Exception
{
    UserNotFoundException()
    {
		//空参构造
    }
    UserNotFoundException(String message)
    {
        super(message);//异常信息
    }
}
//这就是一个判断是否登录成功的异常
```

​	3.异常的注意事项

​	a. 运行时异常可以不处理。 

​	b. 如果父类抛出了多个异常,子类覆盖父类方法时,只能抛出相同的异常或者是该异常更小的级别

​	c. 父类方法没有抛出异常，子类覆盖父类该方法时也不可抛出检查异常。（但是可以抛出运行时异常）此时子类产生该异常，只能捕获处理，不能声明抛出

# 10.2

## String类

### 1.String类的构造方法

```java
		//1
        String s1="这是一个";
        s1+="字符串";
        System.out.println(s1);
        //2
        String s3 =new String("这是一个");
        System.out.println(s3);
        //3.byte
        byte[] data ={97,98,99,100};//存的是ASCII码
        String s4=new String(data);
        String s5=new String(data,1,3);//offset:偏移量 count:拷贝数量
        System.out.println(s4);
        System.out.println(s5);
        //4.Char
        char[] data2={'a','b','c','d','e','f','g','h'};
        String s6=new String(data2);
        String s7=new String(data2,1,3);//offset:偏移量 length:长度
        System.out.println(s6);
        System.out.println(s7);
        //5.Charset
        Charset charset=Charset.forName("GBK");//字符集
        String s8=new String(data,charset);//(byte,charset)
        System.out.println(s8);
```

### 2.String类的常用方法

```java
String s9=new String("0123445");
//1.获取长度 .length()
System.out.println(s9.length());
//2.获取字符 .charAt()
System.out.println(s9.charAt(0));
String s10=new String("ABC");
String s11=new String("abc");
//3.忽略大小写比较 .equalsIgnoreCase()
System.out.println(s11.equalsIgnoreCase(s10));
//4.转换成小/大写  .toLowerCase()/.toUpperCase()
System.out.println(s10.toLowerCase());
System.out.println(s11.toUpperCase());
//5.记录第一次出现的位置/最后一次出现的位置 .indexOf()/lastIndexOf()
System.out.println(s9.indexOf('4'));
System.out.println(s9.lastIndexOf('4'));
//6.截取字符串 subString()
String s12=s9.substring(0,1);//[0,1)
System.out.println(s12);
String s13=s9.substring(0);//从某个值开始截取后面所有的东西
System.out.println(s13);
//7.替换字符串 .replace(要替换的，换成啥)
String s14="aabbccddee1122334455";
System.out.println( s14.replace('b','B'));//替换所有的oldChar
System.out.println( s14.replace("bb","BB"));//字符串整个替换

//8.正则表达式
//[1-9][0-9]{2,2}表示一个三位整数
//[1-9][0-9]{2,4}表示一个三到五位整数
//[a-zA-Z]{1}或[a-zA-Z]+都表示不限长度的字符串
//正则表达式要用双引号括起来
//只有一个字符，不用写中括号
System.out.println(s14.replaceAll("[a-z]",""));

//9.不得不提的intern()
String s15="a";
String s16="b";
String s17=s15+s16;//s17在堆上
String s18="ab";
String s19=s17.intern();
System.out.println(s19==s18);
//对于基本数据类型，比较的就是内容，对于引用数据类型，比较的是两个变量的内存地址
//intern()的作用,把当前字符串放进常量池，先检测该常量池中有没有
//与该字符串内容相同的字符串,如果有,则返回该字符串的地址,如果没有,则
//返回新创建位置的地址

//10.转换成字符数组toCharArray()
String s20="abcd";
char[]c1 =s20.toCharArray();
for (int i = 0; i < c1.length; i++)
{
    System.out.println(c1[i]);
}
//11.转换成字节数组getbytes()
byte[] b1=s20.getBytes();
for (int i = 0; i < b1.length; i++)
{
    System.out.println(b1[i]);
}
//12.删除空白字符
String s21="   a b    ";
System.out.println(s21.trim());
//13.添加字符串
System.out.println(s20.concat(s21));//效率低下，根本用不上
//14.字符串转换对应数字
String c="1234";
int d=Integer.praseInt(c);
//c就等于1234;
//其它类型也能这样
//15.字符串按照某个符号分割存入字符串数组
String c="1,2,3,4";
String []arr=c.split(",");
for(String i:arr)
    System.out.printlin(i);
//输出1 2 3 4
```

## StringBuilder

### 1.构造方法

```java
StringBuilder b=new StringBuilder();//默认16容量
StringBuilder b=new StringBuilder(100);//自定义容量
StringBuilder b=new StringBuilder("Hello World");//字符串初始化
```

### 2.常用方法

```java
//0.构建与初始化
StringBuilder b=new StringBuilder();//默认16容量
StringBuilder b=new StringBuilder(100);//自定义容量
StringBuilder b=new StringBuilder("Hello World");//字符串初始化
//1.append(插入的东西)
b.append(a);//末尾插入一个字符(串)，或者数字,或者更多，有超级多重载
//2.insert(插入位置，需要插入的字符串)
b.insert(0,"b");
//3.delete(开始位置，结束位置) deleteCharAt(删除单个字符的位置)
b.delete(0,1);//[0,1)
b.deleteCharAt(0);
//4.replace(开始替换位置，结束替换位置，用于替换的字符串) 
//setCharAt(设置单个字符的位置，修改的字符)
b.replace(0,1,"c");//其实就是新字符串的起始和结束位置
b.setCharAt(0,'d');
//5.reverse()反转
b.reverse();
//6.获取长度、容量
System.out.println(b.length());//获取当前字符串长度
System.out.println(b.capacity());//获取当前字符串容量
//7.设置长度、容量
b.setLength(30);//不足用空字符代替
System.out.println(b.length());
b.trimToSize();
System.out.println(b.capacity());//将capacity调整成length的大小
//8.子串，字符获取
char temp=b.charAt(0);//获取单个字符
String temp1=b.substring(0,1);//[0,1)
String temp2=b.substring(0);//从某个值开始截取后面所有的东西
//9.查找方法
System.out.println(b.indexOf("H"));//查找第一次出现的位置
System.out.println(b.lastIndexOf("q"));//查找最后一次出现的位置
//补充数组复制
System.ayyaycopy(原数组，开始复制的位置，新数组，放入的位置)
//补充首尾判定
String a="1.txt";
return a.endsWith(".txt")//true
return a.startsWith("1")//true
```



## 内存各区

1.栈区：存放局部变量，跟C++类似

2.堆区：存放通过 `new` 关键字创建的对象和数组

3.方法区：

​				3.1 静态区：专门用于存储静态变量

​				3.2 类内容定义区：专门存储类的结构信息(类的代码之类的)

​				3.3 常量池：用于存放各种常量(final)

# 10.12

## I/O流以及文件操作

### 1.文件操作

```java
//构造文件方法，这并不是实际创建了一个文件，而是创建了一个文件对象
//绝对路径构建
File file0 = new File("C:\\Users\\32524\\Desktop\\Java\\Java\\javaSE\\day07\\test.txt");
//父级文件构建
File fileFather = new File ("C:\\Users\\32524\\Desktop\\Java\\Java\\javaSE\\day07\\");
File fileChild = new File(fileFather,"test.txt");
//文件常用方法
File file=new File("day07\\1.txt");
//1.获取相对路径(模块文件夹下)
System.out.println(file.getPath());
//2.绝对路径
System.out.println(file.getAbsolutePath());
//3.获取名字
System.out.println(file.getName());
//4.获取父文件
File parent=file.getParentFile();
//5.获取父文件路径
System.out.println(file.getParent());
//6.获取文件大小 单位是字节
System.out.println(file.length());//longLong
//7.获取文件最后修改时间 单位是毫秒
System.out.println(file.lastModified());
System.out.println((System.currentTimeMillis()-file.lastModified())/1000);//获取系统时间
//8.判断文件是否存在 通常会与创建目录的方法配合使用
System.out.println(file.exists());
//9.判断文件是否是文件夹
System.out.println(file.getParentFile().isDirectory());
//10.判断文件是否是隐藏文件
System.out.println(file.isHidden());
//11.判断文件是否可执行 点开有反应的都叫可执行文件
System.out.println(file.canExecute());
//12.！！创建新的文件 创建文件之前 必须保证上级目录已经存在 否则会报IO异常
File newFile = new File("day07\\test\\2.txt");
File newFileParent = newFile.getParentFile();
if(!newFileParent.exists())
{
    //创建多级目录
    System.out.println(newFileParent.mkdirs());//一般用这个
}
if(!newFile.exists())
{
    try
    {
        System.out.println(newFile.createNewFile());
    }
    catch (IOException e)//抛出IO异常 IOException
    {
        System.out.println("创建失败");
        e.printStackTrace();
    }
}
//13.删除文件
System.out.println(newFile.delete());
//14.删除文件夹 删除文件夹时 要保证文件夹内清空才可以删除
System.out.println(newFileParent.delete());
//15.重命名文件(移动+重命名) 移动文件时 要保证父目录存在 否则无法移动
//这个重命名也是挺抽象的 直接把文件移动过去再改名 为啥不直接给个字符串的形参......
File a=new File("day07\\a.txt");
try
{
    a.createNewFile();
}
catch (IOException e)
{
    e.printStackTrace();
}
File b =new File("day07\\b.txt");
System.out.println(a.renameTo(b));
//16.列出文件
File firstPackage = new File("day07\\1");
File[] files=firstPackage.listFiles();
//for each 循环 前面表示 i 后面表示要遍历的东西
for(File i:files)
{
    System.out.println(i.getName());
}
//过滤器(运用了匿名内部类)
File Father = new File("C:\\Program Files\\Tencent\\QQNT");
FileFilter rule =new FileFilter()(这个是要实现的接口或者类)
{
    @Override
    public boolean accept(File pathname) {
        return pathname.getName().endsWith(".dll");//endsWith已经补充
    }
};
File[] files2=Father.listFiles(rule);
for(File i:files2)
{
    System.out.println(i.getName());
}
```

### 2.递归的运用(有点新颖)

```java
//要求:删除一个目录下的所有文件
package com.saber.file;
import java.io.File;
public class recursive
{
    public static void main(String[] args)
    {
        File index = new File("C:\\Users\\32524\\Desktop\\Java\\Java\\a");
        recursiveDelete(index);
    }
    public static void recursiveDelete(File file)
    {
        if(file.isDirectory())//判断是否是文件夹 若是
        {
            File[] files = file.listFiles(); //列出所有文件(夹)
            if(files != null)//看看文件夹中是否为空 若不是
            {
                //执行删除所有文件的操作
                for (File i : files)//遍历这个文件夹的所有文件
                {
                    if (i.isDirectory())//如果文件还是文件夹
                    {
                        recursiveDelete(i);//再调用文件夹的处理方式 也就是自己
                        //如果是文件夹 那么主程序就会卡在这里不动 直到该文件夹下的所有文件都被删除
                    } 
                    else
                        i.delete();//如果是文件则直接删除
                }
            }
            //执行删除文件夹的操作
            //程序不会被卡住才会进行到这里，也就说明上面删完了
            file.delete();//上面一层层以及删除了该文件夹里面的所有东西，所以这个文件夹相当于是空的了heihei
        }
        //不是文件夹 直接删除
        else
            file.delete();
    }
}
```

### 3.输出流(字节流)

```java
//根据提供的文件路径构建一条文件输出通道，并根据append的值决定是将内容追加到末尾还是直接覆盖
OutPutStream os =new FileOutPutStream("文件路径",true);//默认为false覆盖 会抛出FileNotFoundException
//根据提供的文件信息构建一条文件输出通道，并根据append的值决定是将内容追加到末尾还是直接覆盖
OutPutStream os =new FileOutPutStream(文件，true);//默认为false覆盖 会抛出FileNotFoundException
//写入文件
os.write(字节数组,偏移量，长度);//会抛IOException 输出流关闭再写就会抛
//刷入数据
os.flush();//强制将通道中的数据刷入文件
//关闭通道 减少内存消耗
os.close();
```

### 4.输入流(字节流)

```java
//构造方法
//根据提供的文件路径构建一条文件输入通道 抛出FileNotFoundException
InputStream is = new FileInputStream ("文件路径");
//根据提供的文件信息构建一条文件输入通道 抛出FileNotFoundException
InputStream is = new FileInputStream (File文件);
//常见方法
//刷入数据
is.flush();//强制将通道中的数据刷入文件
//关闭通道 减少内存消耗
is.close();
//获取通道中的数据长度
is.available();
//读取数据 返回实际读取的数据长度 -1表示读取到尽头了
is.read(数组,偏移量,长度);
//读取方法1 当读取的文件比较小的时候
public static void main(String[] args) 
{
    try 
    {
        InputStream is = new FileInputStream("文件路径");
        int length = is.available();//获取通道中的数据长度
        //根据通道中数据的长度构建字节数组。文件小时可以这样构建 太大了就会爆内存的
        byte[] buffer = new byte[length];//buffer 缓冲区的意思
        int readCount = is.read(buffer); //将通道中的数据全部读取到buffer数组中
        System.out.println("读取了"+readCount+"个字节");
        System.out.println(new String(buffer));//匿名对象
        is.close();//关闭通道
    } 
    catch (FileNotFoundException e) 
    {
        e.printStackTrace();
    } 
    catch (IOException e) 
    {
        e.printStackTrace();
    }
}
//读取方法2 当读取的文件太大的时候
public static void main(String[] args)
{
    try 
    {
        InputStream is = new FileInputStream("文件路径");
        //实际开发过程中字节数组的长度一般定义为1024的整数倍
        byte[] buffer = new byte[31]; //构建了一个长度为31的字节数组
        while (true)
        {
            //从通道中读取数据存入字节数组buffer中，返回值就是读取的字节长度.
            //如果读取到数据的末尾，则返回-1
            int len = is.read(buffer);
            //实际上就是一个小数组复用的过程
            //比如我要读100个字节的文件
            //第一次 读入31个数据 也就是 buffer[0]~buffer[30]的数据被读入然后立马输出
            //第二次 读入31个数据 也就是 buffer[0]~buffer[30]的数据被读入并覆盖然后立马输出
            //第三次 读入31个数据 也就是 buffer[0]~buffer[30]的数据被读入并覆盖然后立马输出
            //第四次 读入7个数据 也就是 buffer[0]~buffer[6]的数据被读入并覆盖然后立马输出
            if(len == -1) break;//读完了
            System.out.println(len);
            System.out.println(new String(buffer));
        }
        is.close();
    } 
    catch (FileNotFoundException e) 
    {
        e.printStackTrace();
    } catch (IOException e) 
    {
        e.printStackTrace();
    }
}
//总结: 字节流用于基本数据类型操作的场景,比较低级一般不用
```

### 5.补充 文件的传递

```java
package com.saber.file;
import java.io.*;
public class InAndOut
{
    public static void main(String[] args)
    {
        File newfile=new File("C:\\Users\\32524\\Desktop\\Java\\Java\\javaSE\\day07\\test\\2.txt");
        File file1=new File("C:\\Users\\32524\\Desktop\\Java\\Java\\javaSE\\day07\\test\\1.txt")；
        //想在全局使用通道流时，可以定义在外面
        InputStream in=null;
        OutputStream up=null;
        //文件读入
        byte[] c=new byte[3];//字节数组小于读入的数据量，也就是用上面的方法2
        try
        {
            in= new FileInputStream(file1);
            up=new FileOutputStream(newfile);
            while(true)
            {
                int temp=in.read(c);
                if(temp==-1)
                    break;
                up.write(c,0,temp);
            }
        }
        catch (FileNotFoundException e)
        {
            throw new RuntimeException(e);
        }
        catch (IOException e)
        {
            throw new RuntimeException(e);
        }
        //1.使用finally释放
        finally
        {
            Close(in,up);
        }

    }
    public static void Close(Closeable...c)//不定长的自变量 本质上是一个数组 挺方便的
    {
        for(Closeable i:c)
        {
            if(i!=null)
            {
                try
                {
                    i.close();
                }
                catch (IOException e)
                {
                    throw new RuntimeException(e);
                }
            }

        }
    }

    //2.在JDK 1.7 之后 可以使用一种很神秘的try(){}catch{} 语句
    //try(里面只能放实现了AutoCloseAble的类){}catch{}
    //这种try可以自动释放通道 减少代码量
}

```

### 6.字符流

```java
//类似于字节流 但是字符流更高级 可以把字符转换成ASCII码  但是编码不同容易出错
//输出
//构造方法 (默认多态的形式 养成好习惯) 
Writer w = new FileWriter("文件路径"/文件对象,append:默认false); 抛出IOECxception
//方法(Writer常用方法)
w.write(int/char[]/String);//写一个字符/字符数组/字符串
w.write(char[],offset,len);
w.flash();//刷出数据
w.close();//关闭通道
//输入
//构造方法
Reader r = new FileReader("文件路径"/文件对象); 抛出 FileNotFoundException
//方法(Reader常用方法)
r.read(char[],offset,len);
//返回读的数据量 -1 表示读完
r.flash();//刷出数据
r.close();//关闭通道
//补充 字符 '1' 变成数字1只需减  '0'
```

### 7.缓冲流

缓冲流的作用：减少跟操作系统的交互，提升程序效率

```java
//缓冲字节流
//输出
//构造方法
BufferOutPutStream bos = new BufferOutPutStream (FileOutPutStream,int size);
//size指缓冲区大小 默认8192B 需要一个 FileOutPutStream 类构造
//常用方法
bos.write(byte[]);
bos.write(byte[],offset,len);
//刷出数据
bos.flash();
//关闭通道
bos.close();
//输入
//构造方法
BufferInPutStream bis = new BufferInPutStream (FileInPutStream,int size);
//size指缓冲区大小 默认8192B 需要一个 FileInPutStream 类构造
//常用方法
bis.read(byte[]);
bis.read(byte[],offset,len);
//返回读的数据量 -1 表示读完
//刷出数据
bis.flash();
//关闭通道
bis.close();
```

```java
//缓冲字符流
//直接上例子
package bufferStream;
import java.io.*;
public class bsChar
{
    public static void main(String[] args)
    {
        try (Writer a= new FileWriter("day07\\a.txt");
             BufferedWriter b= new BufferedWriter(a);//需要一个Writer初始化
             Reader c= new FileReader("day07\\a.txt");
             BufferedReader d= new BufferedReader(c))//需要一个Reader初始化
        {
            char[] e=new char[4096];
            b.write("111");
            b.newLine();//相当于换行
            b.write("111");
            b.newLine();
            b.write("111");
            b.newLine();
            b.flush();//别忘了要刷新哦 ！！！
            while(true)
            {
                String i=d.readLine();//读取一行 新方法 其它方法都一样的
                if(i==null) break;
                System.out.println(i);
            }

        }
        catch (IOException e)
        {
            throw new RuntimeException(e);
        }
    }
}

```

### 8.数据流

用于网络编程

```java
package DataInAndOut;
import java.io.*;
public class Data
{
    public static void main(String[] args)
    {
        try(OutputStream a =new FileOutputStream("day07//test//data.txt");
            DataOutputStream b =new DataOutputStream(a);
            InputStream c =new FileInputStream("day07//test//data.txt");
            DataInputStream d =new DataInputStream(c);)
        {
            b.writeByte(1);
            b.writeBoolean(true);
            b.writeInt(2);
            b.writeShort(3);
            b.writeFloat(4.0f);
            b.writeDouble(5.00);
            b.writeChar('a');
            b.writeUTF("字符串");
            b.flush();
            System.out.println(d.readByte());
            System.out.println(d.readBoolean());
            System.out.println(d.readInt());
            System.out.println(d.readShort());
            System.out.println(d.readFloat());
            System.out.println(d.readDouble());
            System.out.println(d.readChar());
            System.out.println(d.readUTF());
        }
        catch (FileNotFoundException e)
        {
            throw new RuntimeException(e);
        }
        catch (EOFException e)//文件意外到达末尾时抛出
        {
            System.out.println("End of file");
        }
        catch (IOException e)
        {
            throw new RuntimeException(e);
        }
    }
}
```

### 9.对象流(感觉很实用的样子)

```java
package ObjStream;
import java.io.*;
public class obj
{
    public static void main(String[] args)
    {
        try(OutputStream os = new FileOutputStream("day07\\test\\obj.obj");
            ObjectOutputStream oos = new ObjectOutputStream(os);
            InputStream is = new FileInputStream("day07\\test\\obj.obj");
            ObjectInputStream ois = new ObjectInputStream(is);)
        {
            Stu s = new Stu("saber",1);
            oos.writeObject(s);
            oos.flush();
            Stu s1 = (Stu)ois.readObject();//返回的是Object类型，需要强转
            System.out.println(s1);
        }
        catch (IOException e)
        {
            throw new RuntimeException(e);
        } catch (ClassNotFoundException e)
        {
            throw new RuntimeException(e);
        }
    }
}
//将一个对象从内存中写入磁盘文件中的过程称之为序列化。序列化必须要求该对象所有类型实现序列化的接口Serializable
//反之则叫反序列化
//Stu类
package ObjStream;
import java.io.Serializable;
public class Stu implements Serializable//实现可序列化接口才能序列化
{
    private String name;
    private int age;
    public Stu(String name, int age) {
        this.name = name;
        this.age = age;
    }
    @Override
    public String toString() {
        return "Stu [name=" + name + ", age=" + age + "]";//重写toString方法
    }
}
```

### 10.字节流转字符流

用处:当读取的文本不是用UTF-8编码时，直接用字符流存在乱码风险，所以要用字节流指定读入方式，再转为字符流的边界读入，相当于整合了字符流的便捷性和字节流的兼容性

```java
package com.saber.file;

import java.io.*;

public class change
{
    public static void main(String[] args)
    {
        try (OutputStream  os = new FileOutputStream("day07\\test\\change.txt");
             //使用中间载体 OutputStreamWriter 继承于Writer 有字节流的构造方法      
             OutputStreamWriter osw = new OutputStreamWriter(os,"GBK");//指定编码方法
             //这里再转为缓冲流
             BufferedWriter bw= new BufferedWriter(osw);
             //使用中间载体 InputStreamReader 继承于Reader 有字节流的构造方法
             InputStream is = new FileInputStream("day07\\test\\change.txt");
             InputStreamReader isr = new InputStreamReader(is,"GBK");//指定编码方法
             //这里再转为缓冲流
             BufferedReader br = new BufferedReader(isr);
            )
        {
            bw.write('中');
            bw.flush();
            System.out.println((char)br.read());
        }
        catch (FileNotFoundException e)
        {
            throw new RuntimeException(e);
        }
        catch (IOException e)
        {
            throw new RuntimeException(e);
        }

    }
}
```

### 11.总结

I/O流其实分成两大类 ：字节流和字符流

字节流的顶层父类是 OutPutStream 和 InPutStream 缓冲流 数据流 对象流都是他的子类

字符流的顶层父类是 Writer 和 Reader OutputStreamWriter InputStreamReader等都是他的子类

# 10.16

## 内部类以及lambda表达式

## 内部类

### 1.普通内部类：

```java
//类中类 模块化编程
package InnerClass;
import org.w3c.dom.ls.LSOutput;
public class car
{
    static int b;
    String a="1";
    void show()
    {
        System.out.println("外部类函数的调用");
    }
    class engine//车里面有引擎 引擎又有很多零件
    {
        String a="2";
        void show()
        {
            String a="3";
            System.out.println(a);//就近原则
            System.out.println(this.a);//输出这个类里面的a
            System.out.println(car.this.a);//输出外部类的a
            //存在同名情况时
            //创建内部类变量的时候，会自动生成一个（指针）指向外部类的地址，名字叫做car.this
            //当使用这个指针修饰的时候，会自动调用外部类的变量
        }
    }    
```

### 2.静态内部类

![image-20251016152351689](C:\Users\32524\AppData\Roaming\Typora\typora-user-images\image-20251016152351689.png)

```java
//静态内部类,不知道有啥用
static class Class
{
    static void show1()//静态方法
    {
        //可以直接调用静态变量
        car d=new car();
        System.out.println(d.a);//访问非静态变量需要定义一个对象(？)
        System.out.println("静态内部类调用静态成员函数");
    }
    void show2()//静态方法
    {
        System.out.println("静态内部类调用非静态成员函数");
    }
}
```

### 3.匿名内部类：

用于只实现一次的接口实现类

```java
package NoName;
public class test
{
    public static void main(String[] args)
    {
        //直接new是可行的,因为这是个对象,但是要赋值就不行了
        //比如说 Run run = new Run()
        {
            
        }.调用方法
        //是错误的,因为这个语句是创建对象,如果调用方法就不是返回新对象的地址了
        new Run()
        {
            @Override
            public void run()
            {
                System.out.println("Run的重写方法");
            }
        }.run();
        //因为是对象，甚至还能直接调用
        //类的定义和对象的定义集成
        //这就是基于接口的一个匿名类,其实就是接口的一个实现类的对象
        //定义类的同时创建了对象，大大优化代码量
        //具体解析
        //先补充一个点:接口默认抽象，无法被创建，但可以被实现类对象赋值(抽象类函数也是这样的)
        //甚至还能传值给函数，形成接口的多态
        //一般用于!一次性!使用的实现接口，就没必要分多个类写了
        new method()
        {
            @Override
            public void method()
            {
                System.out.println("实现了method");
            }
        }.method();
        //这就是基于继承的匿名内部类
    }
}
```

## lambda表达式

作用:用于省略匿名内部类的复杂写法

```java
//省略方式
method a=new method()
{
    @Override
    public void method()
    {
        System.out.println("实现了method");
    }
}
//这是一个匿名内部类的写法
//使用基础lambda表达式
method a =(参数类型1,参数名称1，参数类型2,参数名称2......)->
{
  重写类的方法  
};
a.调用方法
//lambda表达式的适用范围:只适用于只有一个方法的抽象类接口/父类 
//lambda表达式的省略
/*
1.()中的所有参数类型可以省略
2.如果()中有且仅有一个参数，那么()可以省略 
3.如果{}中有且仅有一条语句，那么{}可以省略，这条语句后的分号也可以省略。如果这条语句是return
语句，那么return关键字也可以省略 *
*/
//没啥特别的用处 省代码了
```

# 10.27

## 集合

### 1.集合的概述

集合（有时称为容器）只是一个将多个元素分组为一个单元的对象。集合用于存储，检索，操作和传达聚合数 据。其实这玩意相当于C++的stl，就是数据结构的实例化

### 2.集合的具体分类

![image-20251027182436005](C:\Users\32524\AppData\Roaming\Typora\typora-user-images\image-20251027182436005.png)

由图可知，集合分为Collection和Map两大类，其中AbstractCollection实现了Collection接口 是所有线性结构的父类 而Map就是顶层父类

### 3.Collection接口

```java
//常用方法
int size();  //获取集合的大小
boolean isEmpty();//判断集合是否存有元素
boolean contains(Object o);//判断集合中是否包含给定的元素
Iterator<E> iterator();//获取集合的迭代器
Object[] toArray();//将集合转换为数组
<T> T[] toArray(T[] a);//将集合转换为给定类型的数组并将该数组返回
boolean add(E e);//向集合中添加元素
boolean remove(Object o);//从集合中移除给定的元素
void clear();//清除集合中的元素
boolean containsAll(Collection<?> c);//判断集合中是否包含给定的集合中的所有元素
boolean addAll(Collection<? extends E> c);//将给定的集合的所有元素添加到集合中
```

### 4.Iterator迭代器

#### 迭代器的基本定义:

**迭代器** 是一个对象，它允许程序员**遍历一个容器（如列表、数组、集合等）中的所有元素**，而无需了解该容器的底层数据结构，这是设计模式的概念，其实等价于指针，是方便STL的算法支持各种容器，为了设计方便,而每个集合中的函数则是效率更高的算法

#### 迭代器在Java中的表现

**迭代器**在Java中是一个名为Iterator的接口，拥有三个约定

```java
//判断是否有下一个元素
boolean hasNext();
//返回当前元素 并且使迭代器向后移动一位
E next();
//删除迭代器当前所指元素
//Tips
/*default关键字用于修饰接口中的方法，使得接口能够包含具有实现的方法，而不仅仅是抽象方法*/
default void remove() 
{throw new UnsupportedOperationException("remove");}
```

#### 迭代器的具体实现

```java
class MyIterator implements Iterator
{
    //光标 指向当前位置 实际上就是下标
    int cursor =0;
    //判断是否有下一个元素
    @Override
    public boolean hasNext()
    {
        return cursor<size;
    }
    @Override
    //返回当前元素 并将迭代器指向下一个元素
    public Object next()
    {
        if(cursor>=size||cursor<0)
            throw new NoSuchElementException("下标越界");
        return element[cursor++];
        //后置运算符 先用再加
    }
    //移除当前元素
    @Override
    public void remove()
    {
        if(cursor>size||cursor<0)
            throw new IllegalStateException();
        System.arraycopy(element,cursor,element,cursor-1, size - cursor);
        size--;
        cursor--;
    }
}
```

### 5.自定义集合

```java
package ArrayList;
import java.util.*;
//让该类继承于Collection 那就相当于创造了一个集合
public class MyCollection extends AbstractCollection
{
    //下面的都跟顺序表非常类似
    Object []element;
    int size=0;
    int capacity=0;
    MyCollection(int capacity)
    {
        element = new Object[capacity];
        this.capacity=capacity;
    }
    MyCollection()
    {
        this(16);
    }
    public void enlarge()
    {
        // 1.5倍,ArrayList源码也是
        this.capacity+=this.capacity>>2;
        Object[] newData=new Object[this.capacity];
        System.arraycopy(element,0,newData,0,this.size);
        this.element=newData;
    }
    class MyIterator implements Iterator
    {
        //上面实现了
    }
    /*需要重写的方法*/
    @Override
    //原来的语句只有抛出异常
    public boolean add(Object o)
    {
        if(this.capacity==size)
            enlarge();
        element[size]=o;
        size++;
        return true;
    }
    @Override
    //必须重写返回该集合的迭代器
    public Iterator iterator()
    {
        return new MyIterator();
    }
    @Override
    //必须重写该集合的大小
    public int size()
    {
        return size;
    }
    /*系统默认已经实现的方法*/
    //是否为空
    public boolean isEmpty() 
    {
        return size() == 0;
    }
    //是否包含
    public boolean contains(Object o) 
    {
        Iterator<E> it = iterator();
        if (o==null) 
        {
            while (it.hasNext())
                if (it.next()==null)
                    return true;
        } 
        else 
        {
            while (it.hasNext())
                //只要有相等的就包含
                if (o.equals(it.next()))
                    return true;
        }
        return false;
    }
    //删除
    public boolean remove(Object o) 
    {
        Iterator<E> it = iterator();
        //默认尾删
        if (o==null)
        {
            while (it.hasNext()) 
            {
                if (it.next()==null) 
                {
                    it.remove();
                    return true;
                }
            }
        } 
        else 
        {
            while (it.hasNext()) 
            {
                if (o.equals(it.next())) 
                {
                    //相等就删除
                    //注意 这里判断相等时调用了next 迭代器的cursor向后移动了一位需要向前回移一位
                    it.remove();
                    return true;
                }
            }
        }
        return false;
    }
    //展示
     public String toString() 
     {
        Iterator<E> it = iterator();
         //空
        if (! it.hasNext())
            return "[]";
        StringBuilder sb = new StringBuilder();
        sb.append('[');
        for (;;) {
            E e = it.next();
            //考虑了自己添加自己的特殊情况 因为append进来会调用本函数的toString方法 会无限递归
            sb.append(e == this ? "(this Collection)" : e);
            if (! it.hasNext())
                //没有了就直接返回
                return sb.append(']').toString();
            //还有就加逗号空格
            sb.append(',').append(' ');
        }
    }
	//其它的不再赘述
}
```

### 6.泛型

泛型就是一个变量，只是该变量只能使用引用数据类型来赋值，这样能 够使同样的代码被不同数据类型的数据重用

```java
//包含泛型的类定义语法：
//通常使用的泛型变量： E T  K  V 
访问修饰符 class 类名<泛型变量> {}
//包含泛型的接口定义语法：
访问修饰符 interface 接口名<泛型变量>{}
//方法中使用新的泛型语法：
访问修饰符 <泛型变量> 返回值类型(泛型变量) 方法名(泛型变量 变量名,数据类型1, 变量名1,...,数据类型n, 变量名n{}
```

```java
//创建一个泛型的myCollection
myCollection <指定的数据类型> mycollection = new myCollection<>();                          
//在JDK7及以上版本，new 对象时如果类型带有泛型，可以不写具体的泛型。
//当创建MyCollection对象没有使用泛型时，默认是Object类型
```

泛型通配符(了解)

当使用泛型类或者接口时，如果泛型类型不能确定，可以通过通配符?表示

```java
<?>
//这个表示所有类型都可以 都当做Object类来处理 所以这个泛型修饰的东西只读不写
<? extend 数据类型>
//上界通配符 最顶层就是指定的数据类型 取出来一定可以当做指定数据类型来处理(即可以向上转型) 但不知道到底以哪种类型放进去 所以只读不写
<? super 数据类型>
//下界通配符 可以存储指定数据类型的父类或者本身 一定可以用指定数据类型放入 但取出来不知道是哪种类型 所以只写不读
```

### 7.list接口

```java
E get(int index); //获取给定位置的元素
E set(int index, E element);//修改给定位置的元素
void add(int index, E element);//在给定位置插入一个元素
E remove(int index);//移除给定位置的元素
int indexOf(Object o);//获取给定元素第一次出现的下标
int lastIndexOf(Object o);//获取给定元素最后一次出现的下标
ListIterator<E> listIterator();//获取List集合专有的迭代器
ListIterator<E> listIterator(int index);//获取List集合专有的迭代器 从给定的下标位置开始的迭代器
List<E> subList(int fromIndex, int toIndex);//获取List集合的一个子集合
```

### 8.ArrayList

ArrayList存储的是有序的集合，集合有序是指存储顺序与遍历时取出来的顺序一致，就是先进先出，就是顺序表！

继承实现关系 : ArrayList继承于AbstractList

​						   AbstractList 继承于AbstractCollection

​						   AbstractCollection实现Collection接口

常用方法:

```java
//1.list接口包括的所有方法
//2.Iterator自带的迭代器(单向访问)
Iterator<> iterator = list.iterator();
//3.ListIterator迭代器(双向访问)可以指定开始位置
ListIterator<Integer> listIterator = list1.listIterator(list1.size());
```

部分源码解析:

```java
//1.add()
public boolean add(E e) 
{
    //修改的次数++
    modCount++;
    add(e, elementData, size);
    return true;
}
private void add(E e, Object[] elementData, int s) 
{
    //如果元素个数等于顺序表的容量
    if (s == elementData.length)
        //那么就扩容
        elementData = grow();
    elementData[s] = e;
    size = s + 1;
}
private Object[] grow() 
{
    return grow(size + 1);
}
private Object[] grow(int minCapacity) 
{
    int oldCapacity = elementData.length;
    if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) 
    {
        int newCapacity = ArraysSupport.newLength(oldCapacity, minCapacity - oldCapacity, oldCapacity >> 1);//这里就是扩容到原来的1.5倍的意思，如果扩容够的话直接返回新数组的地址
        return elementData = Arrays.copyOf(elementData, newCapacity);
    }
    //如果原来的长度是0，要和默认容量进行比较,取最大值 不能减少空间到比默认容量少吧。。。
    else 
    {
        return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
    }
}

//2.remove的注意事项
//源码中有两个remove
//这个是移除某个元素
	public boolean remove(Object o)
//这个是移除某个位置,在对整数进行操作的时候,如果要删除对应元素,需要将其强转为Integer类型
	public E remove(int index)  

    //3.ensureCapacity()确保可用空间足够
    public void ensureCapacity(int minCapacity)
{
    if (minCapacity > elementData.length
        && !(elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
             && minCapacity <= DEFAULT_CAPACITY)) 
    {	
        modCount++;
        //自动扩容
        grow(minCapacity);
    }
}
```

**总结:******ArrayList底层采用的是数组来存储元素，根据数组的特性，ArrayList在随机访问时效率极高，在 增加和删除元素时效率偏低，因为在增加和删除元素时会涉及到数组中元素位置的移动。ArrayList在 扩容时每次扩容到原来的1.5倍****

### 9.LinkList(链表)

底层:双向链表

继承关系：LinkedList继承于AbstractSequentialList，

​					AbstractSequentialList继承于 AbstractList， 

​					AbstractList继承于AbstractColletion

常用方法:

```java
void addFirst(E e)//将数据存储在链表的头部
void addLast(E e)//将数据存储在链表的尾部
E removeFirst()//移除链表头部数据
E removeLast()//移除链表尾部数据
//其它通用方法不再叙述
```

**LinkedList底层采用的是双向链表来存储数据，根据链表的特性可知，LinkedList在增加和删除元 素时效率极高，只需要链之间进行衔接即可。在随机访问时效率较低，因为需要从链的一端遍历至链的 另一端**。

### 10.栈(Stack)

底层:顺序表

```java
//常用方法
//压栈
public E push(E item)
//弹栈
public synchronized E pop() 
//查看栈顶元素
public synchronized E peek()     
```

### 11.Queue接口

说明:Queue一般实现的是先进先出的队列,但也不是绝对的,也有可能实现不满足这些条件的队列

```java
boolean add(E e);//向队列中添加一个元素，如果出现异常，则直接抛出异常
boolean offer(E e);//向队列中添加一个元素，如果出现异常，则返回false*
E remove();//移除队列中第一个元素，如果队列中没有元素，则将抛出异常
E poll();//移除队列中第一个元素，如果队列中没有元素，则返回null*
E element();//获取队列中的第一个元素，但不会移除。如果队列为空，则将抛出异常
E peek();//获取队列中的第一个元素，但不会移除。如果队列为空，则返回null*
```

### 12.队列(LinkedBlockingQueue)

数据特征：先进先出

常用方法：跟Queue接口没区别

继承关系：LinkedBlockingQueue继承于AbstractQueue

​					AbstractQueue实现Queue接口 继承于AbstractCollection
### 13.优先队列(PriorityQueue)

数据特征：按照指定顺序出列(自然排序或者内排序)

常用方法：跟Queue接口没区别

```java
//构造方法
PriorityQueue <T(内部没有实现Comparable接口)> 名称 = new PriorityQueue<>(Comparator接口);
PriorityQueue <T(内部实现Comparable接口)> 名称 = new PriorityQueue<>();
```

继承关系：跟LinkedBlockingQueue一致

注意事项：当不存在比较关系时，出队列会抛出异常

### 14.Comparable接口

用途：当自定义类需要用到排序时,可自定义排序方式，当没有传入Comparator接口时，就按照默认方法排序

实现：类的内部，称为自然排序,也称为默认排序。类的内部重写Comparable的compareTo方法

操作步骤：

```java
@Override
public int compareTo(stu o)
{
    //年龄相同,调用String内部的compareTo方法 按字典序排序
    if(this.age==o.age) return this.name.compareTo(o.name);
    else if(this.age>o.age) return 1;
    else return -1;
}
```

### 15.Comparator接口

用途：特殊排序方式

实现：写在外部，匿名内部类的方式去写,最后使用函数的时候传参就行，也可以作为优先队列的出列顺序

```java
//lambda表达式 记得写泛型
Comparator<lesson> c =(o1,o2)->
{
    if(o1.getScore()==o2.getScore())
        return o1.getName().compareTo(o2.getName());
    else if(o1.getScore()>o2.getScore())
        return 1;
    else
        return -1;
};
Arrays.sort(a,c);
Collections.sort(a,c);//均可
```

### 16.Map接口

1.作用：实现的是一个映射关系，也就是Key->Value，有很多作用，比如快速查找等很多用法
(在数据结构中学的hash查找就是这个Map)

2.常见方法：

```java
int size(); //获取集合的大小
boolean isEmpty();//判断集合是否为空
boolean containsKey(Object key);//判断集合中是否包含给定的键
boolean containsValue(Object value);//判断集合中是否包含给定的值
V get(Object key);//获取集合中给定键对应的值 
V put(K key, V value);//将一个键值对放入集合中 相同Key第一次是插入，第二次是修改
V remove(Object key);//将给定的键从集合中移除
void putAll(Map<? extends K, ? extends V> m);//将给定的集合添加到集合中
void clear();//清除集合中所有元素
Set<K> keySet();//获取集合中键的集合(后面学)
Collection<V> values();//获取集合中值的集合(后面学)
Set<Map.Entry<K, V>> entrySet();//获取集合中键值对的集(后面学)
```

Map接口中有一个Entry类，其实就是Key和Value组合起来的一个对象，有下面的常见方法

```java
K getKey(); //获取键
V getValue();//获取值
V setValue(V value);//设置值
boolean equals(Object o);//比较是否是同一个对象
int hashCode();//获取哈希码
```

### 17.HashMap(哈希表)

1.基本概述：

##### 哈希表是一种用hashCode和Capacity按位与得到的index存储在顺序表中、相同index存储在链表或红黑树中的一种表结构

2.作用：

增删改查都快

3.常用方法：

```java
//Map和Entry的方法不再赘述,没什么特殊方法
```

### 18.TreeMap(树)

1.基本概述：

基于红黑树进行排序之后的集合

2.常用方法：

```Java
//构造方法
TreeMap<K,V> treeMap =new TreeMap<>(Comparator接口);//或者内排序，跟优先队列是一样的
```

### 19.LinkHashMap

1.基本概述：

继承于HashMap，维护双向链表，制定迭代顺序，按照插入顺序进行迭代

2.常用方法：跟Map接口一样(学完数据结构再补这块)

### 20.Set接口

1.基本概述：基于Map接口实现的集合(数学定义上的，不能包含重复元素)的一个接口

2.用处：自动去重

3.常用方法:

```java
int size();  //获取集合的大小
boolean isEmpty();//判断集合是否存有元素
boolean contains(Object o);//判断集合中是否包含给定的元素
Iterator<E> iterator();//获取集合的迭代器
Object[] toArray();//将集合转换为数组
<T> T[] toArray(T[] a);//将集合转换为给定类型的数组并将该数组返回
boolean add(E e);//向集合中添加元素
boolean remove(Object o);//从集合中移除给定的元素
void clear();//清除集合中的元素
boolean containsAll(Collection<?> c);//判断集合中是否包含给定的集合中的所有元素
boolean addAll(Collection<? extends E> c);//将给定的集合的所有元素添加到集合中
//跟Collection完全没有区别
```
4.HashSet：利用HashMap进行存储，不能保证顺序

5.TreeSet：跟TreeMap完全一样，保证自然排序或者Comparator接口的排序

6.LinkedHashSet：跟LinkedHashMap完全一样，保证插入顺序排序

**==还差网络编程 xml 多线程没有总结 eee 找时间总结 还有一些集合的常用方法 这几天搞==**