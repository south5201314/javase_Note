[TOC]

# 反射机制概述

* Reflection（反射）是被视为动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。 
* 加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象（一个 类只有一个Class对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象的称之为：反射。

## Java反射机制提供的功能 

* 在运行时判断任意一个对象所属的类。
* 在运行时构造任意一个类的对象。
* 在运行时判断任意一个类所具有的成员变量和方法。
* 在运行时获取泛型信息。
* 在运行时调用任意一个对象的成员变量和方法。
* 在运行时处理注解。
* 生成动态代理。

# 反射相关的主要API

*  `java.lang.Class`：代表一个类。
* `java.lang.reflect.Method`：代表类的方法。
* `java.lang.reflect.Field`：代表类的成员变量。
* `java.lang.reflect.Constructor`：代表类的构造器。

## Class类的常用方法

* `static Class forName(String name)` ：返回指定类名 `name` 的 `Class` 对象 。
* `Object newInstance()` ：调用缺省构造函数，返回该`Class`对象的一个实例 。
* `String getName()` ：返回此`Class`对象所表示的实体（类、接口、数组类、基本类型 或void）名称 。
* `Class [] getInterfaces()`： 获取当前`Class`对象的接口 。
* `ClassLoader getClassLoader()`： 返回该类的类加载器 。
* `Class getSuperclass()` ：返回表示此`Class`所表示的实体的超类的`Class`。 
* `Constructor[] getConstructors()` 返回一个包含某些`Constructor`对象的数组 。
* `Field[] getDeclaredFields()` ：返回`Field`对象的一个数组。
* `Method getMethod(String name,Class … paramTypes)` ：返回一个`Method`对象，此对象的形参类型为`paramType`。

# 获取Class实例

## 获取Class类实例的方法

* 若已知具体的类，通过类的`class`属性获取，该方法最为安全可靠， 程序性能最高

  ```java
  Class<Person> aClass2 = Person.class;
  ```

* 已知某个类的实例，调用该实例的`getClass()`方法获取`Class`对象

  ```java
  Person person = new Person();
  Class aClass1 = person.getClass();
  ```

* 已知一个类的全类名，且该类在类路径下，可通过Class类的静态方 法`forName()`获取，可能抛出`ClassNotFoundException`

  ```java
  Class aClass3 = Class.forName("com.sunflower.reflection.Person");
  ```

* 通过类加载器加载类并得到此类的Class类的实例

  ```java
  Class aClass4 = classLoader.loadClass("com.sunflower.reflection.Person");
  ```

## 哪些类型可以有Class对象？

* `class`： 外部类，成员(成员内部类，静态内部类)，局部内部类，匿名内部类
* `interface`：接口
* `[]`：数组
* `enum`：枚举
* `annotation`：注解@interface
* `primitive type`：基本数据类型
* `void`

# 类加载与ClassLoader的理解

* 加载：将`class`文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时 数据结构，然后生成一个代表这个类的`java.lang.Class`对象，作为方法区中类数据的访问 入口（即引用地址）。所有需要访问和使用类数据只能通过这个`Class`对象。这个加载的 过程需要类加载器参与。

* 链接：将Java类的二进制代码合并到JVM的运行状态之中的过程。
  * 验证：确保加载的类信息符合JVM规范，例如：以`cafe`开头，没有安全方面的问题。
  * 准备：正式为类变量（`static`）分配内存并设置类变量默认初始值的阶段，这些内存 都将在方法区中进行分配。
  * 解析：虚拟机常量池内的符号引用（常量名）替换为直接引用（地址）的过程。
  
