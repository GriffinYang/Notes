# C语言学习记录

## stdio.h：它“包含”了C语言标准输入/输出库的相关信息，我们以#include <stdio.h>将其导入至程序之中

## C语言的执行步骤

1.预处理。首先程序会被送交给预处理器（preprocessor）。预处理器执行以#开头的命令（通常称为指令）。预处理器有点类似于编辑器，它可以给程序添加内容，也可以对程序进行修改。

2.编译。修改后的程序现在可以进入编译器（compiler）了。编译器会把程序翻译成机器指令（即目标代码）。然而，这样的程序还是不可以运行的。

## 编译C语言程序
```bash
cc -o name target.c #使用该语句在UNIX系统中编译target.c，其输出的名称则为name，其中 -o name 可以被省略
```

```bash
gcc -o name target.c #使用gcc编译target.c，name
```

## C程序的基本格式

```bash
指令
int main(void) #此处的void表示该函数不存在参数，如果是int改为void则表示该函数不返回值
{
    语句
}

```

## 指令：在编译C程序之前，预处理器会首先对其进行编辑。我们把预处理器执行的命令称为指令

其中#incude就是一个指令意为将指定的信息(如<stdio.h>)在**编译之前**包含在程序之中，而如<stdio.h>这样的就称之为头(header),每个头都包含一些标准库的内容。所有指令都是以字符#开始的。这个字符可以把C程序中的指令和其他代码区分开来。指令默认只占一行，每条指令的结尾没有分号或其他特殊标记

## 函数:它们是用来构建程序的构建块

函数分为两大类：一类是程序员编写的函数，另一类则是作为C语言实现的一部分提供的函数。我们把后者称为库函数（library function），因为它们属于一个由编译器提供的函数“库”。。在C语言中，函数仅仅是一系列组合在一起并且赋予了名字的语句。某些函数计算数值，某些函数不这么做。计算数值的函数用return语句来指定所“返回”的值。

虽然一个C程序可以包含多个函数，但只有main函数是必须有的。main函数是非常特殊的：在执行程序时系统会自动调用main函数

**注:** main函数的名字是至关重要的，绝对不能改写成begin或者start，甚至写成MAIN也不行。

**注:** 当我们把一个包含小数点的常量赋值给float型变量时，最好在该常量后面加一个字母f（代表float）

## 占位符

| 占位符 |                             含义                             |
| :----: | :----------------------------------------------------------: |
|   %d   |                         打印整数数据                         |
|   %c   |                      打印字符格式的数据                      |
|   %f   | 打印浮点数字（打印小数），**我们可以使用%.nf 的格式使其保留<u>n</u>为小数** |
|   %p   |                       以地址的形式打印                       |
|   %x   |                       打印十六进制数字                       |
|   %s   |                          打印字符串                          |
|  %lf   |                         打印长浮点数                         |
|   %e   |                        打印科学计数法                        |
|   %g   |       打印小数或者科学计数法 （且不输出毫无意义的零）        |
|  %ld   |                          打印长整型                          |
|   %u   |          打印十进制数输出unsigned型数据（无符号数）          |
|   %o   |                      打印八进制数字整数                      |

## 初始化式（initializer）

当我们声明了一个变量后可以对其进行赋值或者在声明时直接赋值，那么这个被赋予的值便称之为一个初始化式：

```c
int height=8; //数值8即为一个初始化式
```

我们也可以这样声明多个同类型的变量：

```c
int height=8,length=12,width=10;
```

此时8，12，10都是各自变量的一个初始化式，所以这几个变量的初始化式都是独立的。也就意味着

```c
int height,length,width=10;
```

此时仅有width被初始化了，而其他两个变量却未被赋值。

### 域宽

我们可以指定所输入的数据所占的宽度，如:

```c
scanf("%4d",&x); //此时我们便指定类x的位宽为4，这里的&是地址运算符,它将会获取指定变量的地址，如&x便会获取x的地址
//此时我们键入123456	，则x=1234
```

### sizeof

sizeof函数将会获取指定变量所被分配的空间大小，单位为字节:

```c
printf("long:%d bytes \n",sizeof(long)); //结果为long：4 bytes
```

### 赋值无效化

当%后存在一个*，则该次赋值无效:

```c
scanf("%3d %*2d %3d",&a,&b);
//此时我们键入，123 24 141， 123和141将会被分别赋给a和b，而24将被无效化，其不会将值赋给任何变量
```

### 字符键入

当我们使用%c来获取字符串类型数据时，**空格**或者**,**等均可以被视为时一个合法字符，因此我们需要将两个字符连续键入:

```c
scanf("%c%c",&a,&b);
//键入 a b  则a='a' b= 
//键入 ab 则a='a',b='b'
```

### scanf('%d',&x):获取用户输入的值将其赋给x并将x的值赋给前面字符串所对应的%d，这里可以含有多个值，但是它们需要按照先后顺序一一对应

scanf会返回一个整数，而这个整数便是其扫描并实现赋值的总数:

```c
scanf("%d%d",&a,&b)  //若成功执行，则将获得2
```

### 逗号表达式

在C语言中，逗号也是一个运算符，其可以组成逗号表达式，逗号表达式将会自作向右进行求解，最右侧得到的结果将会成为该表达式的解:

```c
int result=（a=3*5,a*4),a+5 
//结果为20，首先a=3*5=15,而后a*4等于15*4=60，因此(a=3*5.a*4)结果为60，但是后面的a+5将等于15+5=20,因此整个表达式的结果应该为20
//如果说我们希望结果为=65，则我们需要将a*4的值赋给a：
int result=（a=3*5,a=a*4),a+5
```

**由上面的运算我们可以得出结论，我们使用逗号表达式时，首先会自自左向右进行运算，变量的值随着运算的进行而改变，而最终的结果将以最右侧的表达式为准**

## 使用math方程库

如果我们希望调用一些指定的数学方法，如平方pow,开方sqrt等我们需要引入数学方法库**<math.h>**;但是需要额外注意的是，我们在使用gcc编译时还需要添加参数-lm以确保数学方法库可以被引入:

```sh
gcc -o main_01 math_01.c -lm
```

## 定义常量

在C语言中我们需要使用#define命令来定义一个常量，这也就意味着它并非是定义在方法之中的，而是与#include <stdio.h>是类似的。

当我们使用#define命令定义一个常量时，可以采用称为宏定义（macro definition）的特性给常量命名：

```C
#define FINAL_VALUE value
```

注意此时我们的value前**不可以有'='号**，否则在调用该常量时这个常量值前面将同样包含一个'='，初次之外若我们希望该常量是一个表达式，则这个值应由一个括号包含，如：

```C
#define FINAL_VALUE (3+2)
#include <stdio.h>
int main(){
    printf("result=%d\n",FINAL_VALUE);
    return 0;
}
```

**注1:** 宏的名字只用了大写字母。这是大多数C程序员遵循的规范，但并不是C语言本身的要求。

**注2:** 在C语言之中如果两个整数相除，那么C语言会对结果向下取整。所以如2/3=0,为了解决这个问题我们可以使用2.0f/3.0f代替

**注3:** 在C语言中，标识符可以含有字母、数字和下划线，但是必须以字母或者下划线开头。

**注3:** 我们的函数语句可以都写在一行，但是不包括预处理指令，因为预处理指令要求独立成行

## 格式化输入和输出

### printf函数

printf函数被设计用来显示格式串（format string）的内容，并且在该串中的指定位置插入可能的值。调用printf函数时必须提供格式串，格式串后面的参数是需要在显示时插入到该串中的值：

```bash
printf(格式串，表达式1，表达式2，...);
```

显示的值可以是常量、变量或者更加复杂的表达式。调用printf函数一次可以打印的值的个数没有限制。

格式串包含普通字符和转换说明（conversion specification），其中转换说明以字符%开头。转换说明是用来表示打印过程中待填充的值的占位符。跟随在字符%后边的信息指定了把数值从内部形式（二进制）转换成打印形式（字符）的方法，这也就是“转换说明”这一术语的由来。

案例:8

```c
int i,j;
float x,y;
i=10;
j=20;
x=43.2892f;
y=5527.0f;
printf("i=%d,j=%d,x=%f,y=%f\n",i,j,x,y);
/*输出结果为:i=10,j=20,x=43.289200,y=5527.000000*/
```

格式串中的普通字符被简单复制给输出行，而变量i、j、x和y的值则依次替换了4个转换说明。

然而语言编译器不会检测格式串中转换说明的数量是否和输出项的数量相匹配，所以表达式和转换说明之间应当保持一致，否则将会编译失败:

```c
printf("%d %d\n",i); /*转换说明大于输出项*/
printf("%d\n",i,j); /*输出项大于转换说明*/
/*两行均会编译失败*/
```

此外，C语言编译器也不检测转换说明是否适合要显示项的数据类型。如果程序员使用不正确的转换说明，程序将会简单地产生无意义的输出。

当我们显示格式化一个数字时，基本格式为:"%a.bt":

其中a代表的时数字的位数，如果是3，则数字为一位数到两位数时，数字将会右对齐，如果此时加上一个符号，则可以左对齐。如果位数大于等于三位时则自动增加位数，并不会出现精度丢失的情况。此称之为最小字段宽度。而且a可以被省略。

b代表的是小数点后精确的位数，如果是2则会精确两位，以此类推，注意当该数字被省略时，小数点也应该被省略。如果这里的类型是一个整数型，那么此时的a表示的仍是位数，但是b此时表示显示的位数，位数不足以0填充，如%5.5d即表示，总位数为五，位数需要显示五位，此时位数不足以0填充，因此显示"40‘时结果为"00040"

t代表的是数字的类型，如果是f，则是浮点数。

### scanf函数

就如同printf函数用特定的格式显示输出一样，scanf函数也根据特定的格式读取输入。像printf函数的格式串一样，scanf函数的格式串也可以包含普通字符和转换说明两部分。scanf函数转换说明的用法和printf函数转换说明的用法本质上是一样的。

