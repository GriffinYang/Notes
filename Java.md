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

### 运算符

#### 表达式的定义：数据与运算符的组合

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

### 数组

#### 概念

一种特殊的数据类型，可以储存多个相同类型的数据

#### 组成

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

#### 类的声明

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

### 封装

#### 实现：使用 private 修饰属性或者方法

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

### 多态

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

### 异常

所谓异常即为运行是出现的意外情况，而这些意外情况会到找程序的终止，为了避免这些异常导致程序的崩溃我们使用异常的捕捉来为这些异常出现提供一条解决的途径：

异常中我们常用的五个关键字：

**try**:将可能会出现异常的语句置于其代码块中以捕获异常

**catch**:对于捕获到的异常进行处理

**finally**：无论是否捕获到异常都将执行其内部的代码

**throw**:抛出一个异常

**throws**：声明一个异常（在方法参数之后使用，声明可能出现的异常）

### 集合类

集合类是一种数据结构，它可以存储多个元素，并且可以进行一些操作，比如添加、删除、查找、排序等。这里主要由三种常用的接口：List，Set，Map。其中前两者都是继承了 Collection 接口，而 Map 则与 Collection 接口继承了 Map 接口。这三者的区别如下：

List 接口中实现类的内容是有序的，而 Set 接口中实现类的内容是无序的。而且 List 接口中实现类的内容是可以重复的，而 Set 接口中实现类的内容是不可以重复的。

至于 Map 接口，它是一种键值对的集合，它的键值对是无序的，而且键值对中的键不可以重复(值则可重复)。

#### List 接口中的实现类

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
