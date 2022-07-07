# Java 基础篇

## 变量和数据类型

### 概念

计算机用以储存数据的容器

### 变量的组成

数据类型 标识符 = 数据；

### 标识符的命名规则

<ol>
<li>标识符由字母，数字，下划线和美元符号组成</li>
<li>标识符不可以以数字开头，同时不建议以美元符号开头</li>
<li>标识符不可以是Java中的关键字</li>
<li>标识符中若含有多个单词则遵循小驼峰命名法的规则</li>
<li>标识符需要做到见名知意</li>
</ol>

### 数据类型

#### 八大基本数据类型

##### 整数型

byte short int long

##### 浮点型

float double

##### 布尔型

boolean

##### 字符型

char

#### 引用数据类型

数组，类，接口

### 数据类型转换

#### 自动类型转换

当我们将小类型的数据置于大类型的数据容器之中，此时小数据类型数据将自动转为大的数据类型，此称之为自动类型转换
因为小的数据类型是必定可以置于大的数据类型容器之中的

#### 强制类型转换

与前者相反，由于当我们需要将大数据类型的数据置于小数据类型的容器之中时，有几率这个大数据类型的数据将会超过小
数据类型的范围，从而导致数据的精度的缺失。因此我们需要使用强制类型转换来进行类型的转换，而实现强制类型转换则需要我们在所需要转换的数据
前加上所需要转换的类型以括号包括：byte number=(byte)300;

### 引用数据类型转换

#### 向上转型

当我们将一个子类的对象赋予给一个父类的引用的时候，我们便对于这个子类对象进行了向上的转型，向上转型是没有风险的，但是我们使用了向上转型
则这个父类的引用无法使用子类的独有方法和变量

#### 向下转型

当我们将一个父类的对象赋予给一个子类的引用，我们便对于这个父类对象进行了向下转型，向下转型后，这个子类的引用可以使用子类的独有方法和变量
但是由于向下转型时，然而同一个父类可以被多个不同的子类继承，因此为了避免我们的这个向下转型的引用可以正确的调用自身的函数，我们可以使用
**instanceof**对于我们当前创建的示例进行判断从而决定调用哪一个子类的特有方法

```java
  class A {
         public void print() {
                  System.out.println("A:print");
         }
}

class B extends A {
         public void print() {
                  System.out.println("B:print");
         }
         public void funcB(){
                  System.out.println("funcB");
         }
}

class C extends A {
         public void print() {
                  System.out.println("C:print");
         }
         public void funcC(){
                  System.out.println("funcC");
         }
}

public class Test{
         public static void func(A a)
         {
                  a.print();
                  if(a instanceof B)
                  {
                          B b = (B)a;   //向下转型,通过父类实例化子类
                          b.funcB();    //调用B类独有的方法
                  }
                  else if(a instanceof C)
                  {
                          C c = (C)a;  //向下转型,通过父类实例化子类
                          c.funcC();   //调用C类独有的方法
                  }
         }

         public static void main(String args[])
         {
                  func(new A());
                  func(new B());
                  func(new C());
         }
}

```

## 运算符

### 表达式的定义：数据与运算符的组合

#### 赋值运算符

"="

#### 算数运算符

**基本算数运算符：**

“+”，“-”，“\_”，“/”，“%”，“++”，“--”

**复合算术运算符：**

“+=”，“-+”，“\_=”，“/=”，“%=”，

#### 关系运算符

“>”，“>=”，“<”，“<=”，“==”，“!=”，

#### 逻辑运算符

“&”，“&&”，“|”，“||”，“!”，

#### 条件运算符(三元运算符)

条件？表达式一：表达式二

#### 位运算符

“>>”，“<<”，“>>>”,"&"（同一为一）,"|"（同零为零）,"~"（取反），"^"（异一同零）

#### 运算符的优先级

括号级别最高，逗号级别最低，单目 > 算术 > 位移 > 关系 > 逻辑 > 三目 > 赋值

### 选择结构

1）if 结构

2）switch 结构

### 循环结构

1)while

2)do-while

3)for

4)多重循环

### 跳转语句

1)break：作用在 switch 和循环结构中，结束整个 switch 选择结构或者循环结构

2)continue：作用在循环结构中，结束当前循环后续操作，继续执行下一次循环操作

3)return：作用在方法内，结束方法并返回结果

## 数组

一种特殊的数据类型，可以储存多个相同类型的数据

### 组成

数据类型 数组标识符 [下标位]=数据

#### 数组的声明

数据类型 数组标识符[ ]=new 数据类型[数组长度];

#### 数组的使用

数组标识符[索引值]=数据

