[TOC]

# final关键字作用

## 修饰类

此类不能被其他类继承，例如：`String`、`System`等类

```java
public final class FinalClass{
    ...
}
```

## 修饰方法

此方法不能被重写，例如：`Object`类中的`getClass()`方法

```java
public class Object {
    ...
    public final native Class<?> getClass();
    ...
}
```

## 修饰变量

### 修饰属性

1. 此时的“变量”就称为常量

2. 必须初始化，而且只能初始化一次

3. final修饰属性的位置

   1. 显示初始化

      ```java
      static final int id = 10;
      ```

   2. 代码块中初始化

      ```java
      public  class T {
      	final int id;
      	{
      		id = 10;
      	}
          ...
      }
      ```

   3. 构造器中初始化(非静态属性)

      ```java
      public class T {
      	final int id;
      	public T(int id) {
      		this.id = id;
      	}
      }
      ```

### 修饰局部变量

1. 修饰形参

   1. 表明此形参是一个常量

   2. 调用方法时，给常量形参赋值(传入实参)后，只能在方法体中使用形参，不能再次赋值

   3. `static` `final` 修饰属性：全局常量

      ```java
      public  class T {
          public static final int len = 100;
          ...
      	public void Test() {
      		final int NUM = 10;
      		//NUM = 20; //初始化后不能再次赋值
      	}
      	public void Test(final int num) {
      		//num = 100; //只能使用，不能修改
      	}
          ...
      }
      ```