```c
int i,j;
float x,y;
scanf("%d%d%f%f",&i,%j,&x,&y);
```

假使输入的值为: 1 -20 .3 -4.0e3

此时1，-20，0.3，-4000.0分别被输入到i、j、x和y中

### scanf的工作原理

像printf函数一样，scanf函数是由格式串控制的。调用时，scanf函数从左边开始处理字符串中的信息。对于格式串中的每一个转换说明，scanf函数从输入的数据中定位适当类型的项，并在必要时跳过空格。然后，scanf函数读入数据项，并且在遇到不可能属于此项的字符时停止。如果读入数据项成功，那么scanf函数会继续处理格式串的剩余部分；如果某一项不能成功读入，那么scanf函数将不再查看格式串的剩余部分（或者余下的输入数据）而立即返回。

**注** :在寻找数的起始位置时，scanf函数会忽略空白字符（white-space character，包括空格符、水平和垂直制表符、换页符和换行符）

scanf会一值监听，直到它收集到指定数量的数字才会罢手，此外，根据它的识别识别方法，使用空格可以分开两个数字，如12 23它可以识别为12和23。当有符号时，也可以写作:12-23此时获得12和-23

**注** :printf格式串经常以\n结尾，但是在scanf格式串末尾放置换行符通常是一个坏主意。对scanf函数来说，格式串中的换行符等价于空格，两者都会引发scanf函数提前进入到下一个非空白字符。例如，如果格式串是"%d\n"，那么scanf函数将跳过空白字符，读取一个整数，然后跳到下一个非空白字符处。像这样的格式串可能会导致交互式程序一直“挂起”直到用户输入一个非空白字符为止。

**注** :在printf中我们可以使用%i或者%d来表示整数，但是在scanf中，前者可以用一匹配十进制，十六进制和八进制的数，但是后者只可以匹配十进制的数。如果输入的数有前缀0（如056），那么%i会把它作为八进制数来处理；如果输入的数有前缀0x或0X（如0x56），那么%i会把它作为十六进制数来处理

**注** :如果printf函数在格式串中遇到两个连续的字符%，那么它将显示出一个字符%

**注** :水平制表符之间的距离通常是8个字符宽度，但C语言本身无法保证这一点

**注** :格式串中的\g和\e都会将小数转为科学计数法的形式,如果所精确的小数位全部为0，那么\g会将小数位省略，如果小数位不全为0，那么\g会将小数位显示出来,而e将始终显示所有小数位，即使小数位全部为0

**注** :当我们使用scanf时，我们有空格和无空格时有所区别的:

```c
 scanf("%d /%d",&num1,&denom1);  //有空格
 scanf("%d/%d",&num1,&denom1);   //无空格
 /*此时前者当我们输入2 /3时，它可以正常接收到2/3，将其分别赋予num1和denom1；而后者当我们输入2/3时，它只能接收到2，将其赋予num1，而denom1被设置为一个随机值，而如果输入时2/3则两者时没有区别的。也就是说，当我们输入的数据间如果存在一个符号且可能存在空格，那么我们需要在格式串中预留一个空格*/
```

**注** :scanf不可以像printf那样控制浮点数的精度，但是可以规定数字的位宽，包括了浮点数和整数:

```c
scanf("%3d",&a); //只接收3位位宽的整数,s输入123456，将获得123
scanf("%4f",&a); /*只接收4位位宽的浮点数,其包含了小数点的四位位，但是如果整数部分位宽已经大于或等于设置位宽，则不会获取到小数点为，如1234.56,将获取浮点数1234.0，实际获取了1234，'.0'是浮点数自动追加的;而如果整数部分位宽小于设置位宽，则会获取到小数点及以后以达到设置位宽位置，如1.23456,将获取浮点数1.23，实际获取'1.23'，共占据4个位宽*/
```

## 数组

### 数组的内存占用

一维数组在内存中连续占用的字节数=数组的元素个数*sizeof(数据类型)

与Java不同，C语言中的数组并不是一个对象，因此我们无法直接引用一个数组，只能引用一个数组的一个指定的元素。

### 数组的引用

当我们定义了一个字符数组后，我们使用**数组名[索引]**将会获取数组中指定索引的值的引用，此时我们可以对其进行获取和修改，但是我们不可以直接引用一整个数组，否则会编译失败：

```c
	/*定义字符数组*/
	char str[]={'H','e','l','l','o'};
    printf("%c \n",str);
	
	/*定义数字数字*/
    int integers[]={1,2,4,4};
    printf("%d \n",integers);
	
	/*以上两者均会编译失败*/
```

然而，字符数组存在例外，因为字符数组可以在被视作字符串时被直接引用

```c
/*定义字符数组*/
	char str[]={'H','e','l','l','o'};
    printf("%s \n",str); /*输出'Hello'*/
```

### 字符串的声明

```c
char str[]="Hello world!";
    printf("%s \n",str); /*输出Hello world!*/
```

### 字符串处理函数