#### 二维数组

实际上即为一维数组的嵌套，理论上可以存在 N 维数组，声明方法：<br>
数据类型 [ ]数组标识符[ ]=new 数据类型[数组长度][数组长度];

#### 二维数组的使用

数组标识符[索引值][索引值]=数据

#### 二维数组的模型

[[index0],[index1],[index2],[index3]]

#### 数组的遍历

一般可以使用 for 循环或者增强 for 循环，增强 for 循环：

```java
for(数据类型 标识符 of 对象数组){
表达式（此时标识符即代表了数组中的各个元素）
}
```

#### 数组最值的获取

我们默认第一个值为最小值或者最大值，通过将最值与数组中的所有余下数据进行比较从而更新数组
最值的方法来获取最值

#### 数组的排序

我们可以采用冒泡排序的方法：我们由第一位向后不断地进行比较，若后面的数据表当前位的数据大
或者小的时候，我们将两者的位置进行调换，从而不断地保证第一位到倒数第二位可以保持升序或者
降序的顺序进行排列

#### 数组的插值

当我们需要像数组中插值的时候我们需要额外创建一个数组来将原数组置于该新创建的数组之中，而后
再进行插值操作

## 面向对象

Java 将世间万物视为不同的对象，而所有的对象都由属性和方法所组成，这些对象的抽象表达即为类，
而其实际表达即为对象，我们将类转化为对象的过程即为实例化，我们再类中声明属性和方法，而后使
可以用实例化后的对象对于这些属性和方法进行调用。因此类是对象的抽象表达，而实例化后的对象则
为类的具体表达

### 类的声明

修饰符（public/default） class 类的标识符 {

类的属性以及方法

}

#### 属性的声明

访问权限修饰符 数据类型 属性名;

#### 方法的声明

访问权限修饰符 返回值类型 方法名(参数列表){

方法体

}

#### 构造方法

类的构造方法是类中不包含返回值类型且方法名与类名相同的特殊方法，构造方法的参数类表可以包含
任意数量的参数，使用构造方法我们可以实现对于不同形式对象的创建。创建对象的示例:

```java
类名 对象名=new 类的构造方法(参数列表);
```

由于传入参数的不同我们可以实现创建对象为其赋予的属性值的不同，因为我们在构造方法中可以使用
**this**关键字借指当前对象，从而在我们创建对象之际便可完成属性的赋值：

```java
//声明构造方法
public ClassnName(参数一，参数二，参数三...){
this.属性一=参数一；
this.参数二=参数二；
this.参数三=参数三；
...
}
//创建对象
ClassName object=new ClassName(参数一，参数二，参数三...);
```

### 方法的重载

**在同一个类中**我们可以同时声明多个重名的方法，但是**这些方法的参数类表必须保证不可以重复**，两个
重名方法也可以含有完全相同的参数列表，但是必须保证**参数列表的顺序不可重复**。在一个类中最常见的
方法重载便是构造方法

### 访问权限修饰符

**public**:在同一个项目中任意处都可以被访问

**protected**:可以被跨包访问

1).在子类中直接使用父类的 protected 变量是可以的，父类的 protected 权限的成员变量可以被子类继承

2).在子类中通过子类的对象访问父类的 protected 成员是可以的

3).在子类中使用父类的对象访问父类的 protected 成员反而是不行的

4).在子类中使用其他子类的对象访问父类的 protected 成员也是不行的

**default**:不可被跨包访问

**private**:仅能在当前类中被直接使用

## 封装

### 实现：使用 private 修饰属性或者方法

#### 目的：将类中的属性和部分方法隐藏从而保护其不会被随意改变，其中属性被设置为 private 的时候，我们可以生成 getter 和 setter 方法来获取和修改这个属性，而在 setter 中我们可以设置判断条件以是的被赋予的值是合法的

#### 类的继承

任意两个类之间(抽象类除外)都可以通过**extends**关键字来实现继承的关系，使用继承的类即为子类，而被继承的类即为父类，子类可以继承除了构造方法和私有属性及方法外的所有属性及方法
，此外，尽管我们不可以继承父类的构造方法，但是我们可以继承父类的参数列表，当我们继承了一个父类后，其子类可以声明一个与父类参数列表一致的构造方法，同时我们我们也可以为其添加子类的
特有属性，其中继承的参数列表需要使用**super**进行标识：

```java
//父类的构造方法：
public parentClass(父类参数1，父类参数2...){
this.父类属性1=父类参数1;
this.父类属性2=父类参数2;
...
}
//子类的构造方法：
public childClass(父类参数1，父类参数2...子类参数1,子类参数2...){
super(父类参数1，父类参数2...);
this.子类属性1=子类参数1;
this.子类属性2=子类参数2;
...
}
```

