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

**注:** 这里文件复制不包括非文本的文件如.exe二进制文件，对于这些文件我们还是需要使用DataOut(/In)putStream

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

## HashCode

散列码（hash code）是由对象导出的一个整型值。散列码是没有规律的。如果x和y是两个不同的对象，x.hashCode( )与y.hashCode( )基本上不会相同。

**多重参数:** 当我们希望一个方法可以同时接受n个参数可以使用...来代替多余的所有参数,这里的省略号...是Java代码的一部分，它表明这个方法可以接收任意数量的对象:

```java
public void fn(Object... params)  //此时内部的参数可以是n个
```

**枚举:** 在枚举中我们除了设置其枚举值以外还可以设置每个枚举值的缩写，但是我们需要首先创建一个枚举对象(所有的枚举类型都是Enum类的子类。它们继承了这个类的许多方法。其中最有用的一个是toString，这个方法能够返回枚举常量名),创建对象我们首先需要调用Enum的静态方法以获取一个枚举值的实例,如：

```java
enum Size
{
   SMALL("S"), MEDIUM("M"), LARGE("L"), EXTRA_LARGE("XL");  //这里的SMALL，MEDIUM，LARGE，EXTRA_LARGE是枚举常量的名称。注意与常规的类不同，枚举的构造方法的实例是直接定义在枚举内部的

   Size(String abbreviation) { this.abbreviation = abbreviation; } //这里也就相当于枚举的一个'构造方法' 
   public String getAbbreviation() { return abbreviation; }  //获取该内部变量的方法

   private String abbreviation;  //相当于一个全局变量
}

```

**注:** 比较两个枚举类型的值时，永远不需要调用equals，而直接使用“= =”就可以了。

1.toString方法:使用 "枚举名.枚举值.toString()" 来获取枚举值的字符串表示->

```java
Size.SMALL.toString(); //返回SMALL
```

2.values方法:使用 "枚举名.values()" 来获取枚举值的数组->

```java
Size[] values = Size.values(); //返回包含元素Size.SMALL, Size.MEDIUM, Size.LARGE和Size.EXTRA_LARGE的数组。
```

3.toString的逆方法是静态方法valueOf:使用 "枚举名.valueOf(枚举值的字符串表示)" 来获取枚举值的实例->

```java
Size s=Enum.valueOf(Size.class,"SMALL");//返回一个enum的实例对象，我们使用s可以返回SMALL,同时我们也可以使用size.getAbbreviation()以获得S。这里我们并未在创建对象时调用构造方法，因为我们在枚举值中已经提前声明了，因此此时我们使用valueOF便会自动调用我们在枚举值定义的构造实例
```

4.ordinal:返回enum声明中枚举常量的位置，位置从0开始计数。例如：Size.MEDIUM. ordinal()返回1。

5.compareTo(E enum):如果枚举常量出现在other之前，则返回一个负值；如果this==other，则返回0；否则，返回正值。枚举常量的出现次序在enum声明中给出。

```java
      Size s1 = Enum.valueOf(Size.class, "SMALL");
      Size s2 = Enum.valueOf(Size.class, "SMALL");
      Size m = Enum.valueOf(Size.class, "MEDIUM");
      System.out.println(s1.compareTo(s2));
      System.out.println(m.compareTo(s1));
      System.out.println(s1.compareTo(m));

//结果
0
1
-1
```

## class类

在程序运行期间，Java运行时系统始终为所有的对象维护一个被称为运行时的类型标识。这个信息跟踪着每个对象所属的类。虚拟机利用运行时类型信息选择相应的方法执行。而它们的容器就是一个CLass类。我们可以通过一个实例对象调用getClass方法来获取这个类的实例。

```java
Employee e;
...
Class cl=e.getClass();
System.out.println(cl.getName()+" "+e.getName());
/*结果: Employee[cl.getName()] Rongxin Yang[e.getName()]*/
```

如果类在一个包里，包的名字也作为类名的一部分：

```java
Random generator=new Random();
Class cl=generator.getClass();
String name=cl.getName();
/*name此时等于:java.util.Random*/
```

初次之外我们还可以使用类名来创建生成一个Class对象，此时调用静态方法'forName(className)'->

```java
/*
如果类名保存在字符串中，并可在运行中改变，就可以使用这个方法。当然，这个方法只有在className是类名或接口名时才能够执行。否则，forName方法将抛出一个checked exception（已检查异常）。无论何时使用这个方法，都应该提供一个异常处理器(exception handler)。
*/
String name="java.util.Random";
Class cl=Class.forName(name);
```

使用场景:在启动时，包含main方法的类被加载。它会加载所有需要的类。这些被加载的类又要加载它们需要的类，以此类推。对于一个大型的应用程序来说，这将会消耗很多时间，用户会因此感到不耐烦。可以使用下面这个技巧给用户一种启动速度比较快的幻觉。不过，要确保包含main方法的类没有显式地引用其他的类。首先，显示一个启动画面；<u>然后，通过调用Class.forName手工地加载其他的类</u>。

**注:** 一个Class对象实际上表示的是一个类型，而这个类型未必一定是一种类。例如，int不是类，但int.class是一个Class类型的对象注释：Class类实际上是一个泛型类。例如，Employee.class的类型是Class\<Employee>

