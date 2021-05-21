[TOC]

# Collection接口

* 单列集合，用于存储单个对象

## 常用方法

* `public boolean add(E e)`：添加元素。
* `public boolean addAll(Collection<? extends E> c)`：把参数集合中的所有元素添加到当前集合中。
* `public int size()`：获取集合中有效元素的个数。
* `public void clear()`：清空集合。
* `public boolean isEmpty()`：是否是空集合。
* `public boolean contains(Object o)`：是否包含某个元素
* `public boolean containsAll(Collection<?> c)` ：当前集合中是否包含参数集合中所有元素。
* `public boolean remove(Object o)`：删除指定元素。
* `public boolean removeAll(Collection<?> c)`：删除当前集合与参数集合相等的所有元素(差集)。
* `public boolean retainAll(Collection<?> c)`：保留当前集合与参数集合相等的所有元素(交集)。
* `public boolean equals(Object o)`：集合是否相等。
* `public Object[] toArray()`：转成对象数组。
* `public int hashCode()`：获取集合对象的哈希值。
* `public Iterator<E> iterator()`：返回迭代器对象，用于集合遍历。

## Iterator接口(迭代器)

### 常用方法

* `boolean hasNext()`：是否还有元素。
* `E next()`：游标下移并返回下移后的元素。
* `default void remove()`：删除当前游标位置的元素。
  * 注意：如果还未调用`next()`或在上一次调用 next 方法之后已经调用了 `remove()` 方法， 再调用`remove()`都会报`IllegalStateException`。

```java
public class IteratorTest {
    public static void main(String[] args) {
        ArrayList arrayList = new ArrayList();
        arrayList.add("555");
        arrayList.add("666");
        arrayList.add(1247);
        Iterator iterator = arrayList.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
}
```

## foreach循环

* Java 5.0 提供了 `foreach` 循环迭代访问 `Collection`和数组。
* 遍历操作不需获取`Collection`或数组的长度，无需使用索引访问元素。
* 遍历集合的底层调用`Iterator`完成操作。
* `foreach`还可以用来遍历数组。

```java
public class IteratorTest {
    public static void main(String[] args) {
        ArrayList arrayList = new ArrayList();
        arrayList.add("555");
        arrayList.add("666");
        arrayList.add(1247);
        for(Object o:arrayList){
            System.out.println(o);
        }
    }
}
```

## List接口

* 存储有序、可重复的数据。

### 常用方法

List除了从Collection集合继承的方法外，List 集合里添加了一些根据索引来操作集合元素的方法。

* `public void add(int index, E element)`：在`index`位置插入`element`元素。
* `public boolean addAll(Collection<? extends E> c)`:从`index`位置开始将`c`中的所有元素添加进来。
* `public E get(int index)`：获取指定`index`位置的元素。
* `public int indexOf(Object o)`：返回`o`在集合中首次出现的位置。
* `public int lastIndexOf(Object o)`：返回`o`在当前集合中末次出现的位置。
* `public E remove(int index)`：移除指定`index`位置的元素，并返回此元素。
* `public E set(int index, E e)`：设置指定`index`位置的元素为`e`。
* `public List<E> subList(int fromIndex, int toIndex)`：返回从`fromIndex`到`toIndex`位置的子集合。

### ArrayList类

####  原理分析

* 底层使用一个Object类型的数据来存储。
* 在调用默认构造器时，jdk8之前默认初始化容量为10，jdk8之后默认初始化容量为0，在添加第一个元素时再创建容量为10的数组。
* 扩容大小是当前的1.5倍。

### LinkedList类

#### 原理分析

* 底层使用一个双向列表来存储数据。

* 定义了Node类型的first和last，用于记录首末元素。

* 定义内部类Node，作为LinkedList中保存数据的基 本结构。Node除了保存数据，还定义了两个变量：

  * prev变量记录前一个元素的位置。

  * next变量记录下一个元素的位置。

    ```java
        private static class Node<E> {
            E item;
            Node<E> next;//下一个元素的位置
            Node<E> prev;//前一个元素的位置
    
            Node(Node<E> prev, E element, Node<E> next) {
                this.item = element;
                this.next = next;
                this.prev = prev;
            }
        }
    ```

#### 新增方法

* `public void addFirst(E e)`：在第一个位置添加元素。
* `public void addLast(E e)`：在最近一个位置添加元素。
* `public E getFirst()`：获取第一个元素。
* `public E getLast()`：获取最后一个元素。
* `public E removeFirst()`：删除第一个元素。
* `public E removeLast()`：删除最后一个元素