### 方法的重写

当我们父类中的方法不足以满足子类需求的时候，我们便可以在子类中将父类中的同名方法进行重写，
继而满足子类的需求，而重写方法需保证，子类中的同名方法标识符私有度不高于父类中的同名方法
同时这两个方法的参数列表也需要完全对应

## 多态

**概念：**同一个事物，使用不同的条件，作用结果不一样，其是基于继承之上的，主要的体现在于方法的重写以及**向上**和**向下转型**

### 抽象类，抽象方法和接口

#### 抽象类

使用 abstract 修饰的类即为抽象类，抽象类可以含有抽象方法也可以含有常规方法，如果一个类想要拥有抽象方法就必须是抽象类。但是抽象
类是无法吧实例化的，此外，如果一个类继承了一个抽象类，那么它必须实现这个抽象父类的所有抽象方法。

#### 抽象方法

使用 abstract 修饰的方法即为抽象方法，抽象方法只可存在于抽象类和接口之中，其本身是不具备方法体的。

#### 接口

接口与类不同，不是 class 而是 interface,且接口无法被实例化，也无法参与实例化。但是可以被实现。一个实现接口的类，必须**实现**(implements)接口内所描述的所有方法，否则就必须声明为抽象类,因此我们使用 extends 使一个类来继承另一个类
使用 implements 来实现我们的接口，初次之外：抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是 **public static final** 类型的 **接口中也不能含有静态代码块以及静态方法**(用 static 修饰的方法)，而抽象类是可以有静态代码块和
静态方法。但是接口相较于抽象类也有其优点：一个类可以实现多个不同的接口但是只能继承一个抽象类。

### static

该关键字意为静态的，使用该关键字修饰的变量和方法可以直接被类名方法和调用。初次之外，使用该关键字修饰的成员变量在该类中是唯一的，即使该类创建了多个对象，该关键字修饰的变量都只会引用同一个变量。

## 异常

所谓异常即为运行是出现的意外情况，而这些意外情况会到找程序的终止，为了避免这些异常导致程序的崩溃我们使用异常的捕捉来为这些异常出现提供一条解决的途径：

异常中我们常用的五个关键字：

**try**:将可能会出现异常的语句置于其代码块中以捕获异常

**catch**:对于捕获到的异常进行处理

**finally**：无论是否捕获到异常都将执行其内部的代码

**throw**:抛出一个异常

**throws**：声明一个异常（在方法参数之后使用，声明可能出现的异常）

## 集合类

集合类是一种数据结构，它可以存储多个元素，并且可以进行一些操作，比如添加、删除、查找、排序等。这里主要由三种常用的接口：List，Set，Map。其中前两者都是继承了 Collection 接口，而 Map 则与 Collection 接口继承了 Map 接口。这三者的区别如下：

List 接口中实现类的内容是有序的，而 Set 接口中实现类的内容是无序的。而且 List 接口中实现类的内容是可以重复的，而 Set 接口中实现类的内容是不可以重复的。

至于 Map 接口，它是一种键值对的集合，它的键值对是无序的，而且键值对中的键不可以重复(值则可重复)。

### List 接口中的实现类

ArrayList:这是一个动态的集合，它可以随时增加或删除元素，而且它的元素是有序的。我们可以通过 add() 方法来添加元素，通过 remove() 方法来删除元素，通过 get() 方法来获取元素，通过 set() 方法来修改元素，通过 indexOf() 方法来获取元素的索引，通过 lastIndexOf() 方法来获取元素的最后一次出现的索引，通过 contains() 方法来判断元素是否存在，通过 size() 方法来获取集合中元素的个数。

LinkedList：这是一个动态的集合，它可以随时增加或删除元素，而且它的元素是有序的。我们可以通过 add() 方法来添加元素，通过 remove() 方法来删除元素，通过 get() 方法来获取元素，通过 set() 方法来修改元素，通过 indexOf() 方法来获取元素的索引，通过 lastIndexOf() 方法来获取元素的最后一次出现的索引，通过 contains() 方法来判断元素是否存在，通过 size() 方法来获取集合中元素的个数。

尽管它们两者方法基本没有差别，但是实际上它们的底层结构却有所不同，ArrayList 可以看作是一个数组，它的每一次元素的添加、删除、获取、修改都是在数组的后面进行的，而 LinkedList 则是一个链表，它的每一次元素的添加、删除、获取、修改都是在链表的后面进行的。我们数组的操作则会导致牵一发而动全身，但是链表则不同，它在操作数据时仅仅只需要变换一部分与之相关的数据而不会影响到全局的结构。因此在大量数据操作上，我们的 LinkedList 是更具优势的，但是在查询上我们的 ArrayList 是略胜一筹的。

