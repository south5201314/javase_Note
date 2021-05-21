[TOC]

# Lambda表达式

* Lambda 是一个匿名函数，我们可以把 Lambda 表达式理解为是一段可以 传递的代码（将代码像数据一样进行传递）。使用它可以写出更简洁、更 灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了 提升。

```java
public class LambdaTest {
    @Test
    public void TestLambda(){
        // 语法格式一：无参，无返回值
        Runnable runnable = ()-> {
            System.out.println("Hello Lambda");
        };
        runnable.run();//Hello Lambda.

        // 语法格式二：Lambda 需要一个参数，但是没有返回值
        Consumer<String> con = (String str)-> {
            System.out.println(str);
        };
        con.accept("Lambda");//Lambda

        // 语法格式三：数据类型可以省略，因为可由编译器推断得出，称为“类型推断”
        Consumer<Integer> con2 = (integer)->{
            System.out.println(integer);
        };
        con2.accept(12487489);//12487489

        // 语法格式四：Lambda 若只需要一个参数时，参数的小括号可以省略
        Consumer<Float> con3 = f ->{
            System.out.println(f);
        };
        con3.accept(0.124f);//0.124

        // 语法格式五：Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值
        Comparator<Integer> com1 = (integer1,integer2)->{
            System.out.println("多条语句");
            return Integer.compare(integer1,integer2);
        };
        System.out.println(com1.compare(12, 2));//1

        //语法格式六：当 Lambda 体只有一条语句时，return 与大括号若有，都可以省略
        Comparator<Integer> com2 = (integer1,integer2)-> Integer.compare(integer1,integer2);
        System.out.println(com2.compare(188, 988));//-1
    }
}
```

# 函数式接口

* 只有一个抽象方法的接口，称为函数式接口。
* 我们可以在一个接口上使用 `@FunctionalInterface` 注解，这样做可以检查它是否是一个函数式接口。同时 javadoc 也会包含一条声明，说明这个接口是一个函数式接口。
* 在`java.util.function`包下定义了Java 8 的丰富的函数式接口。
* `Lambda`表达式就是一个函数式接口的实例。
```java
//自定义函数式接口
public interface MyFunctionalInterface<T> {
    T getValue(T t);
}

@Test
public void FunctionalInterfaceTest(){
    MyFunctionalInterface<String> mFun = str -> str;
    String result = mFun.getValue("this is MyFunctionalInterface test demo");
    System.out.println(result);//this is MyFunctionalInterface test demo
}
```

## Java 内置四大核心函数式接口

|        函数式接口         | 参数类型 | 返回类型 | 用途                                                         |
| :-----------------------: | :------: | :------: | :----------------------------------------------------------- |
|  `Consumer<T>`消费型接口  |    T     |   void   | 对类型为T的对象应用操作，包含方法： `void accept(T t)`       |
|  `Supplier<T>`供给型接口  |    无    |    T     | 返回类型为T的对象，包含方法：`T get()`                       |
| `Function<T,R>`函数型接口 |    T     |    R     | 对类型为T的对象应用操作，并返回结果。结果是R类型的对象。包含方法：`R apply(T t)` |
| `Predicate<T>`断定型接口  |    T     | boolean  | 确定类型为T的对象是否满足某约束，并返回 boolean 值。包含方法：`boolean test(T t)` |

## 其他接口

| 函数式接口          | 参数类型 | 返回类型 | 用途                                                         |
| ------------------- | -------- | -------- | ------------------------------------------------------------ |
| `BiFunction<T,U,R>` | T,U      | R        | 对类型为 T, U 参数应用操作，返回 R 类型的结果。包含方法为： `R apply(T t, U u)` |
| `UnaryOperator<T>`  | T        | T        | 对类型为T的对象进行一元运算，并返回T类型的 结果。包含方法为：`T apply(T t)` |
| `BinaryOperator<T>` | T,T      | T        | 对类型为T的对象进行二元运算，并返回T类型的 结果。包含方法为： `T apply(T t1, T t2)` |
| `BiConsumer<T,U>`   | T,U      | void     | 对类型为T, U 参数应用操作。 包含方法为： `void accept(T t, U u)` |
| `BiPredicate<T,U>`  | T,U      | boolean  | 包含方法为： `boolean test(T t,U u)`                         |
| `ToXxxFunction<T>`  | T        | Xxx      | 分别计算int、long、double值的函数(Xxx：int/long/double)      |
| `XxxFunction<R>`    | Xxx      | R        | 参数分别为int、long、double 类型的函数(Xxx：int/long/double) |



