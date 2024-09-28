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