### Vertor类

* 请问`ArrayList`/`LinkedList`/`Vector`的异同？谈谈你的理解？`ArrayList`底层 是什么？扩容机制？`Vector`和`ArrayList`的最大区别?

  * `ArrayList`和`LinkedList`的异同

    * 二者都线程不安全，相对线程安全的`Vector`，执行效率高。

    * `ArrayList`是实现了基于动态数组的数据结构，`LinkedList`基于链表的数据结构。
    * 对于随机访问`get`和`set`，`ArrayList`觉得优于`LinkedList`，因为`LinkedList`要移动指针。
    * 对于新增和删除操作`add`(指定位置)和`remove`，`LinkedList`比较占优势，因为`ArrayList`要移动数据。

  * `ArrayList`和`Vector`的区别

    * `Vector`和`ArrayList`几乎是完全相同的,唯一的区别在于`Vector`是同步类(`synchronized`)，属于强同步类。因此开销就比`ArrayList`要大，访问要慢。正常情况下,使用`ArrayList`而不是`Vector`,因为同步完全可以由自己来控制。
    * `Vector`每次扩容请求其大 小的2倍空间，而`ArrayList`是1.5倍。
    * `Vector`还有一个子类`Stack`。

## Set接口

* 存储无序、不可重复的数据。

### HashSet类

* `HashSet`不是线程安全的。
* 集合元素可以是 `null`。

* `HashSet` 集合判断两个元素相等的标准：
  * 两个对象的 `hashCode()` 方法返回值相等。
  * 两个对象的 `equals()` 方法返回值为`true`。

* 对于存放在`Set`容器中的对象，对应的类一定要重写`equals()`和`hashCode()`方法，以实现对象相等规则。即：“相等的对象必须具有相等的散列码”。

#### 原理分析

* 底层是用`HashMap`存储的。

* `HashSet`中的添加、删除等操作都是调用`HashMap`的`put()`方法实现的，其中在进行添加操作时，把需要添加的值作为`Key`传入`HashMap`的`put()`方法，`Value`传入默认值(`Object`类型的常量)。

* `HashMap`的添加原理请转到`HashMap`中查看。

  ```java
  //HashSet添加操作源代码
  private static final Object PRESENT = new Object();//默认值常量
  public boolean add(E e) {
          return map.put(e, PRESENT)==null;
      }
  ```

#### 重写equals()和HashCode()的原则

* 在程序运行时，同一个对象多次调用 `hashCode()` 方法应该返回相同的值。
* 当两个对象的 `equals()` 方法比较返回 `true` 时，这两个对象的 `hashCode()` 方法的返回值也应相等。
* 对象中用作 `equals()` 方法比较的 `Field`(字段：对象属性等)，都应该用来计算 `hashCode` 值。

### LinkedHashSet类

* `LinkedHashSet` 是 `HashSet` 的子类。
* `LinkedHashSet` 根据元素的 `hashCode` 值来决定元素的存储位置， 但它同时使用双向链表维护元素的次序，这使得元素看起来是以插入 顺序保存的。
* `LinkedHashSet`插入性能略低于 `HashSet`，但在迭代访问 `Set` 里的全部元素时有很好的性能。
* `LinkedHashSet` 不允许集合元素重复。

### TreeSet类

* `TreeSet` 是 `SortedSet` 接口的实现类，`TreeSet` 可以确保集合元素处于排序状态。
* `TreeSet`底层使用红黑树结构存储数据。
* 只能向`TreeSet`中添加类型相同的对象，否则发生`ClassCastException`异常。
* 有序，查询速度比`List`快。

#### 排序

* 自然排序
  * `TreeSet` 会调用集合元素的 `compareTo(Object obj)` 方法来比较元素之间的大小关系，然后将集合元素按升序(默认情况)排列。
  * 试图把一个对象添加到 `TreeSet` 时，则该对象的类必须实现 `Comparable` 接口并实现`compareTo(Object obj)` 方法，两个对象即通过此方法的返回值来比较大小。
* 定制排序
  * 如果元素所属的类没有实现`Comparable`接口，则通过实现`Comparator`接口并重写`compare()`方法。并把`Comparator`接口的实现类的实例作为参数传入`TreeSet` 构造器中创建`TreeSet` 对象。
  * 判断两个元素相等的标准是：比较两个元素返回值为0。

#### 新增方法(了解)