# 方法引用和构造器引用

## 方法引用

* 当要传递给Lambda体的操作，已经有实现的方法了，可以使用方法引用！
* 方法引用就是Lambda表达式。
* 要求：实现接口的抽象方法的参数列表和返回值类型，必须与被引用的方法的参数列表和返回值类型保持一致！

```java
public class MethodQuoteTest {
    @Test
    public void MethodQuoteTest(){
        //对象::方法
        Consumer<String> con1 = s -> System.out.println(s);
        // 等同于:
        Consumer<String> con2 = System.out::println;
        con1.accept("System.out.println(s)");// System.out.println(s)
        con2.accept("System.out::println");// System.out::println
        
        //类::静态方法
        Comparator<Integer> com1 = (x,y)->Integer.compare(x,y);
        Comparator<Integer> com2 = Integer::compare;
        System.out.println(com1.compare(100, 50));// 1
        System.out.println(com2.compare(100, 50));// 1
        //类::实例方法
        BiFunction<String,String,Integer> biFun1 = (s1,s2)-> s1.compareTo(s2);
        BiFunction<String,String,Integer> biFun2 = String::compareTo;
        System.out.println(biFun1.apply("abc", "apc"));// -14
        System.out.println(biFun2.apply("abc", "apc"));// -14
    }
}
```

## 构造器引用

* 要求

  * 构造器参数列表要与接口中抽象方法的参数列表一致。

  * 抽象方法的返回值即为构造器对应类的对象。

```java
public class ConstructionQuoteTest {
    @Test
    public void ConstructionQuoteTest(){
        Function<String,StringBuffer> fun1 = s -> new StringBuffer(s);
        Function<String,StringBuffer> fun2 = StringBuffer::new;
        StringBuffer sb1 = fun1.apply("s -> new StringBuffer(s)");
        System.out.println(sb1);//s -> new StringBuffer(s)
        StringBuffer sb2 = fun2.apply("StringBuffer::new");
        System.out.println(sb2);//StringBuffer::new
    }
}
```

## 数值引用

* 数组引用类似于构造器引用，把数组的“[]”当做是一个参数的构造器。

```java
public class ArrayQuoteTest {
    @Test
    public void ArrayQuoteTest(){
        Function<Integer,String[]> fun1 = len ->new String[len];
        Function<Integer,String[]> fun2 = String[]::new;
        String[] strings1 = fun1.apply(8);
        System.out.println(strings1.length); // 8
        String[] strings2 = fun2.apply(10);
        System.out.println(strings2.length); // 10
    }
}
```

# Stream API

* `Stream API ( java.util.stream)` 把真正的函数式编程风格引入到Java中。
* 使用 `Stream API` 对集合数据进行操作，就类似于使用 SQL 执行的数据库查询。
* `Collection` 是一种静态的内存数据结构，而 `Stream` 是有关计算的。
* 集合讲的是数据，`Stream`讲的是计算。
* 注意：
  * `Stream` 自己不会存储元素。 
  * `Stream` 不会改变源对象。相反，他们会返回一个持有结果的新`Stream`。 
  * `Stream` 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

## 创建Stream

* 数据源（如：集合、数组），获取一个流(`Stream`实例)。

### 通过集合

* Java8 中的 `Collection` 接口被扩展，提供了两个获取流的方法：
  * default Stream stream() : 返回一个顺序流。
  * default Stream parallelStream() : 返回一个并行流。