#### Set 接口中的实现类

HashSet:这是一个无序的集合，它的元素是不可重复的。我们可以通过 add() 方法来添加元素，通过 remove() 方法来删除元素，通过 get() 方法来获取元素，通过 contains() 方法来判断元素是否存在，通过 size() 方法来获取集合中元素的个数。

TreeSet:
虽说这个集合本身也应该是一个无须的集合，但是事实上如若我们需要使用该集合为其添加元素，我们首先得让所添加的元素实现 Comparable 接口，然后再添加该元素，而一旦实现了 Comparable 接口，这些被添加至 TreeSet 中的元素便会以我们规定的顺序**有序排列**,如：

```java
class News implements Comparable<News>{
        ...
        public int compareTo(News object) {
        if(index>object.index){
            //return 1; Sorted in ascending mode
            return -1;
        }else if(index== object.index){
            return 0;
        }else{
            //return -1; Sorted in ascending mode
            return 1;
        }
    }
}
```

所以事实上我们的 TreeSet 可以看作是一个有序集合，只是它的顺序并非以添加的顺序为准，而是以我们规定的顺序为准。

#### Map 的实现类

我们知道 Map 实际就是以一个键值对的形式存在的，我们的键值在同一个 Map 中是不能重复的，而且键值对的键值是不能为 null 的。但是同一个值可以对应的键确实可以存在多个。其实现类首先是 HashMap，HashMap 是一个无序的集合。当然既然这里有 HashMap,就会有 TreeMap，而这里的 TreeMap 与前者类似，它同样是有序的，但是它并不要求其内部的数据实现 Compareable 接口。它的排序是基于**键的自然排序**。这里HashMap和TreeMap的一则示例：

```java
        Student stu1=new Student("Yang Xin","Boy");
        Student stu2=new Student("Tao Xueting","Girl");
        Student stu3=new Student("Xu Mengcan","Girl");
        Map mp=new HashMap();
        mp.put("Rongxin Yang",stu1);
        mp.put("T-iky79",stu2);
        mp.put("Xucc",stu3);
        mp.put("Griffin Yang",stu1);

        Set<Map.Entry<String,String>>students=mp.entrySet();
        System.out.println();
        System.out.println("Map:");
        for (Map.Entry obj : students) {
           Map.Entry me=obj;
            System.out.println(me.getKey()+":"+me.getValue().toString());
        }

        Map mp1=new TreeMap();
        mp1.put("Rongxin Yang",stu1);
        mp1.put("T-iky79",stu2);
        mp1.put("Xucc",stu3);
        mp1.put("Griffin Yang",stu1);

        Set<Map.Entry<String,String>>students1=mp1.entrySet();
        System.out.println();
        System.out.println("Map1:");
        for (Map.Entry obj : students1) {
            Map.Entry me=obj;
            System.out.println(me.getKey()+":"+me.getValue().toString());
        }
```

```java
//Result:
Map:
Xucc:Xu Mengcan->Girl
Griffin Yang:Yang Xin->Boy
Rongxin Yang:Yang Xin->Boy
T-iky79:Tao Xueting->Girl

Map1:
Griffin Yang:Yang Xin->Boy
Rongxin Yang:Yang Xin->Boy
T-iky79:Tao Xueting->Girl
Xucc:Xu Mengcan->Girl
```

首先我们发现，在Map中我们不再使用add作为添加元素的方法了而是使用put将其替代，其形式为put(key,value)。但是删除元素依旧是remove。由于在Map中只有键是唯一的，所以我们这里要使用键名来实现对于指定对象的删除，同样的，如若我们要查询其中的元素所使用的也是键名。而由于Map集合是键值对这一特点，我们有containsKey和containsValue两个contains方法用以查询是否存在指定的键或值。

### 集合的遍历

#### List的遍历

由于List是一个有序的集合，所以我们可以使用for循环来遍历它，这里的遍历方式是从前到后的遍历。而这里我们不仅可以使用以下标进行的普通for循环，同样也可以使用增强for循环：

```java
//普通for循环
        List<String> list=new ArrayList<String>();
        list.add("Rongxin Yang");
        list.add("T-iky79");
        list.add("Xucc");
        list.add("Griffin Yang");
        for(int i=0;i<list.size();i++){
            System.out.println(list.get(i));
        }
```

