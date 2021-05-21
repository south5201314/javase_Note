# 字符串相关类

## String类

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    private final char value[];
    ...
}
```

### 常用方法

* `public int length()`：返回字符串的长度`return value.length`

* `public char charAt(int index)`：返回某索引处的字符`return value[index]`

* `public boolean isEmpty()`：判断是否是空字符串`return value.length == 0`

* `public String toLowerCase()`：使用默认语言环境，将 `String` 中的所有字符转换为小写

* `public String toUpperCase()`：使用默认语言环境，将 `String` 中的所有字符转换为大写

* `public String trim()`：返回字符串的副本，忽略前导空白和尾部空白

* `public boolean equals(Object anObject)`：比较字符串的内容是否相同

* `public boolean equalsIgnoreCase(String anotherString)`：与`equals`方法类似，忽略大小写

* `public String concat(String str)`：将指定字符串连接到此字符串的结尾。 等价于用“+”

* `public int compareTo(String anotherString)`：比较两个字符串的大小

* `public String substring(int beginIndex)`：返回一个从`beginIndex`开始截取到最后的子字符串

* `public String substring(int beginIndex, int endIndex)`：返回从`beginIndex`开始截取到`endIndex`(不包含)的子字符串

* `public boolean endsWith(String suffix)`:是否以指定的后缀结束

* `public boolean startsWith(String prefix)`:是否以指定的前缀开始

* `public boolean startsWith(String prefix, int toffset)`:从指定索引开始的子字符串是否以指定前缀开始

* `public boolean contains(CharSequence s)`:是否包含s

* `public int indexOf(String str)`：子字符串在此字符串中第一次出现处的索引

* `public int indexOf(String str, int fromIndex)`：子字符串在此字符串中第一次出现处的索引，从指定索引开始

* `public int lastIndexOf(String str)`：子字符串在此字符串最右边出现处的索引

* `public int lastIndexOf(String str, int fromIndex)`：子字符串在此字符串最右边出现处的索引，从指定的索引开始往左搜索

  `indexOf`和`lastIndexOf`方法如果未找到都是返回-1

* `public String replace(char oldChar, char newChar`)：通过用 `newChar` 替换此字符串中所有 `oldChar` 并返回一个新字符串
* `public String replace(CharSequence target, CharSequence replacement)`：通过字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串
* `public String replaceAll(String regex, String replacement)`：使用`replacement` 替换此字符串中所有与正则表达式匹配的子字符串
* `public String replaceFirst(String regex, String replacement)`：使用`replacement` 替换此字符串中一个与正则表达式匹配的子字符串
* `public boolean matches(String regex)`：此字符串是否与正则表达式匹配
* `public String[] split(String regex)`：通过正则表达式的匹配拆分此字符串
* `public String[] split(String regex, int limit)`：通过正则表达式的匹配拆分此字符串，最多不超过limit个，如果超过了，剩下的全部都放到最后一个元素中

### String与基本数据类型转换

* 字符串转换基本数据类型、包装类

  调用`public static xxx parsXxx(String s)`：可以将字符串转换为基本数据类型、包装类

* 基本数据类型、包装类转换字符串

  调用String类的`public String valueOf(xxx value)`： 可以将基本数据类型、包装类转换为字符串

### String与字符数组转换

* 字符数组转换字符串

  通过`public String(char value[])`、`public String(char value[], int offset, int count)`构造器分别将数组中的全部字符和部分字符创建为`String`对象

* 字符串转换字符数组
  * `public char[] toCharArray()`：将字符串中的全部字符存放在一个字符数组中并返回
  * `public void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)`：将指定索引范围内的字符串存放到字符数组中

### String与字节数组转换

* 字节数组转换字符串

  通过`public String(byte bytes[])`、`public String(byte bytes[], int offset, int length)` 构造器分别将数组中的全部字节和部分(`offset`开始取`length`个字节)字节创建为`String`对象

* 字符串转换字节数组
  * `public byte[] getBytes()` ：将此 String 编码为 `byte` 序列储到一个新的 `byte` 数组中并返回
  * `public byte[] getBytes(String charsetName)` ：使用指定的字符集将此 `String` 编码为`byte` 序列储到一个新的 `byte` 数组中并返回

## StringBuffer类

```
public final class StringBuffer
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence
```

### 构造器

* `public StringBuffer()`：初始容量为16的字符串缓冲区
* `public StringBuffer(int capacity)`：指定容量的字符串缓冲区
* `public StringBuffer(String str)` ：将内容初始化为指定字符串内容，容量为`str.length()`+16

### 常用方法

* `public AbstractStringBuilder append(Xxx xxx)`:用于进行字符串拼接
* `public synchronized StringBuffer delete(int start, int end)`:删除指定位置的内容
* `public synchronized StringBuffer replace(int start, int end, String str)`:把[start,end)位置替换为`str`
* `public AbstractStringBuilder insert(int offset, Xxx xxx)`:在指定位置插入`xxx`
* `public synchronized StringBuffer reverse()`:把当前字符序列逆转
* `public int indexOf(String str)` 
* `public String substring(int start,int end)` 
* `public int length() public char charAt(int n )` 
* `public void setCharAt(int n ,char ch)`

## StringBuilder类

* `StringBuilder` 和 `StringBuffer` 非常类似，均代表可变的字符序列，而且 提供相关功能的方法也一样

* 面试题：对比`String`、`StringBuffer`、`StringBuilder`

  * `String(JDK1.0)`：不可变字符序列

  * `StringBuffer(JDK1.0)`：可变字符序列、效率低、线程安全

  * `StringBuilder(JDK 5.0)`：可变字符序列、效率高、线程不安全

 注意：作为参数传递的话，方法内部String不会改变其值，StringBuffer和StringBuilder 会改变其值

# 日期时间相关类

## JDK 8之前

### System类

`public static long currentTimeMillis()`：返回当前时 间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差

### Date类

* 构造器
  * `public Date()`：创建的对象可以获取本地当前时间
  * `public Date(long date)`：创建指定毫秒数的对象

* 常用方法
  * `public long getTime()`：返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 `Date` 对象 表示的毫秒数
  * `public String toString()`：把此 `Date` 对象转换为以下形式的 String： dow mon dd hh:mm:ss zzz yyyy 其中： dow 是一周中的某一天 (Sun, Mon, Tue,Wed, Thu, Fri, Sat)，zzz是时间标准

### SimpleDateFormat类

定义：`public class SimpleDateFormat extends DateFormat`

* 构造器
  * `public SimpleDateFormat()`：默认的模式和语言环境创建对象
  * `public SimpleDateFormat(String pattern)`：指定的格式创建对象方法

* public String format(Date date)：格式化时间对象并返回此字符串

* public Date parse(String source)：从给定字符串的开始解析文本，以生成 一个Date对象

```java
public class SimpleDateFormatTest {
    public static void main(String[] args) {
        //默认的模式和语言环境创建对象
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat();
        Date date = new Date();
        String dateStr= simpleDateFormat.format(date);
        System.out.println(dateStr);//21-2-18 下午1:49
        //指定的格式创建对象方法
        SimpleDateFormat simpleDateFormat1 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
        String formatStr = simpleDateFormat1.format(date);
        System.out.println(formatStr);//2021-02-18 01:49:22
        //从给定字符串的开始解析文本，以生成 一个Date对象
        try {
            Date date1 = simpleDateFormat1.parse("2021-03-18 01:50:22");
            System.out.println(date1);//Thu Mar 18 01:50:22 CST 2021
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
}
```

![时间日期字母表](https://revenge-img.oss-cn-guangzhou.aliyuncs.com/img/%E6%97%B6%E9%97%B4%E6%97%A5%E6%9C%9F%E5%AD%97%E6%AF%8D%E8%A1%A8.png)

### Calendar类(日期类)

`Calendar`是一个抽象基类，主用用于完成日期字段之间相互操作的功能。

* `Calendar`实例的方法
  * 使用`public static Calendar getInstance()`方法
  * 调用它的子类`public GregorianCalendar()`构造器

* 常用方法：
  * `public void git(int field)`：通过field获取时间的信息
  * `public void set(int field,int value)`：通过field设置时间的信息
    * `field：`
      * `public final static int YEAR = 1;`
      * `public final static int MONTH = 2;`
      * `public final static int WEEK_OF_YEAR = 3;`
      * `public final static int WEEK_OF_MONTH = 4;`
      * `...`
  * `public final Date getTime()`：获取Date对象
  * `public final void setTime(Date date)`：设置Data对象为当前日期类的时间

## JDK 8之后

java.time – 包含值对象的基础包
java.time.chrono – 提供对不同的日历系统的访问
java.time.format – 格式化和解析时间和日期
java.time.temporal – 包括底层框架和扩展特性
java.time.zone – 包含时区支持的类

说明：大多数开发者只会用到基础包和format包，也可能会用到temporal包。因此，尽
管有68个新的公开类型，大多数开发者，大概将只会用到其中的三分之一。

### LocalDate、LocalTime、LocalDateTime类

实例 是不可变的对象，分别表示使用 ISO-8601日历系统的日期、时间、日期和时间。 它们提供了简单的本地日期或时间，并不包含当前的时间信息，也不包含与时区 相关的信息。

* `LocalDate`代表IOS格式（yyyy-MM-dd）的日期,可以存储 生日、纪念日等日期。
* `LocalTime`表示一个时间，而不是日期。
* `LocalDateTime`是用来表示日期和时间的，这是一个最常用的类之一。

#### 常用方法

* `public static LocalDate now()`：根据当前时间创建对象
* `public static LocalDate now(ZoneId zone)`：根据指定时区创建对象
* `public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second)`：根据指定日期/时间创建对象
* `public Xxx getXxx()`:获取日期信息(月份天数(1-31) 、年份天数(1-366)、月份Month 枚举值、小时、分钟、秒)
* `public LocalDateTime withXxx(int Xxx)`：月份天数、年份天数、月份、年份修改为指定的值并返回新的对象
* `public LocalDateTime plusXxx(long Xxx)`：向当前对象添加几天、几周、几个月、几年、几小时并返回新的对象
* `public LocalDateTime minusXxx(long Xxx)`:从当前对象减去几月、几周、几天、几年、几小时并返回新的对象

```java
public class LocalDateTest {
    public static void main(String[] args) {
        //创建对象
        LocalDate localDate = LocalDate.now();
        LocalTime localTime = LocalTime.now();
        LocalDateTime localDateTime = LocalDateTime.now();
        LocalDateTime localDateTime1 = LocalDateTime.of(2020, 12, 5, 15, 25, 57);
        //获取日期信息
        System.out.println(localDateTime1.getDayOfYear());//340
        System.out.println(localDateTime1.getMonth());//DECEMBER
        System.out.println(localDateTime1.getMonthValue());//12
        //设置值并返回一个新的对象，原有的对象不会被改变，不可变性
        LocalDateTime localDateTime2 = localDateTime.withDayOfMonth(25);
        System.out.println(localDateTime);//2021-02-18T14:57:42.747
        System.out.println(localDateTime2);//2021-02-25T14:57:42.747
        //添加
        LocalDateTime localDateTime3 = localDateTime.plusDays(10);
        System.out.println(localDateTime);//2021-02-18T15:01:00.036
        System.out.println(localDateTime3);//2021-02-28T15:01:00.036
        //减去
        LocalDateTime localDateTime4 = localDateTime.minusMonths(3);
        System.out.println(localDateTime);//2021-02-18T15:03:11.811
        System.out.println(localDateTime4);//2020-11-18T15:03:11.811
    }
}
```

### Instant类(瞬时)

* 时间线上的一个瞬时点。 这可能被用来记录应用程序中的事件时间戳。
* `Instant`的精度可以达到纳秒级。

#### 常用方法

* `public static Instant now()`：返回默认UTC时区的Instant类的对象
* `public static Instant ofEpochSecond(long epochSecond)` ：返回在1970-01-01 00:00:00基础上加上指定毫秒数之后的Instant类的对象
* `public OffsetDateTime atOffset(ZoneOffset offset)`：结合即时的偏移来创建一个 OffsetDateTime
* `public long toEpochMilli()`：返回1970-01-01 00:00:00到当前时间的毫秒数，即为时间戳
  * 时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01 日08时00分00秒)起至现在的总秒数。

### DateTimeFormatter类

* 预定义的标准格式

  * `public static final DateTimeFormatter ISO_LOCAL_DATE;`
  * `public static final DateTimeFormatter ISO_LOCAL_TIME;`
  * `public static final DateTimeFormatter ISO_LOCAL_DATE_TIME;`

* 本地化相关的格式

  * `public static DateTimeFormatter ofLocalizedDateTime(FormatStyle dateTimeStyle)`

    ```java
    public enum FormatStyle {
        FULL,
        LONG,
        MEDIUM,
        SHORT;
    }
    ```

* 自定义的格式

  * `public static DateTimeFormatter ofPattern(String pattern)`:返 回 一 个 指 定 字 符 串 格 式 的`DateTimeFormatter`

* 常用方法
  * `public String format(TemporalAccessor temporal)`：格式化一个日期、时间，返回字符串
  * `public TemporalAccessor parse(CharSequence text`)：将指定格式的字符序列解析为一个日期、时间

# Java比较器

## Comparable接口

* `java.lang.Comparable`接口强行对实现它的每个类的对象进行整体排序。
* 实现 `java.lang.Comparable`的类必须实现 `compareTo(Object obj)` 方法，如果当前对象this大于形参对象obj，则返回正整数，如果当前对象this小于形参对象obj，则返回负整数，如果当前对象this等于形参对象obj，则返回零。
* 实现`java.lang.Comparable`接口的对象列表（和数组）可以通过 `Collections.sort()` 或`Arrays.sort()`进行自动排序。实现此接口的对象可以用作有序映射中的键或有序集合中的元素，无需指定比较器。

## Comparator接口

* 当元素的类型没有实现`java.lang.Comparable`接口而又不方便修改代码，或者`java.lang.Comparable`接口的排序规则不适合当前的操作，那么可以考虑使用 `Comparator` 的对象来排序。
* 重写`compare(Object o1,Object o2)`方法，比较o1和o2的大小：如果方法返回正整数，则表示o1大于o2；如果返回0，表示相等；返回负整数，表示 o1小于o2。
* 可以将 `Comparator` 传递给 `sort()` 方法（如 `Collections.sort()` 或 `Arrays.sort()`），从而允许在排序顺序上实现精确控制。
* 还可以使用 `Comparator` 来控制某些数据结构（如有序 set或有序映射）的 顺序，或者为那些没有自然顺序的对象 `collection` 提供排序。

# System类

## 成员变量

* `in`：标准输入流 (键盘输入)

* `out`：标准输出流(显示器)

* err：标准错误输出流(显示器)
## 成员方法

* `public static native long currentTimeMillis()`：返回当前的计算机时间
* `public static void exit(int status)`：退出程序。其中`status`的值为0代表正常退出，非零代表异常退出。使用该方法可以在图形界面编程中实现程序的退出功能等。
* `public static void gc()`：请求系统进行垃圾回收。至于系统是否立刻回收，则取决于系统中垃圾回收算法的实现以及系统执行时的情况。
* `public static String getProperty(String key) `：获得系统中属性名为key的属性对应的值。系统中常见 的属性名以及属性的作用如下表所示：
  * java.version:java运行环境版本
  * java.home:java安装目录
  * os.name:操作系统的名称
  * os.version:操作系统的版本
  * user.nama:用户的账户名称
  * user.home:用户的主目录
  * user.dir:用户的当前工作目录

# Math类

java.lang.Math提供了一系列静态方法用于科学计算。其方法的参数和返回
值类型一般为double型。

## 常用方法

* `public static Xxx abs(Xxx a)`：绝对值
* `public static Xxx max(Xxx a, Xxx b)`：最大值
* `public static Xxx min(Xxx a, Xxx b)`：最小值
* `public static double sin(double a)`：三角函数
* `public static double toRadians(double angdeg)`：角度—>弧度
* `public static double toDegrees(double angrad)`：弧度—>角度
* `public static double random()`：返回0.0到1.0的随机数
* `public static double pow(double a, double b)`：a的b次幂

# BigInteger与BigDecimal

## BigInteger类

* BigInteger可以表示不可变的任意精度的整数。
* BigInteger 还提供模算术、GCD 计算、质数测试、素数生成、 位操作以及一些其他操作。

### 构造器

* `public BigInteger(String val)`：根据字符串构建BigInteger对象

### 常用方法

* `public BigInteger abs()`：返回此 BigInteger 的绝对值的BigInteger。
* `BigInteger add(BigInteger val)` ：返回其值为 (this + val) 的BigInteger
* `BigInteger subtract(BigInteger val)` ：返回其值为 (this - val) 的BigInteger
* `BigInteger multiply(BigInteger val)` ：返回其值为 (this * val) 的BigInteger
* `BigInteger divide(BigInteger val)` ：返回其值为 (this / val) 的BigInteger。整数相除只保留整数部
* `BigInteger remainder(BigInteger val)` ：返回其值为 (this % val) 的BigInteger。
* `BigInteger[] divideAndRemainder(BigInteger val)`：返回包含 (this / val) 后跟(this % val) 的两个的BigInteger数组。
* `BigInteger pow(int exponent)` ：返回其值为 (this的exponent次幂) 的 BigInteger。
* `...`其他方请查看文档或者翻译源代码

## BigDecimal类

* 一般的Float类和Double类可以用来做科学计算或工程计算，但在商业计算中， 要求数字精度比较高，故用到java.math.BigDecimal类。
* BigDecimal类支持不可变的、任意精度的有符号十进制定点数。

### 构造器

* `public BigDecimal(double val)`
* `public BigDecimal(String val)`

### 常用方法

* `public BigDecimal add(BigDecimal augend)`：加
* `public BigDecimal subtract(BigDecimal subtrahend)`：减
* `public BigDecimal multiply(BigDecimal multiplicand)`：乘
* `public BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)`：除
* `...`其他方法参考`BigInteger`类的方法或者查看文档