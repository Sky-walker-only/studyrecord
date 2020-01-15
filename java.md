# java基础



类中可以包含的变量：

` 局部变量`  ：在方法、构造方法、语句块中定义的变量称之为变量

` 成员变量`   ：成员变量定义在类中，方法体之外，这种该变量在创建对象的时候实例化

` 类变量`  ：类变量也是定义在类中，但是必须声明为static类型！





### 类

作为Java中创建对象的模板：

```java

public class Dog{
  String breed;
  int age;
  String color;
  void barking(){
  }
 
  void hungry(){
  }
 
  void sleeping(){
  }
}

```





### 对象

是根据类创建的。在Java中，使用关键字new来创建一个新的对象。创建对象需要以下三步：

- ​		**声明**：声明一个对象，包括对象名称和对象类型。
	 ​		**实例化**：使用关键字new来创建一个对象。
	 ​		**初始化**：使用new创建对象时，会调用构造方法初始化对象。

```java

public class Puppy{
   public Puppy(String name){
      //这个构造器仅有一个参数：name
      System.out.println("小狗的名字是 : " + name ); 
   }
   public static void main(String[] args){
      // 下面的语句将创建一个Puppy对象
      Puppy myPuppy = new Puppy( "tommy" );
   }
}

```

### 源文件的声明规则

- ​	一个源文件中只能有一个public类
	 ​	一个源文件可以有多个非public类
	 ​	源文件的名称应该和public类的类名保持一致。例如：源文件中public类的类名是   Employee，那么源文件应该命名为Employee.java。
	 ​	如果一个类定义在某个包中，那么package语句应该在源文件的首行。
	 ​	如果源文件包含import语句，那么应该放在package语句和类定义之间。如果没有package语句，那么import语句应该在源文件中最前面。
	 ​	import语句和package语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。

类有若干种访问级别，并且类也分不同的类型：抽象类和final类等。

## 访问控制修饰符（保护类、变量、方法、构造方法）

Java中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。Java 支持 4 种不同的访问权限。

- **default** (即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。
- **private** : 在同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类） 接口**
- **public** : 对所有类可见。使用对象：类、接口、变量、方法
- **protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。