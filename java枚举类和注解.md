# 枚举类

## 简介

* 类的对象的数量是有限的，确定的。
* 当需要定义一组常量时，强烈建议使用枚举类。

## 枚举类的使用

### 枚举类的实现

* 若枚举只有一个对象, 则可以作为一种单例模式的实现方式。
* 枚举类的属性
  * 枚举类对象的属性不应允许被改动, 所以应该使用 private final 修饰。
  * 枚举类的使用 `private final` 修饰的属性应该在构造器中为其赋值。
  * 若枚举类显式的定义了带参数的构造器, 则在列出枚举值时也必须对应的传入参数。

* JDK1.5之前需要自定义枚举类

  ```java
  public class Hero {
      private Hero(String name, int id) {
          this.id = id;
          this.name = name;
      }
  
      private String name;
      private int id;
      public static final Hero ASHE = new Hero("寒冰射手", 1);
      public static final Hero KAYLE = new Hero("审判天使", 2);
      public static final Hero JAX = new Hero("武器大师", 3);
  
      @Override
      public String toString() {
          return "英雄名称:" + name + '\'' +
                  ",英雄id:" + id ;
      }
  }
  ```

* JDK 1.5 新增的 enum 关键字用于定义枚举类

  * 使用 enum 定义的枚举类默认继承了 java.lang.Enum类，因此不能再 继承其他类。
  * 枚举类的构造器只能使用private(可省略不写，默认就是private)权限修饰符。
  * 必须在枚举类的第一行声明枚举类对象
  * 枚举类的所有实例必须在枚举类中显式列出(, 分隔 ; 结尾)。列出的实例系统会自动添加`public static final` 修饰。
  * 可以在 `switch` 表达式中使用Enum定义的枚举类的对象作为表达式, case 子句可以直接使用枚举值的名字, 无需添加枚举类作为限定。

  ```java
  public enum HeroEnum {
      //省略了public static final new等关键字
      ASHE("寒冰射手", 1),
      KAYLE("审判天使", 2),
      JAX("武器大师", 3);
      private final String name;
      private final int id;
  
      HeroEnum(String name, int id) {
          this.name = name;
          this.id = id;
      }
  }
  ```

* 和普通 Java 类一样，枚举类可以实现一个或多个接口
* 若每个枚举值在调用实现的接口方法呈现相同的行为方式，则只要统一实现该方法即可。
* 若需要每个枚举值在调用实现的接口方法呈现出不同的行为方式，则可以让每个枚举值分别来实现该方法。

### Enum类的主要方法

* `public <T extends Enum<T>>[] values()`:返回枚举类型的对象数组。该方法可以很方便地遍历所有的枚举值。
* `public <T extends Enum<T>> T valueOf(String name)`：可以把一个字符串转为对应的枚举类对象。要求字符串必须是枚举类对象的“名字”。如不是，会有运行时异常：`lllegalArgumentException`。

* `public static <T extends Enum<T>> T valueOf(Class<T> enumType,String name)`：枚举类型的Class对象和枚举常量名作为参数传入，会得到与参数匹配的枚举常量。
* `public String toString()` ：得到当前枚举常量的名称。根据实际情况进行重写。
* `public final boolean equals(Object other)`：在枚举类型中可以直接使用“==”来进行两个枚举常量是否相等。Enum提供的这个equals()方法，也是使用“==”实现的。它的存在是为了杂Set、List和Map中使用。
* `public final int hashCode()`：实现此方法是为与`equals()`保存一致。
* `public final Class<E> getDeclaringClass()`：得到枚举类常量所属枚举类的Class对象。可以通过Class对象来判断两个枚举常量是否属于同一个枚举类。
* `public final String name()`：得到当前枚举常量的名称。建议优先使用`toString()`方法。
* `public final int ordinal()：得到当前枚举常量的次序。`
* `public final int compareTo(E o)`：比较两个枚举常量的大小(根据声明的顺序排列)。
* `protected final Object clone() throws CloneNotSupportedException`：枚举类型不能被克隆，防止子类实现此方法，Enum实现一个仅抛出`CloneNotSupportedException`异常的不变`Clone()`