尽管我们的数组是不存在默认的函数库的，但是，我们的字符串(数组)是拥有函数库的[**<string.h>**]

#### 字符串输入函数fgets

使用该方法我们可以将从终端获取的字符串赋予某一个指定的字符串

```c
char str[20];
fgets(str,20,stdin); /*str表示目标的接收字符串，20表示输入的限制，stdin表示标准输入流(宏数据)*/
printf("str is %s\n",str);
```

#### 字符串输出函数fputs

前面是将终端的内容输入至字符串，后面便是将字符输出至终端

```c
 char str[20];
 fgets(str,20,stdin);
 fputs(str,stdout); /*输出值等于前面的输入值，stdout表示标准输出流*/
```

#### 字符串的长度strlen

```c
char str[]="hello"
strlen(str); /*获得str的长度5*/
```

#### 字符串输入gets

```c
char str[20];
gets(str); /*此时输入'text','text'将会被储存在str中*/
```

#### 字符串输出puts

```c
char[]str="hello world";
puts(str); /*输出'hello world'*/
```

#### 字符串拼接strcat

```c
char str1[]="Hello,";
char str2[]="Rongxin!";
puts(strcat(str1,str2)); /*输出Hello,Rongxin!*/
```

#### 字符串复制strcpy

```c
char str1[20];
char str2[]="hello";
puts(strcpy(str1,str2)); /*输出‘hello’,需要注意的是是将str2的值赋给了str1*/
```

#### 字符串大小比较strcmp

```c
strcmp(str1,str2);
/*
*此时str1和str2上的每一个字符将会被依次进行比较，直到判断出结果或者到最后一位为止。
*如abc与cba进行比较，则由于a<b，所以返回-1;而acb和abc则因为c>b返回1;注意，该比较仅与ACCII码的大小相关。
*与字符串的长度无关:ac>abc是成立的。如果两个字符串的所有字符均完全相等，则返回0,如abc与abc
*/
```

#### 字符串大写转小写strwr

strupr(str)

#### 字符串小写转大写strupr

strupr(str)

### 多维数组

在Java中我们规定了多维数组的概念，在C语言中也存在。然而在C语言中多维数组可以声明为多维数组，但是实际储存时，无论是几维数组由于它们都是占用一段连续的内存空间所以都是以一维数组的形式进行储存的；而Java的多维数组则是一维数组嵌套一维数组，因此每一个小数组都是独立的数组，因此在Java中可能出现数组下标越界异常的情况在C语言中并不会出现：

```c
#include <stdio.h>
#define OUTTER 3
#define INNER 4
int main(void){
    int arr[OUTTER][INNER]={{1,2,4,6},{2,23,44,14},{90,45,76,11}};
    /*输出14*/
    printf("%d\n",arr[1][3]);
    /*输出90，并不会出现数组下标越界异常*/
    printf("%d\n",arr[1][4]);
    return 0;
}
```

根据C语言的多维数组存储特点我们甚至可以将上文代码写作:

```c
#include <stdio.h>
#define OUTTER 3
#define INNER 4
int main(void){
    int arr[OUTTER][INNER]={1,2,4,6,2,23,44,14,90,45,76,11};
    printf("%d\n",arr[1][3]);
    printf("%d\n",arr[1][4]);
    return 0;
}
```

而在Java中则会出现异常:

```java
class Flower{
    public static void main(String[] args){
        Integer arr[][]={{1,2,4,6},{2,23,44,14},{90,45,76,11}};
        /*输出14*/
        System.out.println(arr[1][3]);
        /*抛出下标越界异常*/
        System.out.println(arr[1][4]);
    }
}
```



## 产生随机数

在C语言中，如果我们希望创建一个随机数，则我们需要引用**<stdlib.h>**库，而后调用其内部的rand方法即可，但是需要注意的是，此时我们获得的随机数是固定的，因为在调用rand方法前需要首先调用srand方法，为后面的rand创建一个唯一的种子。当srand中的种子是同一个值时，则获取的随机数是不变的。为此我们可以使用时间戳(需要引入**<time.h>**)作为种子:

```c
srand((unsigned)time(NULL)); 
printf("%d \n",rand());
```

### 注:在C语言里，将1表示为逻辑真，0表示逻辑假。

## 函数

在C语言中，函数是可以可以被定义的在主函数之下，但是需要保证在调用该函数前，我们首先在主函数之前声明该函数:

```c
#include <stdio.h>
int fn(para1,para2);
int main(void){
    ...
    fn(a,b);
    ...
}
int fn(para1,para2){
    ...
}
```

### 变量

#### 变量的作用域

和其他语言一样，在C语言中每一个函数都有自己的变量作用域，一个函数**内部**定义的变量将不可以在该函数外被调用。除此之外，C语言遵循自上而下的运行逻辑，因此，当一个变量被定义在一个函数后，那么该函数也将无法调用该变量:

```c
#include <stdio.h>
int a=0;
void f(){
    printf("%d",a);
    printf("%d",b); /*编译失败*/
}
int b=1;
int main(void){
    fn();
    printf("%d",a);
    printf("%d",b);
    return 0;
}
```