```java
//增强for循环
        List<String> list=new ArrayList<String>();
        list.add("Rongxin Yang");
        list.add("T-iky79");
        list.add("Xucc");
        list.add("Griffin Yang");
        for(String str:list){
            System.out.println(str);
        }
```

#### Set的遍历

由于Set并非是一个有序的集合，故而我们不能使用for循环来遍历它，而是使用增强for循环：

```java
//增强for循环
        Set<String> set=new HashSet<String>();
        set.add("Rongxin Yang");
        set.add("T-iky79");
        set.add("Xucc");
        set.add("Griffin Yang");
        for(String str:set){
            System.out.println(str);
        }
```

#### Map的遍历

由于Map是一个键值对的集合，故而我们不能使用for循环来遍历它，而是使用增强for循环：

```java
//增强for循环
        Map mp1=new TreeMap();
        mp1.put("Rongxin Yang",stu1);
        mp1.put("T-iky79",stu2);
        mp1.put("Xucc",stu3);
        mp1.put("Griffin Yang",stu1);

        Set<Map.Entry<String,String>>students1=mp1.entrySet();
        System.out.println();
        System.out.println("Map1:");
        for (Map.Entry obj : students1) {
            Map.Entry me=obj;
            System.out.println(me.getKey()+":"+me.getValue().toString());
        }
```

### 泛型

所谓泛型，就是指在编译时不确定类型的变量，而在运行时可以指定类型的变量。一旦我们在集合声明的时候规定了泛型，那么该集合就只能存储指定类型的数据了。一旦我们添加了一个不属于泛型规定的数据，则编译失败。除此之外一旦我们规定了一个集合的泛型，我们在遍历该集合的时候便不再需要首先使用Object类型将其遍历而后再将object强制转为指定类型了，因为一但规定了泛型，我们的集合之中就不会存在其他类型的数据了：

```java
//声明一个泛型集合
        List<String> list=new ArrayList<String>();
        list.add("Rongxin Yang");
        list.add("T-iky79");
        list.add("Xucc");
        list.add("Griffin Yang");
```

## 枚举类

所谓枚举就是将同一个类型的数据全部列举出来而后单独组成一个类，此时这个类便称作为枚举类。其实际上相当于是一个同拥有多个可能值得单独常量，当我们使用枚举后，只可选择其中中的一个值：

```java
public enum Gender {
    Male,Female,Boy,Girl,Man,Women
}
```

使用：

```java
Person person = new Person("Rongxin Yang",22,Gender.Boy);
//此处我们调用了Gender枚举类，此时它的值只可以是枚举中已存在的 “Male,Female,Boy,Girl,Man,Women”
```

## Java框架篇

### 日期类

### Date(已弃用)

主要构造方法：

Date(int year,int month,int date)，Date(int year,int month,int date,int hrs,int min,int sec)

注意其中的年所☞的是自1900到当前时间点的时间长度，故我们若想得到当前的年份则需要额外加上1900才可以。其中常用的几个方法如下：

getDate()：获取今天是一个月中的第几天

getDay()：获得今天是星期几

getHours()：获得现在是几点

getMinutes()：获得现在是几分

getMonth()：获得当前的月份，注意月份需要加一

getSeconds()：获得当前的秒数

### Calendar(Date 的替代 )

这是一个抽象类，所以我们不可以直接进行实例化，而是使用其静态方法进行对象的创建：

```java
Calendar cal = Calendar.getInstance();
```

 Calendar.get(Calendar.DAY_OF_MONTH): 获取今天是一个月中的第几天

 Calendar.get(Calendar.DAY_OF_WEEK): 获得今天是星期几

 Calendar.get(Calendar.HOUR_OF_DAY): 获得现在是几点

 Calendar.get(Calendar.MINUTE): 获得现在是几分

 Calendar.get(Calendar.MONTH): 获得当前的月份，注意月份需要加一

 Calendar.get(Calendar.SECOND): 获得当前的秒数

#### 直接获得当前具体时间

使用Date类时，我们直接输出Date对象即可得到当前时间，但是此时时间是标准时间，若想将其转换为特定的时间格式我们可以通过引入SimpleDateFormat进行转换：

```java
SimpleDateFormat sdf = new SimpleDateFormat("HH:mm a MM/dd/yyyy");
Date date=new Date();
sdf.format(date);
```

输出值为：

```java
12:00 PM 12/22/2022
```

以下为SimpleDateFormat的格式对应表：

