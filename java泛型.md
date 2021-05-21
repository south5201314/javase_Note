# 泛型的概念

* 把元素的类型设计成一个参数，这个类型参数叫做泛型。
* 从JDK1.5以后，Java引入了“参数化类型（Parameterized type）”的概念。
* 为什么要有泛型？Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生 `ClassCastException`异常。同时，代码更加简洁、健壮。

# 集合中使用泛型

```java
public class GenericTest1 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<String>();
        list.add("aa");
        list.add("bb");
        //list.add(123);编译错误
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()){
            String s = iterator.next();
            System.out.println(s);//aa bb
        }

        HashMap<String, Integer> map = new HashMap<>();
        map.put("tom",88);
        map.put("jax",55);
        map.put("li",76);
        Set<Map.Entry<String, Integer>> entrySet = map.entrySet();
        Iterator<Map.Entry<String, Integer>> iterator1 = entrySet.iterator();
        while (iterator1.hasNext()){
            Map.Entry<String, Integer> next = iterator1.next();
            String name = next.getKey();
            int number = next.getValue();
            System.out.println("name:"+name+" number:"+number);
        }
    }
}
```

# 自定义泛型结构

* 泛型类可能有多个参数，此时应将多个参数一起放在尖括号内。
* 泛型不同的引用不能相互赋值。
* 泛型如果不指定，将被擦除，泛型对应的类型均按照`Object`处理，但不等价于`Object`。
*  泛型的不能使用基本数据类型，可以使用包装类替换。
* 静态方法中不能使用类的泛型。但泛型方法可以是静态方法。
* 异常类不能是泛型。
* 父类有泛型，子类可以选择保留泛型也可以选择指定泛型类型：
  * 子类不保留父类的泛型： 去除(自动变成`Object`类型)或者指定具体类型。
  * 子类保留父类的泛型：全部保留或者部分保留(不保留的部分自动变成`Object`类型)。

* 子类除了指定或保留父类的泛型，还可以增加自己的泛型。

##  自定义泛型类

```java
public class CustomGeneric<T>{
    T t;
    ...
    public void SetT(T t){
        this.t = t;
    }
    public T getT(){
        return this.t;
    }
    ...
}
```

## 自定义泛型接口

```java
public interface CustomGenericInterface<T,E> {
    T getT();
    E getE();
}
```

```java
public class GenericImplement<T,E,V> implements CustomGenericInterface<T,E>{
    private T t;
    private E e;
    private V v;
   public GenericImplement(){ }
   public GenericImplement(T t,E e,V v){
       this.t = t;
       this.e = e;
       this.v = v;
   }
    @Override
    public T getT() {
        return this.t;
    }

    @Override
    public E getE() {
        return this.e;
    }
    //泛型方法
    public static <G extends CustomGenericInterface> String show(G g){
       return g.getT()+" "+g.getE();
    }
}
```

## 自定义泛型方法

```java
//泛型方法,具体看自定义泛型接口代码(上面)
    public static <G extends CustomGenericInterface> String show(G g){
       return g.getT()+" "+g.getE();
    }
```

```java
public class GenericTest2 {
    public static void main(String[] args) {
        CustomGeneric<String> tom = new CustomGeneric<>("tom", 1001, "tom:1001");
        System.out.println(tom.getT());//tom:1001
        CustomGenericInterface genericImplement = new GenericImplement<>("jak", 98, "菜鸡");
        System.out.println(genericImplement.getT());
        String show = GenericImplement.show(genericImplement);//调用泛型方法
        System.out.println(show);//jak 98
    }
}
```

# 泛型在继承上的体现

* 如果`B`是`A`的一个子类型（子类或者子接口），而`G`是具有泛型声明的类或接口，`G<A>并不是G<B>的子类型`。

```java
List<String> list = null;
List<Date> list1 =new ArrayList<Date>();
//list = list1; //编译错误
```

# 通配符

* 通配符：`?`
* `List`是`List<String>`、`List<Object>`等各种泛型`List`的父类。
* 读取`List<?>`对象中的元素时，永远是安全的，因为不管集合的真实类型是什么，它包含的都是`Object`.
* 写入元素时，不行。因为我们不知道集合中的元素类型，我们不能向其中添加对象。
  * 唯一的例外是`null`，它是所有类型的成员。

## 使用注意

* 不能用在泛型方法声明上，返回值类型前面<>不能使用。
* 不能用在泛型类的声明上。

* 不能用在创建对象上，`new`后面属于创建对象。

## 有限制的通配符

* 上限`extends`
  * 指定的类型必须是继承某个类或者实现某个接口。
  * `<? extends Number> (无穷小 , Number]`：只允许泛型为`Number`及`Number`子类的引用调用。
* 下限`super`
  * 指定的类型必须是某个类的父类或者父接口。
  * `<? super Number> [Number , 无穷大)`：只允许泛型为`Number`及`Number`父类的引用调用。

```java
public static void TestAddSuper(Collection<? super CustomGenericInterface> collection){
        collection.add(new GenericImplement());//CustomGenericInterface的实现类实例可以添加
        Iterator<?> iterator = collection.iterator();
        while (iterator.hasNext()){
            Object next = iterator.next();
            System.out.println(next);
        }
    }
    public static void TestAddExtends(Collection<? extends CustomGenericInterface> collection){
        //编译错误，无法添加
        //collection.add(new GenericImplement());
        //collection.add(new Object());

        Iterator<? extends CustomGenericInterface> iterator = collection.iterator();
        while (iterator.hasNext()){
            CustomGenericInterface next = iterator.next();
            System.out.println(next);
        }
    }
```