事实上即使b被定义在函数内部，如果是定义在被调用后，同样会编译失败:

```c
#include <stdio.h>
int a=0;
void f(){
    printf("%d",a);
    printf("%d",b); /*编译失败*/
    int b=1;
}
int b=1;
int main(void){
    fn();
    printf("%d",a);
    printf("%d",b);
    return 0;
}
```

**还有一点我们需要注意:**之前我们有强调过，在我们调用一个函数前首先需要声明该函数，以便告诉main函数我们有这样一个函数以使其可以完成调用，然而我们注意当上文中，我们将fn函数定义于main函数，并且没有再声明**fn()**以告诉main函数，这里存在一个fn函数;事实上如果我们这样做了，反而会编译异常，因此我们需要知道，**仅当我们将main函数所需要调用的函数定义于main函数之后时，才需要额外地声明该函数**。

#### 调用全局变量

事实上，无论是之前我们声明的a，或者是b。由于它们都是在main函数之外，并先于main函数被定义的，因此它们都是全局变量。只是这种全局变量无法直接被声明在其被声明前的函数所调用。当然，我们只是无法直接调用而已，如果说我们需要调用也是可以实现的，我们仅需在**被调用前**额外引入即可:

```c
void fn(){
    /*此时我们的全局变量b被加入到了该函数中，我们可以直接调用*/
    extern int b;
    printf("I-> \n");
    printf("%d \n",a);
    printf("%d \n",b);
}
int b=1;
```

<a style="color:red;">注意:**所有的全局变量都是静态变量**，当一个全局变量的值被更改了，那么该变量在下一次被调用时便会以被修改后值进行运算。</a>

```c
#include <stdio.h>

int a=0;
void alpha();
void beta();
int main(void){
    alpha();
    beta();   
    return 0;
}
void alpha(){
    a++;
    printf("a->%d \n",a);
}
void beta(){
    a*=5;
    printf("a->%d \n",a);
}
```

bb输出的结果为:

```
a->1
a->5
```



#### 声明局部的静态变量

我们前文提到所有的全局变量都是静态变量，但是局部变量则默认都是动态的，这也就意味着，每一次函数调用完成后，其内部的局部变量便会便销毁，因而下一次该函数被再次调用时，这个局部变量将又会被重新初始化，而后再被调用:

```c
#include <stdio.h>
void testStatic();
int main(void){
    for (size_t i = 0; i < 5; i++)
    {
        testStatic();
    }
    return 0;
}
void testStatic(){
    int a=0;
    a++;
    printf("a->%d \n",a);
}
/*输出结果:
a->1 
a->1 
a->1 
a->1 
a->1 
*/
```

```c
#include <stdio.h>
void testStatic();
int main(void){
    for (size_t i = 0; i < 5; i++)
    {
        testStatic();
    }
    return 0;
}
void testStatic(){
    static int a=0;
    a++;
    printf("a->%d \n",a);
}
/*输出结果:
a->1 
a->2 
a->3 
a->4 
a->5 
*/
```

### 函数与带参数的宏

所谓的宏它首先是在编译前就被生成了，而函数则是运行时才会被生成

#### 宏的定义

**#define 宏的名称 （宏的参数列表） (宏的内容体（可以写入参数）)**

**#define 宏名 (内容体)**

以上的括号都是可以省略的，但是为了更易识别，尤其是前者，我们尽量添加括号即可。除此之外，宏相较于函数而言只是一个文本替代而已，它不需要对于参数进行类型的检测，而函数则是需要对于参数进行判断，以确保所输入的参数与所得到的结果和运算过程兼容。总共言之，宏可以被认为是一种通用的文本模板，如果我们在代码中存在重复的代码，我们便可以将它们提取出来，并为之命名，以减少代码量，精简代码。

#### 无参数的宏

```c
#include <stdio.h>
#include <math.h>
#define PI (asin(1)*2)
double area(double a,double b);
int main(void){
    printf("result is %f \n",PI); /*充当函数的作用*/
    printf("the area is %f \n",area(2,3));/*充当通用代码的文本替代*/
}
double area(double a,double b){
    return PI*a*b; /*等价于" (asin(1)*2)*a*b "*/
}
```

#### 有参数的宏

```c
#include <stdio.h>
#define MAX(a,b) (a>b?a:b)

int main(void){
    int a=3;
    int b=2;
    printf("the greater one between %d and %d is %d\n",a,b,MAX(a,b));
    return 0;
}
```

**需要注意的一点是宏的参数和宏的名称之间不可以存在空格，必须是紧紧相靠才可以**

#### 宏与字符串

我们可以直接定义一个字符串的宏:

```c
#include <stdio.h>
#define greet "Hello World!"
int main(void){
    printf("%s\n",greet);
    return 0;
}
```

我们也可以让一个带参数的宏输出字符串:

```c
#include <stdio.h>
#include <string.h>
#include <stdarg.h>
#include <stdlib.h>
#define MAX(a,b) (a>b?a:b)
#define M(m) (#m)
#define N(n) (#n)
char* concatStrs(int count,char* str,...);
int main(void){
    int a=3;
    int b=2;
    char* result=concatStrs(5," the greater one between ",M(2)," and ",N(3)," is ");
    printf("%s%d\n",result,MAX(2,3));
    free(result);
    return 0;
}

char* concatStrs(int count,char* str,...){
    va_list args;
    va_start(args, str);
    char* result;
    result=str;
    for (int i = 1; i < count; i++)
    {   
        char value[100];
        strcpy(value,va_arg(args,char*));
        char src[500];
        strcpy(src,result);
        
        result=strcat(src,value);
    }
    va_end(args);
    char* returnValue=(char*)malloc(50);
    strcpy(returnValue,result);
    return returnValue;
}
```

## 指针

### 内存地址

在计算机中，我们用于存储数据的字节对于内存进行比编址的，即每***8*位为一个内存地址**:

```text
00000001
00000011
...
01111111
11111111
```

而每一个不同的数据类型所占用的内存大小可能是不同的，如float占用的是4个字节，而char类型仅占用1个字节。然而无论是一个还是四个字节，每种数据类型所占用的空间都称之为一个内存单元，**<u>占用多个字节的内存单元是总是连续的</u>**，而且**<u>我们将这个内存单元的最小的字节地址作为整个内存单元的地址</u>**:

```````````shell
``````````
`1008H   A char
``````````
` ... `
```````
`2000H````
```````  `
`2001H`  `
```````  B float
`2002H`  `
```````  `
`2003H`  `
``````````
```````````

此时我们将A的地址视为1008H,将B的地址视为2000H。

### 指针的概念

**当我们定义了一个变量时，这个<u>变量的指针便是该变量的地址</u>，而我们将存放指针的变量称之为<u>指针变量</u>**。

因此变量的指针也就是变量的地址，而变量所存储的值与它们两者则是不同的概念。在C语言中我们可以对一变量使用&来获取一个变量的地址,在变量前添加一个*号表示指针变量:

```c
int a;
int *pointer; /*定义一个指针变量*/
pointer=&a;	/*将a的地址赋予指针变量,此时的*pointer就等价于变量a*/
*pointer=1;	/*因为*pointer等价于a，因此该语句等价于a=1*/
```

我们需要注意的是，我们可以在定义指针变量后在为之赋予指定变量的指针，如上文中的pointer=&a，亦可以在定义时赋予，如int *pointer=&a。因此:

```c
int a;
int *pointer1=&a; 
/*<=等价于=>*/ 
int *pointer2;
pointer2=&a; 
```

**不过需要注意的是，当我们使用后者时，pointer前不可添加***；

前面我们不允许在赋予指针变量变量地址时添加*是因为我们希望为它赋值时添加,如前文所示:

```c
int a;
int *pointe&a;
*pointer=1;	
```

因此我们可以得到一个结论:

```text
在C语言中，指针变量直接指向对应变量的地址。而*指针变量则可以视作时被指向的变量,即若指针变量pointer指向变量a，则*pointer<==>a
```

前文中我们定义了指针变量，由此得知指针变量的定义如此:

**基类型 *指针变量名；**

其中的*表示该变量所定义的是一个指针变量，而基类型表示的是该指针变量所需要指向的变量的类型名，注意，**一个指针变量只能储存与基类型相同类型的变量的指针**。

&：取地址符，在变量前添加该标识符将获取该变量的地址

*:指针运算符，对于一个指针使用该标识符将会获取该指针所储存的值

取地址符和指针运算符时可逆的关系，两者会相互抵消，只是针对的对象时不同的，比如&只能直接置于变量之前，而*只能直接置于指针之前。如:

```c
int a;
int *pr=&a;
/*
此时
	*&a<==>a
	&*p<==>p 或者其他 &a
*/
```

**注:不能直接使用没有被赋予初始值的指针变量:**

```c
int *p;
*p=3;		/*注意，该赋值为非法赋值，因为指针变量p还未被赋值*/
```

### 指针与数组

每一个变量都存在一个地址，数组也不例外，一个数组世纪上就是一个由一块连续的内存单元所组成的。其中**<u>数组名便代表了整个数组的首地址</u>**。而所谓的数组指针实际上也就是首个数组元素的地址即起始地址。因此我们可以得出以下

结论:

```c
int a[10];
int *p;