<table class="striped"> <caption style="display:none">Chart shows pattern letters, date/time component, presentation, and examples.</caption> <thead> <tr> <th scope="col" style="text-align:left">Letter </th>
<th scope="col" style="text-align:left">Date or Time Component </th>
<th scope="col" style="text-align:left">Presentation </th>
<th scope="col" style="text-align:left">Examples </th>
</tr>
</thead> <tbody> <tr> <th scope="row">
<code>G</code> </th>
<td>Era designator </td>
<td>
<a href="#text">Text</a> </td>
<td>
<code>AD</code> </td>
</tr>
<tr> <th scope="row">
<code>y</code> </th>
<td>Year </td>
<td>
<a href="#year">Year</a> </td>
<td>
<code>1996</code>; <code>96</code> </td>
</tr>
<tr> <th scope="row">
<code>Y</code> </th>
<td>Week year </td>
<td>
<a href="#year">Year</a> </td>
<td>
<code>2009</code>; <code>09</code> </td>
</tr>
<tr> <th scope="row">
<code>M</code> </th>
<td>Month in year (context sensitive) </td>
<td>
<a href="#month">Month</a> </td>
<td>
<code>July</code>; <code>Jul</code>; <code>07</code> </td>
</tr>
<tr> <th scope="row">
<code>L</code> </th>
<td>Month in year (standalone form) </td>
<td>
<a href="#month">Month</a> </td>
<td>
<code>July</code>; <code>Jul</code>; <code>07</code> </td>
</tr>
<tr> <th scope="row">
<code>w</code> </th>
<td>Week in year </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>27</code> </td>
</tr>
<tr> <th scope="row">
<code>W</code> </th>
<td>Week in month </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>2</code> </td>
</tr>
<tr> <th scope="row">
<code>D</code> </th>
<td>Day in year </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>189</code> </td>
</tr>
<tr> <th scope="row">
<code>d</code> </th>
<td>Day in month </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>10</code> </td>
</tr>
<tr> <th scope="row">
<code>F</code> </th>
<td>Day of week in month </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>2</code> </td>
</tr>
<tr> <th scope="row">
<code>E</code> </th>
<td>Day name in week </td>
<td>
<a href="#text">Text</a> </td>
<td>
<code>Tuesday</code>; <code>Tue</code> </td>
</tr>
<tr> <th scope="row">
<code>u</code> </th>
<td>Day number of week (1 = Monday, ..., 7 = Sunday) </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>1</code> </td>
</tr>
<tr> <th scope="row">
<code>a</code> </th>
<td>Am/pm marker </td>
<td>
<a href="#text">Text</a> </td>
<td>
<code>PM</code> </td>
</tr>
<tr> <th scope="row">
<code>H</code> </th>
<td>Hour in day (0-23) </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>0</code> </td>
</tr>
<tr> <th scope="row">
<code>k</code> </th>
<td>Hour in day (1-24) </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>24</code> </td>
</tr>
<tr> <th scope="row">
<code>K</code> </th>
<td>Hour in am/pm (0-11) </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>0</code> </td>
</tr>
<tr> <th scope="row">
<code>h</code> </th>
<td>Hour in am/pm (1-12) </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>12</code> </td>
</tr>
<tr> <th scope="row">
<code>m</code> </th>
<td>Minute in hour </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>30</code> </td>
</tr>
<tr> <th scope="row">
<code>s</code> </th>
<td>Second in minute </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>55</code> </td>
</tr>
<tr> <th scope="row">
<code>S</code> </th>
<td>Millisecond </td>
<td>
<a href="#number">Number</a> </td>
<td>
<code>978</code> </td>
</tr>
<tr> <th scope="row">
<code>z</code> </th>
<td>Time zone </td>
<td>
<a href="#timezone">General time zone</a> </td>
<td>
<code>Pacific Standard Time</code>; <code>PST</code>; <code>GMT-08:00</code> </td>
</tr>
<tr> <th scope="row">
<code>Z</code> </th>
<td>Time zone </td>
<td>
<a href="#rfc822timezone">RFC 822 time zone</a> </td>
<td>
<code>-0800</code> </td>
</tr>
<tr> <th scope="row">
<code>X</code> </th>
<td>Time zone </td>
<td>
<a href="#iso8601timezone">ISO 8601 time zone</a> </td>
<td>
<code>-08</code>; <code>-0800</code>; <code>-08:00</code> </td>
</tr>
</tbody></table>

当我们使用Calendar时，我们可以使用其getTime()方法来获取当前时间，其返回值也是一个Date类型的数据。

### 文件流

我们的文件流指的是再程序和文件之间的连接，它可以读取文件的内容，也可以向文件写入内容。最原始的是字节流，它是一种按字节读取和写入的流。 我们主要使用的是FileInputStream和FileOutputStream这两个类来读写文件。而它们均为继承自InputStream和OutputStream类，所以我们可以使用它们的方法来读写文件。

