## 注解

**注解**（Annotation）

作用：

* 不是程序本身，可以对程序作出解释（这一点和注释（comment）没什么区别）
* 可以被其他程序（比如编译器等）获取

格式：

* 注解是以“@注释名”在代码中存在的，还可以添加一些参数值，例如：@SuppressWarnings(value="unchecked")

使用方式：

* 可以附加在 package，class，method，field 等上面，相当于给他们添加了额外的辅助信息,我们可以通过反射机制编程实现对这些元数据的访问



### 内置注解

**@Override**：定义在 java.lang.Override 中，此注释只适用于修辞方法，表示一个方法声明打算重写超类中的另一个方法声明

**@Deprecated**：定义在 java.lang.Deprecated 中，此注释可以用于修辞方法，属性，类，表示不鼓励程序员使用这样的元素，通常是因为它很危险或者存在更好的选择

**@SuppressWarnings**：定义在 java.lang.SuppressWarnings 中，用来抑制编译时的警告信息。与前两个注释有所不同，你需要添加一个参数才能正确使用，这些参数都是已经定义好了的

* 我们选择性的使用就好了
  @SuppressWarnings("all")
  @SuppressWarnings("unchecked")
  @SuppressWarnings(value={"unchecked","deprecation"})
  等等

```java
public class Test01 {
    // 重写注解
    @Override
    public String toString() {
        return super.toString();
    }
    
    // 正压注解
    @SuppressWarnings("all")
    public void test01(){
        List list = new ArrayList();
    }
}
```



### 元注解

元注解的作用就是负责注解其他注解，Java 定义了4个标准的 meta-annotation 类型，他们被用来提供对其他 annotation 类型作说明。
这些类型和它们所支持的类在 java.ang.annotation 包中可以找到（@Target、@Retention、@Documented 、@inherited ）

* @Target：用于描述注解的使用范围(即:被描述的注解可以用在什么地方)
  @Retention：表示需要在什么级别保存该注释信息，用于描述注解的生命周期 > （SOURCE < CLASS < RUNTIME）
  @Document：说明该注解将被包含在javadoc中
  @Inherited：说明子类可以继承父类中的该注解



### 自定义注解

使用@interface自定义注解时，自动继承了 java.lang.annotation.Annotation 接口
分析:

* interface 用来声明一个注解，格式：public @interface注解名{定义内容}
* 其中的每一个方法实际上是声明了一个配置参数
* 方法的名称就是参数的名称
* 返回值类型就是参数的类型（返回值只能是基本类型，Class，String，enum）
* 可以通过default来声明参数的默认值
* 如果只有一个参数成员，一般参数名为value
* 注解元素必须要有值，我们定义注解元素时，经常使用空字符串，0作为默认值



## 反射

**反射**（Reflection）

**动态语言** 是一类在运行时可以改变其结构的语言:例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。通俗点说就是在运行时代码可以根据某些条件改变自身结构。

主要动态语言:Object-C、C#、JavaScript、PHP、Python等。

**静态语言** 与动态语言相对应的，运行时结构不可变的语言就是静态语言。如Java、C、C++。

Java不是动态语言，但Java可以称之为“准动态语言”。即Java有一定的动态性，我们可以利用反射机制获得类似动态语言的特性。Java的动态性让编程的时候更加灵活!



**Reflection**（反射）是Java被视为动态语言的关键，反射机制允许程序在执行期借助于 Reflection API 取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。
`Class c = Class.forName("java.lang.String")`
加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象的称之为：反射

<img src="C:\Users\xtt\AppData\Roaming\Typora\typora-user-images\image-20240928120334582.png" alt="image-20240928120334582" style="zoom:50%;" />



### 获得反射对象

Java反射机制提供的功能：

* 在运行时判断任意一个对象所属的类

* 在运行时构造任意一个类的对象

* 在运行时判断任意一个类所具有的成员变量和方法

* 在运行时获取泛型信息

* 在运行时调用任意一个对象的成员变量和方法

* 在运行时处理注解

* 生成动态代理

* 等等



反射的优缺点：

优点：可以实现动态创建对象和编译，体现出很大的灵活性

缺点：对性能有影响。使用反射基本上是一种解释操作，我们可以告诉JVM，我们希望做什么并且它满足我们的要求。这类操作总是慢于

直接执行相同的操作。