/*以下两者等价*/
p=a;
p=&a[0]
```

数组指针的计算逻辑:

**a[i]  <=等价于=> *(a+i\*t)**

**a\[i][j]<=等价于=>\*（*(a+i\*t)+j\*t)**

**注:**(<u>此时t为该数据类型的字节大小</u>)

**注:****指针变量也可以使用++或者--来改变其自身的值，而数组变量则不可行:**

```c
int a[]={1,2,3,4,5};
int *p=a;
p++; /*合法操作*/
a++;	/*非法操作*/
```

### 指针与字符串

字符串，实际上就是一个数组，只不过时一个字符数组而已。因此它的存储模型和与指针的关系是类似的。我们可以使用指针或字符数组直接定义字符串:

```C
#include <stdio.h>
int main(void){
    char * str;
    str="Hello World";
    printf("%s\n",str);
    char arr[]="Hello World";
    char * pointer=arr;
    printf("%s\n",pointer);
    return 0;
}
```

**注:'="\\0"代表字符串的结尾**：

```c
#include <stdio.h>
/*计算字符串的长度*/
size_t stringLength(const char *str) {
    size_t length = 0;
    while (str[length] != '\0') {
        length++;
    }
    return length;
}

int main() {
    const char *str = "Hello, World!";
    size_t length = stringLength(str);
    printf("Length of the string: %zu\n", length);
    return 0;
}

```

### 函数与指针

实际上函数与字符串或者数组量也是类似的，它也是占用一段连续内存空间的"变量"，函数名也是其所占用的内存区域的首地址，因此我们也可以将函数储存在一个指针变量之中，这种变量我称之为**函数指针**，它的定义格式为:

```c
#include <stdio.h>
int add(int a,int b){
    return a+b;
}
int main(void){
    /* 类型说明符 （*指针变量名）（参数变量列表） */
    int (*ADD_VALUE)(int,int);
    int a=10,b=2;
    ADD_VALUE=add;
    printf("a+b=%d\n",ADD_VALUE(a,b));
    return 0;
}
```

**注:**

* 函数的指针变量不可以像数组的指针那样进行算术运算，它的移动是无意义的。
* 我们指针变量的括号是不可省略的"（*指针变量名）"

### 多级指针

指针的核心在于指向关系，如果存在A指向B(A=&B)，则A的值便是B的地址，如果我们希望访问B则可以通过*来实现，而此时如果还存在B指向C的关系(B=&C)。并且C是用以储存基本类型或者诸如数组元素的值，则此时C被称为**一级指针变量**，B和A分别被称之为二级和三级指针变量，一个N级直至如果希望最后得到真正的储存值也就是一级指针变量的值便需要不断地追溯自身指向的变量，直到一级指针变量。

```shell
    n级指针                二级指针    一级指针 非指针型变量 
      pn                     p2         p1       x
+--------------+          +-----+     +----    +---+
|n-1级指针的地址|-- ... -> | &p1 |--->| &x |--->| 7 |
+--------------+          +-----+     +----    +---+
```

一级变量因为需要指向一个非指针变量因此有一个\*,而二级和三级分别需要指向一级和二级指针变量因此分别需要两个和三个\*：

```c
/*非指针变量*/
int x=7;
/*一级指针变量*/
int *p1=&x;
/*二级指针变量*/
int **p2=&p1;
/*三级指针变量*/
int ***p3=&p2;
```

一般情况下，C语言中只会使用到三级指针变量。除此之外我们需要额外知晓:C语言对于指针的要求较为苛刻，只能一级指针指向非指针变量，二级指针变量指向一级指针变量...

### 指针数组

所谓的指针数组便是存储指针的数组，它是一组有序指针的集合，其中的所有的元素都是指向相同类型数据的指针，其一般形式为:

```c
/*类型说明符 *数组名[数组长度];*/
```

因为指针数组中每一个元素都是一个地址，而数组的数组名又是指向首个元素的指针，即指向一级指针的指针，因此是一个二级指针。因此**指针数组的数组名即为一个二级指针:**

```c
#include <stdio.h>
#define ARR_LEN 5
int main(void) {
	/*定义一个int数组*/
    int a[ARR_LEN] = {12,23,44,27,94};
	/*定义一个指针数组。注意，这里的num实际上是一个二级指针，但是这个数组却只是存储一级指针的数组因此数组定义时只有一个**/
    int *num[ARR_LEN];
	for (size_t i = 0; i < ARR_LEN; i++)
	{
		num[i] = &a[i];
	}
    /*上文提到了num是一个二级指针，因此这里接收num的p需要是一个二级zhi*/
	int **p, i;
	p = num;
	for (size_t i = 0; i < ARR_LEN; i++)
	{
		printf("%d,", **p);
		p++;
	}
	return 0;
}
```

## 结构体和共用体

在Java中我们可以使用类来抽象化一个实体，在C语言中我们则可以使用结构体来近似的表示类的概念。而之所以存在结构体的概念也与类的初衷很类似。

### 结构体的定义和初始化

!["结构体存储结构"](.\Structure_in_C_1.jpg)

#### 定义

1. 先定义结构体类型再定义结构体变量

   ```c
   #include <stdio.h>
   int main(void){
       /*struct:定义结构体的关键字,stu即为结构体类型*/
       struct stu  
       {
           char num[10];
           char name[20];
           char sex;
           int age;
           float score;
       };      /*定义结构体类型*/
   	
       /*根据结构体类型声明结构体变量*/
       struct stu alpha,beta;
       return 0;
   }
   ```

   

2. 定义结构体类型的同时定义结构体变量

   ```c
   #include <stdio.h>
   int main(void){
       /*结构体类型*/
       struct stu
       {
           char num[10];
           char name[20];
           char sex;
           int age;
           float score;
       }alpha,beta;/*结构体变量*/
       
   }
   ```

   

3. 不定义结构体类型直接定义结构体变量

   ```c
   #include <stdio.h>
   int main(void){
       struct
       {
           char num[10];
           char name[20];
           char sex;
           int age;
           float score;
       }alpha,beta;/*结构体变量*/
       
   }
   ```

   我们可以在一个结构体内定义其他结构体的结构体变量：

   ```c
   #include <stdio.h>
   
   int main(void){
       struct date
       {
           int month;
           int day;
           int year;
       };
   
       struct
       {
           char num[10];
           char name[20];
           char sex;
           struct date birthday;
           float score;
       }alpha,beta;
   }
   ```

   **初始化结构体变量**
   
   ```c
   #include <stdio.h>
   
   int main(void){
       struct
       {
           char num[10];
           char name[20];
           char sex;
           float score;
       }alpha,beta={
           "2232321",
           "rongxin",
           'M',
           83.5
       };
       alpha=beta;
       printf("Number is %s\nName=%s\n",alpha.num,alpha.name);
       printf("Sex is %c\nScore=%4.1f\n",alpha.sex,alpha.score);
       return 0;
   }
   ```
   
   **输出结果**

```shell
Number is 2232321
Name=rongxin
Sex is M
Score=83.5
```

### 单独赋值

```c
#include <stdio.h>
struct stu{
    char num[10];
    char name[20];
    char sex[10];
    int age;
    float scores;
};
int main(void){
    struct stu alpha;
    /*注意数组名是指针(数组的第一个元素的指针)*/
    *alpha.num=20201100;
    *alpha.name="rongxin";
    *alpha.sex="male";
    alpha.age=23;
    alpha.scores=83.5;
    return 0;
}
```

#### 当结构体包含其他结构体类型时的赋值

```c
#include <stdio.h>
struct date
{
    int month;
    int day;
    int year;
};