除此之外，当我们获得了一个Class对象以后可以调用newInstance()创建一个该类型的实例对象:

```java
String name="java.util.Random";
Class cl= null;
cl = Class.forName(name);
Random r= (Random) cl.newInstance(); //返回值是一个Object对象,初次之外需要注意的是该方法目前已被弃用，而且该方法只可以使用无参的构造函数创建一个对象，如果这个类不存在无参构造函数则会抛出一个异常。
```

## 反射机制

反射库（reflection library）提供了一个非常丰富且精心设计的工具集，以便编写能够动态操纵Java代码的程序。这项功能被大量地应用于JavaBeans中，它是Java组件的体系结构。使用反射，Java可以支持Visual Basic用户习惯使用的工具。特别是在设计或运行中添加新类时，能够快速地应用开发工具动态地查询新添加类的能力。能够分析类能力的程序称为反射（reflective）。反射机制的功能极其强大，反射机制可以用来：

● 在运行时分析类的能力。

● 在运行时查看对象，例如，编写一个toString方法供所有类使用。

● 实现通用的数组操作代码。

● 利用Method对象，这个对象很像C++中的函数指针。

**反射机制对于类的分析**:

我们的java.lang.reflect包即为反射类reflect的包，其提供了Field,Method,Constructor三个类分别用以描述类的域，方法和构造器。这些域，方法和构造器又都包含了一个getModifiers方法，这个方法返回一个int值，表示类的修饰符。之后我们可以调用Modifier.toString()将其转为字符串类型。除了修饰符以外，它们也都包含了自己的名称，这些名字则都可以调用getName()方法获得。对于域来说，它还含有一个数据类型，这个类型则可以使用getType()[返回一个Class类型]方法进行获得。对于方法来说，它们还有一个返回值类型(包括了void)，其通过getReturnType()[返回一个Class]方法进行获得。对于构造器来说，它们还有一个参数类型，其通过getParameterTypes()[返回一个参数列表类的数组]方法进行获得，该方法对于构造方法同样适用。

**域**->Field[] fields = cl.getDeclaredFields();[cl表示一个class类型的对象]

getModifiers()方法返回一个int值，表示域的(所有)修饰符。

getType()方法返回一个Class类型，表示域的数据类型。

getName()方法返回一个String类型，表示域的名称。

**构造方法**->Constructor[] constructors = cl.getDeclaredConstructors();[cl表示一个class类型的对象]

getModifiers()方法返回一个int值，表示构造器(所有)的修饰符。

getParameterTypes()方法返回一个参数列表类的数组，表示构造方法的参数类型。

getName()方法返回一个String类型，表示构造方法的名称。

**方法**-> Method[] methods = cl.getDeclaredMethods();[cl表示一个class类型的对象]

getModifiers()方法返回一个int值，表示方法(所有)的修饰符。

getReturnType()方法返回一个Class类型，表示方法的返回类型。

getParameterTypes()方法返回一个参数列表类的数组，表示方法的参数类型。

getName()方法返回一个String类型，表示方法的名称。

**对于域和方法来说我们还可以做一些更好玩的事情**:

**域** ->我们可以通过Class的一个实例对象调用getDeclaredField(fieldName),此时我们将获得该类的一个指定的域，返回这是一个Field对象。如果我们此时再调用该Field对象的get(instance)(instance为该类型的一个实例对象)方法，则会返回该instance对象的该域的值的一个对象(返回值是一个Object,但是如果输出它将会获得一个该域的字符串结果):

```java
Employee bob = new Employee("Bob Grandson", 50000, 1989, 10, 1);
Class cl=bob.getClass();
Field field = cl.getDeclaredField("name");
field.setAccessible(true);  //因为name是一个私有域，所以我们首先要开放访问权限才可以进行下一步的get操作
Object obj1=field.get(bob);
System.out.println(obj1);
System.out.println();
//结果:Bob Grandson
```

**注**:get方法有一个需要解决的问题。name域是一个String，因此把它作为Object返回没有什么问题。但是，假定我们想要查看salary域。它属于double类型，而Java中数值类型不是对象。要想解决这个问题，可以使用Field类中的getDouble方法，也可以调用get方法，此时，反射机制将会自动地将这个域值打包到相应的对象包装器中，这里将打包成Double。

**方法** ->与前者类似，我们可以调用getDeclareMethod(methodName(字符串类型),paramTypes(Class类型)...)来获取指定的方法，返回的是一个Method对象。如果我们想调用该方法，则需要调用该Method对象的invoke(instance,args...)方法，其中instance为该类型的一个实例对象，args为该方法的参数列表。

**注:** 我们需要获得一个基本数据类型的Class可以使用"基本数据类型名.class"

```java
Employee bob = new Employee("Bob Grandson", 50000, 1989, 10, 1);
Class cl=bob.getClass();
Method method=cl.getMethod("raiseSalary",double.class);
method.invoke(bob,20.0);
System.out.println(bob.getSalary());
//结果:60000.0
```

**利用反射原理创建数组:**

当一个数组被声明时为某一种类型，那么该数组可以在暂时的转换为Object类型之后再转回原本的类型，但是一旦一个数组初始声明便被声明为一个Object类型，则该数组不在可以被转为其他类型了，因此当我们希望动态的创建一个范型数组是则需要提前获悉目标数组的类型。