#### File 文件对象

这个对象是用以获取文件的，它可被用于创建FileinputStream和FileOutputStream对象等。初次之外它也可以用以获取文件的名称、大小、创建时间、修改时间等。

##### File对象的构造方法

File(String pathname)：构造一个文件对象，其中pathname是文件的路径。

##### File对象的常用方法

createNewFile()：创建一个文件，如果文件已经存在，则返回false。

createTempFile(String prefix,String suffix)：创建一个临时文件，其中prefix是文件的前缀，suffix是文件的后缀。

createTempFile(String prefix,String suffix,File directory)：创建一个临时文件，其中prefix是文件的前缀，suffix是文件的后缀，directory是文件的目录。

delete(): 删除文件。

exists()：判断文件是否存在。

lastModified()：返回文件的最后修改时间。

length()：返回文件的大小。

isHidden()：判断文件是否隐藏。

getAbsoluteFile()：返回文件的绝对路径。

getPath()：返回文件的相对路径。

#### 其中FileInputStream类的常用构造方法是

FileInputStream(File file)：使用指定的文件来构造一个FileInputStream对象。

FileInputStream(String filePath)：使用指定文件的路径直接构造一个FileInputStream对象。

#### FileInputStream类的常用方法是

read()：读取一个字节。

read(byte[] b)：将读取到的内容存放在b数组之中。一次存储的的数据量是b数组的长度。

read(byte[] b,int off,int len)：将读取到的内容存放在b数组之中，其中off是偏移量，len是读取的长度。

skip(long n)：跳过n个字节。

close(): 关闭文件输入流。

#### FileOutputStream类的常用构造方法是

FileOutputStream(File file)：使用指定的文件来构造一个FileOutputStream对象。

FileOutputStream(String filePath)：使用指定文件的路径直接构造一个FileOutputStream对象。

#### FileOutputStream类的常用方法是

write(int b)：写出一个字节。

write(byte[] b)：写出数组b中所有的字节数据至指定的文件之中。

write(byte[] b,int off,int len):从数组b中off开始指定的长度len的字节数据至指定的文件之中。

write(int b)：写出一个指定的字节至指定的文件之中。

close()：关闭文件输出流。

#### InputStreamReader & InputStreamReader

其将字节流转换为字符流,转为字符流的好处是，字符流会使用缓存，而常规的字节流则不会使用缓存，因此如果你需要的是字符串层次上的操作使用字符则会提高程序的效率。然而事实上字符流只是基于字节流之上的，可以理解为字节流的优化。

它的构造方法是：

```java
InputStreamReader(InputStream in,[String charsetName||Charset cs]) //第一个参数是字节流，第二个参数是字符集名称或者字符集对象。
OutputStreamWriter(OutputStream in,[String charsetName||Charset cs]) //第一个参数是字节流，第二个参数是字符集名称或者字符集对象。
```

既然都叫字符流了，当然它便支持将数据写入一个字符数组之中了

```java
read(char[] cbuf,int off,int len)
write(char[] cbuf||String str,int off,int len) 
```

只是可惜的是这个数组长度硬是固定的，所以这也就比较适合字符串的修改，当然我们也可以使用传统的 read() 和 write(int c),除此之外，我们使用了字符流意味着我们必须最后要OutputStreamWriter这个对象进行flush()操作，否则数据不会写入文件。

#### BufferedReader & BufferedWriter

但是毕竟前者这个字符流也还是一个字符一个字符的操作，由此为了更高效率的完成字符的读取操作等，我们还有一个BufferedReader和BufferedWriter类，它们的构造方法是：

```java
BufferedReader(Reader in)
BufferedWriter(Writer out)
```

与前者相比，我们的reader可以使用reandLine()方法一次读取一行的数据并将其存为一个字符串，此时我们再将这个返回的字符串转为字节数组，此时我们就可以使用write(int b)方法将数据写入文件了。此时我们的读写效率将会再次的到提高，当然这依旧只是对于字符串操作有效

#### DataInputStream & DataOutputStream

既然我们之前有提到字符流不可以用以处理二进制文件，那么我们便可以使用DataInputStream和DataOutputStream类来处理二进制文件。它们是专职处理二进制文件的类，它们的构造方法是：

```java
    DataInputStream(InputStream in)
    DataOutputStream(OutputStream out)
```

然而它们也同样有一缺点：它们均为字节流，故而不支持缓存，因此它们的执行效率同样很是低下