* 初始化：
  * 执行类构造器`<clinit>()`方法的过程。类构造器()方法是由编译期自动收集类中所有类变量的赋值动作和静态代码块中的语句合并产生的。（类构造器是构造类信息的，不是构造该类对象的构造器）。
  * 当初始化一个类的时候，如果发现其父类还没有进行初始化，则需要先触发其父类初始化。 
  * 虚拟机会保证一个类的`<clinit>()`方法在多线程环境中被正确加锁和同步。
  * 什么时候会发生类初始化？
    * 类的主动引用（一定会发生类的初始化）
      * 当虚拟机启动，先初始化main方法所在的类。
      * new一个类的对象。
      * 调用类的静态成员（除了final常量）和静态方法。
      * 使用java.lang.reflect包的方法对类进行反射调用。
      * 当初始化一个类，如果其父类没有被初始化，则先会初始化它的父类。
    * 类的被动引用（不会发生类的初始化）
      * 当访问一个静态域时，只有真正声明这个域的类才会被初始化。
      * 当通过子类引用父类的静态变量，不会导致子类初始化。
      * 通过数组定义类引用，不会触发此类的初始化。
      * 引用常量不会触发此类的初始化（常量在链接阶段就存入调用类的常量池中了）。
  
* 类加载器的作用：

  * 类加载的作用：将`class`文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据结构，然后在堆中生成一个代表这个类的`java.lang.Class`对象，作为方法区中类数据的访问入口。

  * 类缓存：标准的`JavaSE`类加载器可以按要求查找类，但一旦某个类被加载到类加载器中，它将维持加载（缓存）一段时间。不过JVM垃圾回收机制可以回收这些`Class`对象。

* 类加载器的分类

  * 引导类加载器：用C++编写的，是JVM自带的类 加载器，负责Java平台核心库，用来装载核心类库。该加载器无法直接获取。
  * 扩展类加载器：负责`jre/lib/ext`目录下的jar包或 `– D java.ext.dirs` 指定目录下的jar包装入工作库。
  * 系统类加载器：负责`java –classpath` 或 `–D java.class.path`所指的目录下的类与jar包装入工 作 ，是最常用的加载器。
  * 自定义加载器：继承或者实现某个类加载器并实现某些方法。
```java
//获取类加载器
System.out.println(aClass3.getClassLoader().getClass().getName());//sun.misc.Launcher$AppClassLoader 系统类加载器
System.out.println(aClass3.getClassLoader().getParent().getClass().getName());//sun.misc.Launcher$ExtClassLoader 扩展类加载器
System.out.println(String.class.getClassLoader());//null 引导类加载器
System.out.println(aClass3.getClassLoader().getParent().getParent());//null 引导类加载器
System.out.println(Object.class.getClassLoader()); //null 引导类加载器
```

# 创建运行时类的对象

* 创建类的对象：调用Class对象的`newInstance()`方法

  * 类必须有一个无参的构造器。
  * 类的构造器的访问权限必须足够，否则抛`IllegalAccessException`，一般为`public`权限。

  ```java
  Class aClass = Class.forName("com.sunflower.reflection.Person");
  Object o = aClass.newInstance();
  System.out.println(o);//Person{name='null', age=0, wages=0.0}
  ```

* 通过有参构造器创建对象
  * 1）通过Class类的`public Constructor<T> getConstructor(Class<?>... parameterTypes)`或者`getDeclaredConstructor(Class … parameterTypes)`取得本类的指定形参类型的构造器。
  * 向构造器的形参中传递一个对象数组进去，里面包含了构造器中所需的各个参数。
  * 通过`Constructor`调用`newInstance(Object ... initargs)`方法实例化对象。

```java
Constructor constructor = aClass.getDeclaredConstructor(String.class, int.class, float.class);
constructor.setAccessible(true);
Object tom = constructor.newInstance("tom", 18, 10.5f);
System.out.println(tom);//Person{name='tom', age=18, wages=10.5}
```

# 获取运行时类的完整结构

## 接口

* `public Class[] getInterfaces()` ：返回类实现的所有接口。

  ```java
  Class aClass = Class.forName("com.sunflower.reflection.SuperMan");
  Class[] interfaces = aClass.getInterfaces();
  for(Class inter: interfaces){
      System.out.println(inter.getName());//java.io.Serializable com.sunflower.reflection.MyInterface
  }
  ```