```java
//通过集合
        Collection<Student> students = Team.GetTeam();
        Stream<Student> stream = students.stream();
        Stream<Student> parallelStream = students.parallelStream();
```

### 通过数组

* Java8 中的 `Arrays` 的静态方法 `stream()` 可以获取数组流：
  * `static  Stream stream(T[] array)`: 返回一个流。
    * 重载形式，能够处理对应基本类型的数组：
    * p`ublic static IntStream stream(int[] array)`
    * `public static LongStream stream(long[] array)`
    * `public static DoubleStream stream(double[] array)`

```java
//通过数组
int[] is = new int[]{1,8,4,9,0,11,88};
IntStream arrayStream = Arrays.stream(is);
```

### 通过of()方法

* 可以调用`Stream`类静态方法 `of(),` 通过显示值创建一个流。它可以接收任意数量的参数。

* `public static Stream of(T... values)` : 返回一个流。

```java
//通过Stream的of()
Stream<String> stringStream = Stream.of("qwe", "rty", "uio");
```

### 创建无限流

* 可以使用静态方法 `Stream.iterate()` 和 `Stream.generate()`创建无限流。
* 迭代：`public static Stream iterate(final T seed, final UnaryOperator f)`
* 生成：`public static Stream generate(Supplier s)` 

```java
//创建无限流
 //迭代
 Stream<Integer> iterate = Stream.iterate(0, sum -> sum + 2);
 iterate.limit(100).forEach(System.out::println);
 //生产
 Stream<Double> generate = Stream.generate(Math::random);
 generate.limit(10).forEach(System.out::println);
```

## 中间操作

* 中间操作链，对数据源的数据进行处理。

### 筛选与切片

| Method              | Desc                                                         |
| ------------------- | ------------------------------------------------------------ |
| filter(Predicate p) | 接受Lambda，从流种排除不符合条件的元素                       |
| distinct()          | 筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素 |
| limit(long maxSize) | 截断流，使其元素不超好指定数量                               |
| skip(long n)        | 跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一 个空流。与 limit(n) 互补 |

```java
    @Test
    public void MidOperatorTest(){
        Collection<Student> students = Team.GetTeam();
        Stream<Student> studentStream = students.stream();
        //筛选所有男同学出来
        studentStream.filter(student -> student.getSex().equals(Sex.Male)).forEach(System.out::println);
        System.out.println();
        //通过流所生成元素的 hashCode() 和 equals() 去除重复元素
        students.add(new Student("陈冠希",33,Sex.Male));
        students.add(new Student("陈冠希",33,Sex.Male));
        students.add(new Student("陈冠希",33,Sex.Male));
        students.add(new Student("陈冠希",33,Sex.Male));
        students.add(new Student("陈冠希",33,Sex.Male));
        students.stream().distinct().forEach(System.out::println);
        System.out.println();
        //截断,打印前五个
        students.stream().limit(5).forEach(System.out::println);
        System.out.println();
        //跳过,打印第五个后面所有的元素
        students.stream().skip(5).forEach(System.out::println);
    }
```

### 映射

| Method                          | Desc                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| map(Function f)                 | 接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。 |
| mapToDouble(ToDoubleFunction f) | 接收一个函数作为参数，该函数会被应用到每个元 素上，产生一个新的 DoubleStream。 |
| mapToInt(ToIntFunction f)       | 接收一个函数作为参数，该函数会被应用到每个元 素上，产生一个新的 IntStream。 |
| mapToLong(ToLongFunction f)     | 接收一个函数作为参数，该函数会被应用到每个元 素上，产生一个新的 LongStream。 |
| flatMap(Function f)             | 接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。 |

