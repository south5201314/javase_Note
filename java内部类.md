[TOC]

# 内部类的分类

## 成员内部类

### 作为外部类的成员

1. 调用外部类的结构
2. 可以被`static`修饰
3. 可以被`public`、`private`、`protected`和不写修饰

### 作为一个类

1. 可以定义属性、方法、构造器等
2. 可以被`final`和`abstract`修饰

## 局部内部类

定义在方法体、代码块、构造器中

```java
public class T {
    public void Test(){
        class T1{
            ...
        }
    }
}
```

# 内部类的使用

1. 成员内部类进行实例化

   ```java
   public class T {
   	private void mian() {
   		//创建非静态内部类对象
   		Pesion.Dog dog = new Pesion().new Dog();
   		//创建静态内部类对象
   		Pesion.Bird bird = new Pesion.Bird();
   	}
   }
   class Pesion{
   	//定义非静态内部类
   	class Dog{
   		...
   	}
   	//定义静态内部类
   	static class Bird{
   		...
   	}
   }
   ```

2. 内部类调用外部类结构

   ```java
   class Pesion {
   	int id;
   
   	public void show(int id) {
   		System.out.println(id);// 方法的形参
   		System.out.println(this.id);// 属性
   		/**
   		 * 外部类不能调用内部的非静态结构，因为外部类对象创建加载的时候内部类没有创建加载
   		 **/
   		// System.out.println(Dog.this.id);
   	}
   	// 定义非静态内部类
   	class Dog {
   		int id;
   		public void show(int id) {
   			System.out.println(id);// 方法的形参
   			System.out.println(this.id);// 内部类的属性
   			System.out.println(Pesion.this.id);// 外部类的属性
   		}
   	}
   }
   ```

3. 局部内部类的使用

   ```java
   class T {
   	int id;
       //定义一个方法来实现一个接口并返回
   	public Comparable getComparable() {
   		class MyComparable implements Comparable{
   			@Override
   			public int compareTo(Object o) {
   				return 0;
   			}
   		}
   		return new MyComparable();
   	}
   }
   ```

   