## 父类

* `public native Class<? super T> getSuperclass()`：返回表示此 `Class` 所表示的实体（类、接口、基本类型）的父类的 `Class`。

  ```java
  Class superclass = aClass.getSuperclass();
  System.out.println(superclass.getName());//com.sunflower.reflection.Man
  ```

## 构造器

* `public Constructor<?>[] getConstructors() throws SecurityException`：返回此 `Class` 对象所表示的类的所有`public`构造方法。

  ```java
  Class aClass = Class.forName("com.sunflower.reflection.SuperMan");
  Constructor[] constructors = aClass.getConstructors();
  for(Constructor struct:constructors){
      System.out.println(struct.getModifiers());//1
      System.out.println(struct);//public com.sunflower.reflection.SuperMan()
  }
  ```

* `public Constructor<?>[] getDeclaredConstructors() throws SecurityException`：返回此 `Class` 对象表示的类声明的所有构造方法。

```java
Constructor[] declaredConstructors = aClass.getDeclaredConstructors();
        for(Constructor struct:declaredConstructors){
            StringBuffer constructor = new StringBuffer();
            switch (struct.getModifiers()){
                case 1:
                    constructor.append("public ");
                    break;
                case 2:
                    constructor.append("private ");
                    break;
                case 4:
                    constructor.append("protected ");
                    break;
            }
            String name = struct.getName();
            constructor.append(name+"(");
            Parameter[] parameters = struct.getParameters();//获取形参列表
            for(int i=0;i<parameters.length;i++){
                String type = parameters[i].getType().getName();//类型名称
                String arg = parameters[i].getName();//参数名称
                if(i>=parameters.length-1){
                    constructor.append(type+" "+arg+")");
                }else {
                    constructor.append(type+" "+arg+",");
                }
            }
            System.out.println(constructor);
            // public com.sunflower.reflection.SuperMan()
            // public com.sunflower.reflection.SuperMan(
            // protected com.sunflower.reflection.SuperMan(double arg0,java.lang.String arg1)
            // com.sunflower.reflection.SuperMan(double arg0,java.lang.String arg1,int arg2)
            // private com.sunflower.reflection.SuperMan(double arg0,java.lang.String arg1,int arg2,float arg3)
        }
```

### Constructor类常用方法

* 获取修饰符: `public int getModifiers()`; 
* 获取方法名称: `public String getName()`; 
* 获取参数的类型：`public Class[] getParameterTypes()`;

##  方法

*  `public Method[] getDeclaredMethods()`：返回此`Class`对象所表示的类或接口的全部方法。
* `public Method[] getMethods()`：返回此`Class`对象所表示的类或接口的`public`的方法。

### Method类常用方法

* `public Class getReturnType()`：取得全部的返回值。
* `public Class[] getParameterTypes()`：取得全部的参数。
* `public int getModifiers()`：取得修饰符。
* `public Class[] getExceptionTypes()`：取得异常信息。

## Field

* `public Field[] getFields()`：返回此`Class`对象所表示的类或接口的`public`的`Field`。
* `public Field[] getDeclaredFields()`：返回此`Class`对象所表示的类或接口的全部`Field`。

### Field类中常用方法

* `public int getModifiers()`：以整数形式返回此`Field`的修饰符。
* `public Class getType()`：得到`Field`的属性类型。
* `public String getName()`：返回`Field`的名称。

## Annotation(注解)

* `get Annotation(Class annotationClass)`
* `getDeclaredAnnotations()`

## Generic(泛型)

* `Type getGenericSuperclass()`：获取父类泛型类型，实际返回的是`ParameterizedType`。

* `ParameterizedType`：泛型类型。
  * `Type[] getActualTypeArguments()`：获取实际的泛型类型参数数组。