```java
@Test
public void MidOperatorTest2(){
    Collection<Student> students = Team.GetTeam();
    //把name映射出来
    Stream<Student> studentStream = students.stream();
    Stream<String> nameStream = studentStream.map(student -> student.getName());
    nameStream.forEach(System.out::println);

    //把年龄映射出来，指定类型映射
    Stream<Student> studentStream1 = students.stream();
    IntStream mapToInt = studentStream1.mapToInt(student -> student.getAge());
    mapToInt.forEach(System.out::println);
    System.out.println();

    //通过年龄映射id
    Stream<Student> studentStream2 = students.stream();
    studentStream2.flatMap(student -> {
        if(student.getAge()<30){
            return Stream.of(student.getId());
        }
        return null;
    }).forEach(System.out::println);
}
```

### 排序

| Method                 | Desc                               |
| ---------------------- | ---------------------------------- |
| sorted()               | 产生一个新流，其中按自然顺序排序   |
| sorted(Compararor com) | 产生一个新流，其中按比较器顺序排序 |

```java
@Test
public void MidOperatorTest3(){
    Collection<Student> students = Team.GetTeam();
    Stream<Student> studentStream = students.stream();
    //自然排序,需要实现Comparable接口
    studentStream.sorted().forEach(System.out::println);
    System.out.println();
    //定制排序
    Stream<Student> studentStream1 = students.stream();
    studentStream1.sorted((student1,student2)-> -student1.getId()- student2.getId())
            .forEach(System.out::println);
}
```

## 终止操作(终端操作)

* 一旦执行终止操作，中间操作链才会被执行(延时操作)，并产生结果。之后，此`Stream`实例不能再被使用。

### 匹配与查找

| Method                 | Desc                                                         |
| ---------------------- | ------------------------------------------------------------ |
| allMatch(Predicate p)  | 检查是否匹配所有元素                                         |
| anyMatch(Prediccate p) | 检查是否至少匹配一个元素                                     |
| nomeMatch(Predicate p) | 检查是否没有匹配所有元素                                     |
| findFist()             | 返回第一个元素                                               |
| findAny()              | 返回当前流中的任意元素                                       |
| count()                | 返回流中元素总数                                             |
| max(Comparator com)    | 返回流中最大值                                               |
| min(Comparator com)    | 返回流中最小值                                               |
| forEach(Consumer con)  | 内部迭代(使用 Collection 接口需要用户去做迭代， 称为外部迭代。相反，Stream API 使用内部迭代——它帮你把迭代做了) |

```java
@Test
public void StopOperatorTest1(){
    Collection<Student> students = Team.GetTeam();
    Stream<Student> studentStream = students.stream();
    //学生是否都大于50岁
    System.out.println(studentStream.allMatch(student -> student.getAge() < 50));//true
    // 学生名单中是否有姓李的
    Stream<Student> studentStream1 = students.stream();
    System.out.println(studentStream1.anyMatch(student -> student.getName().startsWith("李")));//false
    //学生名单中是不是没有姓黄的
    Stream<Student> studentStream2 = students.stream();
    System.out.println(studentStream2.noneMatch(student ->
            student.getName().startsWith("黄")));//ture 是没有姓黄的
    //返回年龄最小的
    Stream<Student> studentStream3 = students.stream();
    //studentStream3.sorted(Comparator.comparingInt(Student::getAge)).forEach(System.out::println);
    Optional<Student> student = studentStream3.sorted().findFirst();
    Student student1 = student.get();
    System.out.println(student1);// Student{name='张一鸣', age=17, sex=Male, id=10003}
    // 返回流中的元素总数
    System.out.println(students.stream().count());// 11
}
```

### 归约

| Method                          | Desc                                                   |
| ------------------------------- | ------------------------------------------------------ |
| reduce(T iden,BinaryOperator b) | 可以将流中元素反复结合起来，得到一 个值。返回 T        |
| reduce(BinaryOperator b)        | 可以将流中元素反复结合起来，得到一 个值。返回 Optional |

```java
@Test
public void StopOperatorTest2(){
    Collection<Student> students = Team.GetTeam();
    Stream<Student> studentStream = students.stream();
    Stream<Integer> ageStream = studentStream.map(student -> student.getAge());
    Integer ageSum = ageStream.reduce(0, Integer::sum);
    System.out.println(ageSum);//280

    IntStream stream = Arrays.stream(new int[]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10});
    OptionalInt optionalInt = stream.reduce(Integer::sum);
    int sum = optionalInt.orElse(100);
    System.out.println(sum);
}
```

