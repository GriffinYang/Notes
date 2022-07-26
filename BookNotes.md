# Java核心卷一(第十版)

## 字符，代码单元和码点

在Java中ava字符串由char值序列组成，而char数据类型是一个采用UTF-16编码表示Unicode码点的代码单元。大多数的常用Unicode字符使用一个代码单元就可以表示，而**辅助字符(supplementary code point)**需要**一对代码单元**表示。而我们的charAt(int index)方法中index所对应的是所查询的字符串中的指定代码单元的索引而非绝对的该字符串的**码点(一个实际的字符称之为一个码点)**的索引。如：

```java
String str = "𝕆 is a set of octonions";
```

该字符串中的"𝕆"便是一个辅助字段，这也就意味着其占用的代码单元是两个，所以当我们对于该字符串调用cahrAt()方法时若我们的索引是1，则此时指向的并非是空格，而是"𝕆"的第二个代码单元，但是由于它是由两个代码单元所组成的我们仅仅凭借一个代码单元是无法对它进行表达的，所以我们将得到一个?也就是未知字符，所以此时如果我们需要获得空格则需要所以2，得到i则需要所以3了。

当我们对于字符串调用length()方法时，我便会获得该字符串的字节单元的长度，而如果我们想要获得实际长度(实际字符的个数亦或者时码点数量)则需要使用codePointCount()方法，其中参数分别为起始和结束索引：

```java
int cpCount = str.codePointCount(0, str.length()); //获得实际的str的字符个数
```

当我们需要获得一个字符串中的指定的码点的实际索引则可以调用offsetByCodePoints()方法，其中参数分别为起始索引和所需要的索引的偏移量：

```java
int index = str.offsetByCodePoints(0, 3); //此时我们将获得自0开始的第三个码点的索引
```

初次之外我们还可以调用isSupplementaryCodePoint()方法来判断一个码点是否是辅助字符，其中参数为码点的索引,如果是则返回true，否则返回false。该方法的参数是一个int值，即判断该int(十六进制)是否属于辅助字符的范围之内，除此之外我们还可以使用isSorrogate()方法来判断一个码点是否是一个补充字符，如果是则返回true，否则返回false。该方法的参数是一个字符值，若该字符属于辅助字符则返回true。

我们也可以使用字符串调用codePoints().toArray()来将其间的码点的字符的十进制表达储存在一个数组之中。

## Console

Console对象是IO包中的一个对象，其创建对象需借由System对象调用console()方法以返回一个Console对象。使用该对象我们可以调用readLine和readPassword两个方法，前者与Scanner和BufferedReader类都是获取控制台中的用户输入，但是后面的readPassword则提供密码保护功能，当用户输入是会自动隐藏其密码内容，这类似与Linux中的密码输入。除此之外，它们两个方法的参数即为提示内容，我们这也就意味着我们不需要额外添加一个输出语句作为提示语句了:

```java
        String userName = cons.readLine("Enter your name: ");
        char[] password = cons.readPassword("Enter your password: ");//此处注意获取的密码为字符数组
```

结果：

```java
Enter your name: Rongxin Yang
Enter your password: 
```

## Scanner和PrintWriter

Scanner对象可以从控制台和文件中读取数据，而后可以借由System.out.println()等方法输出，如：

```java
            Scanner scanner = new Scanner(new FileInputStream("D:\\iBooksDataBase\\Private investigators\\The Adventure of Sherlock Holmes.txt"),"UTF-8");
            while (scanner.hasNextLine()){
                System.out.println(scanner.nextLine());
            }
```

也就是说Scanner对象主要是用于读取数据的，而还有一个PrintWriter则主要是用于输出数据的:

```java
            /*向文件输出内容*/
            PrintWriter writer = new PrintWriter("D:\\iBooksDataBase\\Private investigators\\The Adventure of Sherlock Holmes.txt","UTF-8");
            writer.println("The Adventure of Sherlock Holmes");/*此时向D:\\iBooksDataBase\\Private investigators\\The Adventure of Sherlock Holmes.txt写入The Adventure of Sherlock Holmes*/
            writer.flush();/*使用PrintWriter必须刷新才能生效*/
            writer.close();

            /*向控制台输出内容*/
            PrintWriter writer = new PrintWriter(System.out);
            writer.println("Hello world!");
            writer.flush();
```

此时我们可以实现一个文件的复制功能：

```java
 try {
            Scanner scanner = new Scanner(new FileInputStream("D:\\iBooksDataBase\\Private investigators\\The Adventure of Sherlock Holmes.txt"),"UTF-8");
             PrintWriter writer = new PrintWriter("D:\\iBooksDataBase\\Private investigators\\The Text.txt","UTF-8");
            while (scanner.hasNextLine()){
                writer.println(scanner.nextLine());
            }
            writer.flush();
            writer.close();
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException(e);
        }
```

**注:**这里文件复制不包括非文本的文件如.exe二进制文件，对于这些文件我们还是需要使用DataOut(/In)putStream

一般情况下数组的声明是由两种：type[]indentifie=new type[length] 或者 type[]indentifie={ele1,ele2...eleN}; 然而事实上也存在匿名数组:type[]indentifie=new type[]{ele1,ele2...eleN},使用匿名数组我们可以事项对于原数组的重写:

```java
         int [] array ={1, 2, 3, 4, 5};
         array=new int[]{6, 7, 8, 9, 10};
```

## 命令行参数

在每一个Main主方法内我们都放置了一个String[] args参数，该参数是一个数组，其中每一个元素都是一个命令行参数，我们可以使用args[i]来获取其中的每一个参数，如：

```java
class MainTest {
       public static void main(String[] TXT) {
        for (int i = 0; i < TXT.length; i++) {
            System.out.println(TXT[i]);
        }
    }
}
```

但是通常情况下它是一个长度为零的数组，当我们使用命令行使用java 启动该main方法是才可以奏效，如：

```java
 java MainTest Hello World ！
```

则此时会输出：

```java
Hello 
World
!
```

因为此时我们的参数是在命令行中的所以这个参数便称之为命令行参数

## 初始化代码块

在Java类创建对象时，类的初始化代码块会被执行，这个代码块可以用来初始化对象的属性，如：

```java
class Employee{
    private static int nextId;
    private String name;
    private double salary,
    private int id;

    //该代码块在该类创建对象时被执行，无论该对象是使用哪一个构造其创建的对象
    {
        id=nextId;
        nextId++;
    }

    public Employee(String name, double salary){
        this.name=name;
        this.salary=salary;
    }
    ...
}
```