* `Comparator comparator()`
* `Object first()`
* `Object last()`
* `Object lower(Object e)`
* `Object higher(Object e)`
* `SortedSet subSet(fromElement, toElement)`
* `SortedSet headSet(toElement)`
* `SortedSet tailSet(fromElement)`

# Map接口

* 双列集合，用来存储一对(key - value)一对的数据。

## 常用方法

* `V put(K key, V value)`：将指定`key-value`添加到(或修改)当前`map`对象中。
* `void putAll(Map<? extends K, ? extends V> m)`：将m中的所有`key-value`对存放到当前`map`中。
* `V get(Object key)`：获取指定`key`对应的`value`。
* `V remove(Object key)`：移除指定`key`的`key-value`对，并返回`value`。
* `void clear()`：清空当前`map`中的所有数据。
* `boolean containsKey(Object key)`：集合中是否包含指定的`key`。
* `boolean containsValue(Object value)`：集合中是否包含指定的`value`。
* `int size()`：返回`map`中`key-value`对的个数。
* `boolean isEmpty()`：判断当前`map`是否为空。
* `boolean equals(Object o)`：判断当前`map`和参数对象`obj`是否相等。
* `Set<K> keySet()`：返回所有`key`构成的`Set`集合。
* `Collection<V> values()`：返回所有`value`构成的`Collection`集合。
* `Set<Map.Entry<K, V>> entrySet(`)：返回所有`key-value`对构成的Set集合

## HashMap类

### 原理分析

* `HashMap`中是用一个`Node`数组来存储的(jdk7时用`Entry`数组)：

  * 每个数组元素就是一条链表结构。

  ```java
  static class Node<K,V> implements Map.Entry<K,V> {
          final int hash;
          final K key;
          V value;
          Node<K,V> next;//指向下一个元素
      ...
  }
  ```

* 在jdk8调用`HashMap`默认构造器是，不会创建数组，只有在第一次添加元素的时候才会创建一个容量为16的数组；jak8之前调用默认构造器时则会先创建容量16的数组。

* 在进行添加操作时，会调用Key中的`hashCode()` 得到hash值，再通过算法确定`Key`在数组中存储的位置。

  * 如果当前位置的为空，则把`Key`和`Value`作为`Node<K,V>`构造器的参数来创建一个对象节点，并把该对象节点存储到数组当前位置。

  * 如果当前位置不为空，则与当前数组位置的元素进行hash值比较：

    * 如果相等则使用`equals()`进行比较：

      * 如果相等则把新的`Value`赋值给原位置上的元素的`Value`，并返回被修改前的`Value`。

      * 如果不相等，jdk8之前和jdk8的操作有所不同，jdk8则是先判断当前位置是否是红黑树的形式存储的，如果是则按照红黑树的方式添加，否则(jdk8之前直接到这一步)就与当前位置的链表中所有的元素逐个进行hash值比较：

        * 如果相等则进行`equals()`比较，相等则修改`Value`值并返回被修改前的`Value`，否则继续下一个，以此类推。

        * 如果都不相等就把元素添加(jdk8是直接添加到末尾，jdk8之前是先指向当前数组位置的元素(指向链表的表头)，再把添加进来的元素赋值给数组的当前位置)。

    * 如果不相等，按照hash值比较相等时`equals()`不相等的情况处理(jdk8之前和jdk8的操作有所不同......)

* `HashMap`中的其他操作的原理与添加的原理类似。

### 重要常量

* `DEFAULT_INITIAL_CAPACITY` : HashMap的默认容量，16 。
* `MAXIMUM_CAPACITY` ： HashMap的最大支持容量，2^30 。
* `DEFAULT_LOAD_FACTOR`：HashMap的默认加载因子 。
* `TREEIFY_THRESHOLD`：Bucket(桶)中链表长度大于该默认值，转化为红黑树 。
* `UNTREEIFY_THRESHOLD`：Bucket中红黑树存储的Node小于该默认值，转化为链表 。
* `MIN_TREEIFY_CAPACITY`：Bucket中的Node被树化时最小的hash表容量。（当Bucket中Node的数量大到需要变红黑树时，若hash表容量小于`MIN_TREEIFY_CAPACITY`时，此时应执行 resize扩容操作，这个`MIN_TREEIFY_CAPACITY`的值至少是`TREEIFY_THRESHOLD`的4 倍）。
*  `table`：存储元素的数组，总是2的n次幂 。
* `entrySet`：存储具体元素的集 。
* `size`：HashMap中存储的键值对的数量 。
* `modCount`：HashMap扩容和结构改变的次数。 
* `threshold`：扩容的临界值，=容量*填充因子。
*  `loadFactor`：填充因子。