## Package(包)

* `Package getPackage()` ：获取类所在的包。

# 调用运行时类的指定结构

## 方法

* 通过`Class`类的`getMethod(String name,Class... parameterTypes)`方法取得 一个`Method`对象，并设置此方法操作时所需要的参数类型。
* 之后使用`Object invoke(Object obj, Object... args)`进行调用，并向方法中传递要设置的`obj`对象(静态方法传递`null`)和参数信息。
  * 返回值`Object`对应原方法的返回值，若原方法无返回值，此时返回`null`。
  * .若原方法若为静态方法，此时形参`Object obj`可为`null`。
  * 若原方法形参列表为空，则`Object... args`为`null`。
  * 若原方法声明为`private`,则需要在调用此`invoke()`方法前，显式调用方法对象的`setAccessible(true)`方法，将可访问`private`的方法。

## 属性

* `public Field getField(String name)` ：返回此`Class`对象表示的类或接口的指定的`public`的`Field`。
* `public Field getDeclaredField(String name)`：返回此`Class`对象表示的类或接口的指定的`Field`。

```java
Class aClass = Class.forName("com.sunflower.reflection.SuperMan");
Field count = aClass.getDeclaredField("count");//静态属性
count.setAccessible(true);
System.out.println(count.get(null));// 5
count.set(null,100);
System.out.println(count.get(null));// 100
Object o = aClass.newInstance();

Class superclass = aClass.getSuperclass();//得到父类的Class
Field value = superclass.getDeclaredField("value");//得到父类中的属性
value.setAccessible(true);
value.set(o,"12487489");//给子类对象中的父类定义的属性赋值
System.out.println(value.get(o));//12487489 得到属性的值
```

# 反射的应用：动态代理

* 创建一个实现接口`InvocationHandler`的类，它必须实现`invoke()`方法，以完成代理的具体操作。

  ```java
  class MyInvocationHandler implements InvocationHandler{
      private Object object;
      public void BindObject(Object object){
          this.object = object;
      }
      //此方法的返回值是被代理类方法中的返回值
      @Override
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          return method.invoke(object,args);
      }
  }
  ```

* 创建一个专门生产代理类对象的通用代理类。

  * 编写一个返回代理类对象的方法。
  * 调用`Proxy`类中的`public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) throws IllegalArgumentException`方法，动态生成一个代理类对象，并把此对象返回。

  ```java
  public class MyProxy{
      //返回代理类的对象
      public static Object getProxy(Object object){
          MyInvocationHandler myInvocationHandler = new MyInvocationHandler();
          myInvocationHandler.BindObject(object);
          Object newProxyInstance = Proxy.newProxyInstance(object.getClass().getClassLoader(),
                  object.getClass().getInterfaces(), myInvocationHandler);
          return newProxyInstance;
      }
  }
  ```

* 创建被代理类接口

  ```java
  public interface Occupation {
      void Work(String Task);
      int MakeMoney();
  }
  ```

* 创建被代理类并实现被代理类接口

  ```java
  public class Star implements Occupation{
      @Override
      public void Work(String task) {
          System.out.println(task);
      }
  
      @Override
      public int MakeMoney() {
          System.out.println("明星一年可以赚五千万");
          return 50000000;
      }
  }
  ```

* 调用通用代理类的`public static Object getProxy(Object object)`方法，把被代理类的实例作为参数传递到此方法中，并得到一个代理类实例，通过此代理类实例调用与被代理类同名同参数的方法。

  ```java
  class TestProxy{
      @Test
      public void TestProxy(){
          Occupation proxy = (Occupation) MyProxy.getProxy(new Star());
          proxy.Work("唱歌跳舞");
          int money = proxy.MakeMoney();
          System.out.println(money);
          Factory proxy1 = (Factory) MyProxy.getProxy(new NikeFactory());
          proxy1.Production();
      }
  }
  ```