# Annotation注解

##  注解(Annotation)概述

* 从 JDK 5.0 开始, Java 增加了对元数据(MetaData) 的支持, 也就是 Annotation(注解)。
* Annotation 其实就是代码里的特殊标记, 这些标记可以在编译, 类加载, 运行时被读取, 并执行相应的处理。
* Annotation 可以像修饰符一样被使用, 可用于修饰包、类、构造器、方法、成员变量、参数、局部变量的声明，这些信息被保存在 Annotation 的 “name=value” 对中。
* 框架 = 注解 + 反射 + 设计模式。

##  常见的Annotation

### 生成文档相关的注解

* `@author`：标明开发该类模块的作者，多个作者之间使用,分割。
* `@version`：标明该类模块的版本 。
* `@see`：参考转向，也就是相关主题。
* `@since`：从哪个版本开始增加的 。
* `@param 形参名 形参类型 形参说明` 对方法中某参数的说明，如果没有参数就不写
* `@return 返回值类型 返回值说明`：对方法返回值的说明，如果方法的返回值类型是void就不能写。
* `@exception异常类型 异常说明` 对方法可能抛出的异常进行说明 ，如果方法没有throws显式抛出的异常就不能写。

### 在编译时进行格式检查(JDK内置的三个基本注解)

* `@Override`: 限定重写父类方法, 该注解只能用于方法。
* `@Deprecated`: 用于表示所修饰的元素(类, 方法等)已过时。通常是因为所修饰的结构危险或存在更好的选择。
* `@SuppressWarnings`: 抑制编译器警告。

##  自定义Annotation

* 使用 `@interface` 关键字来定义会自动继承了`java.lang.annotation.Annotation`接口。
* `Annotation` 的成员变量在 `Annotation` 定义中以无参数方法的形式来声明。其方法名和返回值定义了该成员的名字和类型。我们称为配置参数。类型只能是八种基本数据类型、`String`类型、`Class`类型、`enum`类型、`Annotation`类型、以上所有类型的数组。
* 可以在定义 `Annotation` 的成员变量时为其指定初始值, 指定成员变量的初始值可使用 `default` 关键字。
* 如果只有一个参数成员，建议使用参数名为value。
* 如果定义的注解含有配置参数，那么使用时必须指定参数值，除非它有默认值。格式是“参数名 = 参数值”，如果只有一个参数成员，且名称为value，可以省略“value=”。
* 没有成员定义的 `Annotation` 称为标记; 包含成员变量的 `Annotation` 称为元数据 `Annotation`。
* 注意：自定义注解必须配上注解的信息处理流程才有意义。

```java
MyAnnotation.java

public @interface MyAnnotation {
    String[] value() default {"00","99"};
    String[] value1();
}
```

```java
AnnotationT1.java
@MyAnnotation(value = {"11","22"},value1 = {"tt","ss"})
public class AnnotationT1{
    @MyAnnotation(value1 = {"tt","ss"})
    public void show(){
        ...
    }
}
```

## JDK中的元注解

* `@Retention`：只能用于修饰一个 `Annotation` 定义, 用于指定该 `Annotation` 的生命周期，参数需要指定`RetentionPolicy`枚举值。

  ```java
  public enum RetentionPolicy{
  SOURCE,//在源文件中有效（即源文件保留）
  CLASS,//在class文件中有效（即class保留）
  RUNTIME//在运行时有效（即运行时保留），程序可以通过反射获取该注释
  }
  ```

  ```java
@Retention(RetentionPolicy.RUNTIME)
  public @interface MyAnnotation {
      String[] value() default {"00","99"};
      String[] value1();
  }
  ```

