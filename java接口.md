# 接口的定义

1. 接口使用interface定义

2. Java中，接口和类是并列的两个结构

3. 定义接口中成员在JDK不同版本中有所不同

   1. JDK7及以前：只能定义全局常量和抽象方法

      * 全局常量：`public` `static` `final`，在接口中可以省略关键字
      * 抽象方法：`public` `abstract`，在接口中可以省略关键字

      ```java
      interface Flyable{
          public static final int MAX_SPEED = 7900;//全局常量
          int MIX_SPEED = 1;
          public abstract void Fly();
          void Stop();
      }
      ```

   2. JDK8：除了定义全局常量和抽象方法之外，还可以定义静态方法、默认方法

      ```java
      interface Compare{
          ...
          public static void method1(){
              ...
          }
          public default void method2(){
              ...
          }
          static void method3() {
          	...
          }
          default void method4() {
          	...
          }
      }
      ```

      * 接口中定义的静态方法只能通过接口使用(接口名.方法名)

      * 通过此接口实现类的对象，可以调用接口中的默认方法

      * 实现类可以重写接口中的默认方法

      * 如果子类(实现类)继承的父类和实现的接口中声明了同名同参数的方法(接口中抽象方法或默认方法)并且子类(实现类)没有重写，那么调用时，调用的是父类的方法(类优先原则)

      * 如果实现类实现了多个接口，而这多个接口中定义了同名同参数的默认方法(或一个抽象方法，一个默认方法)，那么实现类没有重写此方法的情况下，会报错；这就需要我们必须在实现类中重写此方法(接口冲突)

      * 在实现类中调用接口的默认方法

        ```java
        public class T implements Compare{
            public void Test(){
                Compare.super.method1();
                Compare.super.method4();
            }
        }
        ```

        

4. 接口中不能定义构造器，意味着接口不能进行实例化(创建对象)

# 接口的使用

1. 接口通过让类去实现(`implements`)的方式来使用

2. 如果实现类覆盖了接口中的所有抽象方法，则此实现类就可以实例化(创建对象)；如果实现类没有覆盖接口中的所有抽象方法，则此实现类仍然为一个抽象类

   ```java
   //实现所有抽象方法
   public class Plane implements Flyable {
   	@Override
   	public void Fly() {
   		...	
   	}
   	
   	@Override
   	public void Stop() {
   		...	
   	}
   }
   //不实现所有抽象方法
   abstract class Kite implements Flyable{
   	@Override
   	public void Fly() {
   		...
   	}
   }
   ```

3. 类与接口的多实现

   ```java
   public class T implements Flyable,...{
       //实现抽象方法
       ...
   }
   ```

4. 接口与接口的多继承

   ```java
   interface BB{
       void method1();
   }
   interface CC{
       void method2();
   }
   interface AA extends BB,CC{
       //定义自己的全局常量和抽象方法
       ...
   }
   ```

5. 接口的具体使用，体现多态性
6. 接口可以看做一种规范