* 备注：map 和 reduce 的连接通常称为 map-reduce 模式，因 Google 用它来进行网络搜索而出名。

### 收集

| Method               | Desc                                                         |
| -------------------- | ------------------------------------------------------------ |
| collect(Collector c) | 将流转换为其他形式。接收一个 Collector 接口的实现，用于给Stream中元素做汇总 的方法 |

```java
@Test
public void StopOperatorTest3(){
    Collection<Student> students = Team.GetTeam();
    Stream<Student> studentStream = students.stream();
    //把Name映射出来并收集到Set集合中
    Stream<String> nameStream = studentStream.map(student -> student.getName());
    Set<String> nameCollect = nameStream.collect(Collectors.toSet());
    System.out.println(nameCollect);
}
```

* Collector 接口中方法的实现决定了如何对流执行收集的操作(如收集到 List、Set、 Map)。

  | Method            | Return type          | Desc                                                         |
  | ----------------- | -------------------- | ------------------------------------------------------------ |
  | toList            | List<T>              | 把流中元素收集到List                                         |
  | toSet             | Set<T>               | 把流中元素收集到Set                                          |
  | toCollection      | Collection<T>        | 把流中元素收集到创建的集合                                   |
  | counting          | Long                 | 计算流中元素的个数                                           |
  | summingInt        | Integer              | 对流中元素的整数属性求和                                     |
  | averagingInt      | Double               | 计算流中元素Integer属性的平均值                              |
  | summarizingInt    | IntSummaryStatistics | 收集流中Integer属性的统计值。如：平 均值                     |
  | joining           | String               | 连接流中每个字符串                                           |
  | maxBy             | Optional<T>          | 根据比较器选择最大值                                         |
  | minBy             | Optional<T>          | 根据比较器选择最小值                                         |
  | reducing          | 归约产生的类型       | 从一个作为累加器的初始值开始， 利用BinaryOperator与流中元素逐个结合，从而归约成单个值 |
  | collectingAndThen | 转换函数返回的类型   | 包裹另一个收集器，对其结果转换函数                           |
  | groupingBy        | Map<K,List<T>>       | 根据某属性值对流分组，属性为K， 结果为V                      |
  | partitioningBy    | Map<Boolean,List<T>> | 根据true或false进行分区                                      |

# Optional类

* 臭名昭著的空指针异常是导致Java应用程序失败的最常见原因。
* `Optional` 类(`java.util.Optional`) 是一个容器类，它可以保存类型`T`的值，代表这个值存在。或者仅仅保存`null`，表示这个值不存在。原来用 `null` 表示一个值不 存在，现在 `Optional` 可以更好的表达这个概念。并且可以避免空指针异常。
* 一般只会使用`Optional`的方法，而不是创建`Optional`的实例。

### 创建Optional类对象的方法：

* `Optional.of(T t)` : 创建一个 `Optional` 实例，t必须非空。
* `Optional.empty()` : 创建一个空的 `Optional` 实例。
* `Optional.ofNullable(T t)`：t可以为`null` 。

### 判断Optional容器中是否包含对象： 

* `boolean isPresent()` : 判断是否包含对象。
* `void ifPresent(Consumer consumer)` ：如果有值，就执行`Consumer` 接口的实现代码，并且该值会作为参数传给它。

### 获取Optional容器的对象：

* `T get()`：如果调用对象包含值，返回该值，否则抛异常。
* `T orElse(T other)` ：如果有值则将其返回，否则返回指定的other对象。
* `T orElseGet(Supplier other)` ：如果有值则将其返回，否则返回由 Supplier接口实现提供的对象。
* `T orElseThrow(Supplier exceptionSupplier)` ：如果有值则将其返 回，否则抛出由Supplier接口实现提供的异常。