## LinkedHashMap类

* `LinkedHashMap` 是 `HashMap` 的子类。
* 在`HashMap`存储结构的基础上，使用了一对双向链表来记录添加 元素的顺序。
* 与`LinkedHashSet`类似，`LinkedHashMap` 可以维护 `Map` 的迭代顺序：迭代顺序与 `Key-Value` 对的插入顺序一致。

## TreeMap类

* `TreeMap`存储 `Key-Value` 对时，需要根据 `key-value` 对进行排序。 `TreeMap` 可以保证所有的 `Key-Value` 对处于有序状态。
* `TreeMap`底层使用红黑树结构存储数据。
* TreeMap 的 Key 的排序：
  * 自然排序：`TreeMap` 的所有的 `Key` 必须实现 `Comparable` 接口，而且所有的 `Key` 应该是同一个类的对象，否则将会抛出 `ClasssCastException` 。
  * 定制排序：创建 `TreeMap` 时，传入一个 `Comparator` 对象，该对象负责对 `TreeMap` 中的所有 `key` 进行排序。此时不需要 `Map` 的 `Key` 实现 `Comparable` 接口。

* `TreeMap`判断两个`key`相等的标准：两个`key`通过`compareTo()`方法或者`compare()`方法返回0。

## Hashtable类

* Hashtable是个古老的 Map 实现类，JDK1.0就提供了。不同于HashMap， Hashtable是线程安全的。 
* Hashtable实现原理和HashMap相同，功能相同。底层都使用哈希表结构，查询速度快，很多情况下可以互用。
* 与HashMap不同，Hashtable 不允许使用 null 作为 key 和 value。
* 与HashMap一样，Hashtable 也不能保证其中 Key-Value 对的顺序 。
* Hashtable判断两个key相等、两个value相等的标准，与HashMap一致。

### Properties类

* `Properties` 类是 `Hashtable` 的子类，该对象用于处理属性文件。
* 由于属性文件里的 `key`、`value` 都是字符串类型，所以 `Properties` 里的 `key` 和 `value` 都是字符串类型。
* 存取数据时，建议使用`setProperty(String key,String value)`方法和 `getProperty(String key)`方法。

#### 常用方法

* `public Object setProperty(String key,String value)`：存储数据。
* `public String getProperty(String key)`：获取数据。

# collections工具类

* `Collections` 是一个操作 `Set`、`List` 和 `Map` 等集合的工具类。

## 常用方法

* `public static <T extends Comparable<? super T>> void sort(List<T> list)`：
* `public static <T> void sort(List<T> list, Comparator<? super T> c)`：根据元素的自然顺序对指定 `List` 集合元素按升序排序。
* `public static void reverse(List<?> list)`：根据指定的 `Comparator` 产生的顺序对 List 集合元素进行排序。
* `public static void shuffle(List<?> list)`：对 `List` 集合元素进行随机排序。
* `public static void swap(List<?> list, int i, int j)`：将指定 `list` 集合中的 `i` 处元素和 `j` 处元素进行交换。
* `public static <T> void fill(List<? super T> list, T obj)`：用指定的元素替换指定列表中的所有元素。
* `public static <T> void copy(List<? super T> dest, List<? extends T> src)`：将`src`中的内容复制到`dest`中，注意`dest`集合的`size`属性必须大于或等于`src`的`size`。
* `public static <T extends Object & Comparable<? super T>> T min(Collection<? extends T> coll)`：返回自然排序中的最小值。
* `public static <T> T min(Collection<? extends T> coll, Comparator<? super T> comp)`：返回定制排序中的最小值。
* `public static <T extends Object & Comparable<? super T>> T max(Collection<? extends T> coll)`：返回自然排序中的最大值。
* `public static <T> T max(Collection<? extends T> coll, Comparator<? super T> comp)`：返回定制排序中的最大值。
* `public static <T> boolean replaceAll(List<T> list, T oldVal, T newVal)`：使用`oldVal`替换 `List` 中的所有的`oldVal`。
* `public static int frequency(Collection<?> c, Object o)`：返回指定集合中指定元素的出现次数

## 同步控制方法

* `Collections` 类中提供了多个 `synchronizedXxx`() 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题。
  * `public static <T> Collection<T> synchronizedCollection(Collection<T> c)`
  * `public static <T> List<T> synchronizedList(List<T> list)`
  * `public static <T> Set<T> synchronizedSet(Set<T> s)`
  * `public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)`