```java
package com.dantalian.reflection;

// 反射
public class Test01 {
    public static void main(String[] args) throws ClassNotFoundException {
        // 通过反射获取类的 Class 对象
        Class<?> c1 = Class.forName("com.dantalian.reflection.User");
        System.out.println(c1);

        Class<?> c2 = Class.forName("com.dantalian.reflection.User");
        Class<?> c3 = Class.forName("com.dantalian.reflection.User");
        Class<?> c4 = Class.forName("com.dantalian.reflection.User");

        // 一个类在内存中只有一个 Class 对象
        // 一个类被加载后，类的整个结构都会被封装在 Class 对象中
        System.out.println(c2.hashCode());
        System.out.println(c3.hashCode());
        System.out.println(c4.hashCode());
    }
}

// 实体类
class User{
    private String name;
    private int id;
    private int age;
    
    public User() {
    }

    public User(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id=" + id +
                ", age=" + age +
                '}';
    }
}
```



### 得到 Class 对象的几种方式

**Class 类**，对象照镜子后可以得到的信息:某个类的属性、方法和构造器、某个类到底实现了哪些接口。对于每个类而言，JRE都为其保留一个不变的Class类型的对象。一个Class对象包含了特定某个结构（classlinterface/enum/annotation/primitive type/void/[]）的有关信息。

* Class本身也是一个类
* Class对象只能由系统建立对象
* 一个加载的类在JVM中只会有一个Class实例
* 一个Class对象对应的是一个加载到JVM中的一个.class文件
* 每个类的实例都会记得自己是由哪个Class 实例所生成
* 通过Class可以完整地得到一个类中的所有被加载的结构
* Class类是Reflection的根源，针对任何你想动态加载、运行的类，唯有先获得相应的Class对象



获取 Class 类的实例

* 若已知具体的类，通过类的class属性获取，该方法最为安全可靠，程序性能最高。

​		`Class clazz= Person.class;`

* 已知某个类的实例，调用该实例的getClass()方法获取Class对象

​		`Class clazz = person.getClass();`

* 已知一个类的全类名，且该类在类路径下，可通过Class类的静态方法forName()获取，可能抛出ClassNotFoundException

​		`Class clazz= Class.forName("demo01.Student");`

* 内置基本数据类型可以直接用类名.Type

* 还可以利用ClassLoader我们之后讲解

```java
// 测试 Class类的创建方式
public class Test02 {

    public static void main(String[] args) throws ClassNotFoundException {
        Person person = new Student();
        System.out.println("这个人是：" + person.name);

        // 1、通过对象
        Class c1 = person.getClass();
        System.out.println(c1.hashCode());

        // 2、通过 forName
        Class c2 = Class.forName("com.dantalian.reflection.Student");
        System.out.println(c2.hashCode());

        // 3、通过 类名.class
        Class c3 = Student.class;
        System.out.println(c3.hashCode());

        // 4、基本内置类型的包装类都有一个 Type属性
        Class c4 = Integer.TYPE;
        System.out.println(c4);

        // 获得父类类型
        Class c5 = c1.getSuperclass();
        System.out.println(c5);
    }
}

class Person{
    public String name;

    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Student extends Person{
    public Student() {
        this.name = "学生";
    }
}
class Teacher extends Person{
    public Teacher() {
        this.name = "老师";
    }
}
```



### 所有类型的 Class 对象

哪些类型可以有Class对象?

* class:外部类，成员（成员内部类，静态内部类），局部内部类，匿名内部类

* interface:接口

* []:数组

* enum:枚举
* annotation:注解@interface
* primitive type:基本数据类型void

```java
// 所有类型的 Class
public class Test03 {
    public static void main(String[] args) {
        Class c1 = Object.class;    // 类
        Class c2 = Comparable.class;    // 接口
        Class c3 = String[].class;  // 一维数组
        Class c4 = int[][].class;   // 二维数组
        Class c5 = Override.class;  // 注解
        Class c6 = ElementType.class;   // 枚举
        Class c7 = Integer.class;   // 基本数据类型
        Class c8 = void.class;  // void
        Class c9 = Class.class; // Class

        System.out.println(c1);
        System.out.println(c2);
        System.out.println(c3);
        System.out.println(c4);
        System.out.println(c5);
        System.out.println(c6);
        System.out.println(c7);
        System.out.println(c8);
        System.out.println(c9);

        // 只要元素类型与维度一样，就是同一个 Class
        int[] a = new int[10];
        int[] b = new int[100];
        System.out.println(a.getClass().hashCode());
        System.out.println(b.getClass().hashCode());
    }
}
```



### 类加载内存分析