* `@Target`: 用于修饰 `Annotation` 定义,被修饰的 `Annotation` 能用于修饰哪些程序元素。 参数为ElementType枚举类数组。
  
  ```java
  public enum ElementType {
      TYPE, //类、接口(包括注解类型)和enum声明
      FIELD, //域
      METHOD, //方法
      PARAMETER, //参数
      CONSTRUCTOR, //构造器
      LOCAL_VARIABLE, //局部变量
      PACKAGE, //包
      //1.8增加的新特性
      TYPE_PARAMETER,//表示该注解能写在类型变量的声明语句中（如：泛型声明）。
      TYPE_USE //表示该注解能写在使用类型的任何语句中。
  }
  ```
  
  ```java
  @Target({ElementType.TYPE,ElementType.CONSTRUCTOR,ElementType.METHOD})
  @Retention(RetentionPolicy.RUNTIME)
  public @interface MyAnnotation {
      String[] value() default {"00","99"};
      String[] value1();
  }
  ```


* `@Documented`： 用于指定被该元 `Annotation` 修饰的 `Annotation` 类将被
  javadoc 工具提取成文档。默认情况下，javadoc是不包括注解的。
  
* 定义为`Documented`的注解必须设置`Retention`值为`RUNTIME`。
  
* `@Inherited`：被它修饰的 `Annotation` 将具有继承性。如果某个类使用了`@Inherited` 修饰的 `Annotation`, 则其子类将自动具有该注解。

  ```java
  MyAnnotation.java
      
  @Inherited
  @Documented
  @Target({ElementType.TYPE, ElementType.CONSTRUCTOR,ElementType.METHOD})
  @Retention(RetentionPolicy.RUNTIME)
  public @interface MyAnnotation {
      String[] value() default {"00","99"};
      String[] value1();
  }
  ```

  ```java
  AnnotationTest.java
      
  public class AnnotationTest {
      public static void main(String[] args) {
          Annotation[] annotations = AnnotationT2.class.getAnnotations();
          for (int i=0;i<annotations.length;i++){
              System.out.println(annotations[i]);
              //@com.sunflower.java.MyAnnotation(value=[11, 22],value1=[tt, ss])
          }
      }
  }
  @MyAnnotation(value = {"11","22"},value1 = {"tt","ss"})
  class AnnotationT1{
  }
  
  class AnnotationT2 extends AnnotationT1{
  }
  ```

## 利用反射获取注解信息（在反射部分涉及）



## JDK 8中注解的新特性

### 重复注解

* 定义一个`Annotation`，成员变量value的类型为`Annotation[]`数组(需要重复注解的`Annotation`的数组)。
* 在需要重复注解的`Annotation`声明处使用`@Repeatable(Class<? extends Annotation> value())`来修饰。

* 元注解需要保持一致，否则将会报错。

```java
MyAnnotation.java
    
@Repeatable(MyAnnotations.class)
@Inherited
@Documented
@Target({ElementType.TYPE, ElementType.CONSTRUCTOR,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String[] value() default {"00","99"};
    String[] value1();
}

@Inherited
@Documented
@Target({ElementType.TYPE, ElementType.CONSTRUCTOR,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotations{
    MyAnnotation[] value();
}
```

```java
AnnotationT1.java
    
@MyAnnotation(value = {"11","22"},value1 = {"tt","ss"})
@MyAnnotation(value = {"55","66"},value1 = {"qq","cc"})
public class AnnotationT1{
}
```

### 类型注解

* java8在`@Target`的参数`ElementType`中新增加了两个枚举值

```java
public enum ElementType {
    TYPE, //类、接口(包括注解类型)和enum声明
    FIELD, //域
    METHOD, //方法
    PARAMETER, //参数
    CONSTRUCTOR, //构造器
    LOCAL_VARIABLE, //局部变量
    PACKAGE, //包
    //1.8增加的新特性
    TYPE_PARAMETER,//表示该注解能写在类型变量的声明语句中（如：泛型声明）。
    TYPE_USE //表示该注解能写在使用类型的任何语句中。
}
```