由于字符流无法胜任二进制文件的操作，也就是说当我们想要对于除了字符串以外的文件进行操作的时候，字符流是不能够帮我们做到的。虽然我们或许也可以得到结果，但是此时速度很慢不说，得出的结果也是无法使用的。举个例子，当我们使用字符流实现了文件复制粘贴的操作，或许，我们过了很长一段时间可以实现文件的复制粘贴（有可能比字节流还慢），我们复制的这个文件还会被损坏。所以这就得不偿失了。而且如若我们想要使用DataInputStream和DataOutputStream来弥补，效率又过低。那么我们既要再操作这些非字符串操作的文件，又要提高效率该怎么做呢？虽然我们常规的字节流很慢，但我们还有非常规的字节流啊，它可以帮我们解决这个问题。而这非常规的字节流就是BufferedInputStream和BufferedOutputStream。它们是字节流，同时它们也会使用缓存，所以它们兼顾了速度和功能性是可以称得上最好的选择。

#### BufferedInputStream & BufferedOutputStream

```java
BufferedInputStream(InputStream in)
BufferedOutputStream(OutputStream out)
```

BuffferdInputStream和BufferedOutputStream加上了Buffered修饰也就意味着它们正式拥有了使用缓存的能力，因此它们便获得了效率提升。除此之外又因为它们本身实际上还是字节流，所以它们也可以用于字符流的操作，也就是说它们可以处理字符串操作以外的操作。

#### ObjectInputStream & ObjectOutputStream

又是我们发现一些文件的后缀名很是奇怪，我们甚至无法将其打开。这是因为它们是专门用于处理对象的，而对象的序列化是一种把对象转换为字节流的操作，而反序列化是一种把字节流转换为对象的操作。而ObjectInputStream和ObjectOutputStream是专门用于处理对象的序列化和反序列化的类，它们的构造方法是：

```java
    ObjectInputStream(InputStream in)
    ObjectOutputStream(OutputStream out)
```

这两个对象与前面的不同，它们操作对象实际上是对于同一个文件进行操作的，因此首先它们的构造方法中的in和out一定是相同的，除此之外，它们对于对象的读写都是基于方法的，其中writeObject(object)和readObject()便是核心了。它们将一对象存于一个文件之中，而后对于该文件进行序列化和反序列化从而实现对象的储存和取出。但需要注意的是在我们对于一个对象进行序列化和反序列化之前我们需要使其可序列化，即实现Serializable接口。

## 线程

其主要构造方法是：

```java
    Thread(String name)
    Thread(Runnable target)
    Thread(Runnable target, String name)
    Thread(ThreadGroup group, Runnable target)
    Thread(ThreadGroup group, Runnable target, String name)
    Thread(ThreadGroup group, Runnable target, String name, long stackSize)
```

所谓线程即为进程中的一部分，是程序执行中最小的单元。其实现类即为Thread类，这个类实现了Runnable接口由此获得了线程实现这一功能。如果我们希望一个类可以使用多线程的模式进行执行则我们可以选择继承Thread类，并重写run方法即可。注意其中的run方法是一个抽象方法，我们需要重写它以实现我们的线程的功能,如果这个类中又一个功能需要实现多线程运行，则可直接将其写在run方法中。之后我们调用start方法即可启动线程。

除此之外我们也可以通过实现Runnable接口的方式来实现，只是如果我们选择使用实现接口来获得线程执行这一功能。但是我们使用该方式之后则无法使用start方法直接来启动线程。因为该接口中无start方法，而且我们也未将其重写，古不可使。此时我们便需要借助Thread类中的 Thread(Runnable target)和Thread(Runnable target, String name)这两个构造方法进行创建线程。因为Runnable是个接口是不可创建对象的，我们此时只需将我们创建的实现类置于其中即可，此时我们使用Thread线程对象直接调用start方法即可。倘若我们需要多次使用该实现类，则只需创建指定数量的Thread类即可，如：

```java
//ThreadByRunnable.java
public class ThreadByRunnable implements Runnable {

    @Override
    public void run() {
        for (int i=1; i<=20;i++){
            System.out.println(Thread.currentThread().getName()+" "+i+" "+" has started!");
            System.out.println(Thread.currentThread().getName()+" "+i+" "+" has ended!");
        }
    }
}
//ThreadMain.java
public class ThreadMain {
    public static void main(String[] TXT) {
        ThreadByRunnable byRunnable = new ThreadByRunnable();
        Thread threadAlpha=new Thread(byRunnable,"Alpha");
        Thread threadBeta=new Thread(byRunnable,"Beta");
        threadAlpha.start();
        threadBeta.start();
    }
}
```