struct stu{
    char num[10];
    char name[20];
    char sex[10];
    int age;
    struct date birthday;
    float scores;
};
int main(void){
    struct date birthday;
    birthday.month=9;
    birthday.day=4;
    birthday.year=2000;

    struct stu alpha;
    *alpha.num=20201100;
    *alpha.name="rongxin";
    *alpha.sex="male";
    alpha.age=23;
    /*赋予其他结构体类型属性值*/
    alpha.birthday=birthday;
    alpha.scores=83.5;
    return 0;
}
```

### 结构体数组

结构体实际上可以立即为我们Java中的类，自然也就是可以认为是一种自定义的**类型**,因此我们可以创建同类型的数组即结构体数组:

```c
#include <stdio.h>

struct Person{
    char name[20];
    char gender[10];
    int age;
};

#define ARR_SIZE(arr) (sizeof(arr)/sizeof(arr[0]))
int main(void){
    struct Person persons[]={
        {name:"rongxin",gender:"male",age:23},
        {name:"yang",gender:"male",age:26},
        {name:"ning",gender:"female",age:22}
    };
    size_t size=ARR_SIZE(persons);
    for (size_t i = 0; i < size; i++)
    {
        struct Person person=persons[i];
        printf("{\nname:%s,\ngender:%s,\nage:%d\n},\n",person.name,person.gender,person.age);
    }
    // printf("size is %zu\n",ARR_SIZE(persons)); 使用sizeof得到的结果需要使用%zu接收
    return 0; 
}
```

### 指向结构体的指针

从结构体的存储结构上我们可以得知结构体的存储逻辑实际上和数组是类似的，数组的名称事实上是指向数组第一个元素的地址(指针)，因此我们使用带*号的变量即可储存数组名。对于结构体而言，它的指针变量指的**则不是**结构体变量的首地址，它更像是基本数据类型，指代的就是该结构(类型)的元素值，因此我们可以使用"."来调用它的各个成员。

定义结构体指针变量的格式:

**struct 结构体类型名 *结构体指针变量名;**

我们如果希望将指针变量指向一个结构体变量的首元素的地址则可以:

```c
#include <stdio.h>

struct person{
    char name[20];
    int age;
};

int main(void){
   struct person p={name:"rongxin",age:23};
    struct person *ponter=&p;	//取出首地址并赋予指针变量
    int arr[3]={2,4,5};

    // printf("name is %s\n",p);	注意此时p既不代指首元素也不代指首元素地址
    printf("first is %d\n",*arr);	//数组名代指首元素地址
    printf("{name:%s,age:%d}\n",p.name,(*ponter).age);
    printf("{name:%s,age:%d}\n",p.name,ponter->age); //与前者功能一致表示指向
    printf("{name:%s,age:%d}\n",p.name,(ponter++)->age); //输出age
    struct person *another=ponter++;
    printf("{name:%s,age:%d}\n",p.name,(another++)->age); //输出地址，这意味着此时another已经超出了person的范围

    return 0;
}
```

