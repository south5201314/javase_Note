[TOC]

# 异常体系

* `java.lang.Throwable`
  * `java.lang.Error`:一般不编写针对性代码进行处理
  * `java.lang.Exception`:可以进行异常的处理
    * 编译时异常(checked)
      * `IOException`
        * `FileNotFoundException`
      * `ClassNotFoundException`
    * 运行时异常(unchecked)
      * `NullPointerException`
      * `ArrayIndexOutOfBoundsException`
      * `ClassCastException`
      * `InputMisMatchException`
      * `ArithmeticException`

# 常见异常

* `java.lang.NullPointerException`：空指针异常

  ```java
  public class T {
  	public static void main(String agrs[]) {
  		int [] arr = null;//对象为空
  		System.out.println(arr[0]);
  	}
  }
  ```

* `java.lang.ArrayIndexOutOfBoundsException`:数组越界

  ```java
  public class T {
  	public static void main(String agrs[]) {
  		int [] arr = new int[3];
  		System.out.println(arr[10]);
  	}
  }
  ```

* `java.lang.ClassCastException`:类型转换异常

  ```java
  public class T {
  	public static void main(String agrs[]) {
  		Object obj = new Date();
  		String str = (String)obj;
  	}
  }
  ```

* `java.lang.NumberFormatException`:数值转换异常

  ```java
  public class T {
  	public static void main(String agrs[]) {
  		String str = "abc";
  		int num = Integer.parseInt(str);
  	}
  }
  ```

* `java.util.InputMismatchException`:输入类型不匹配异常

  ```java
  public class T {
  	public static void main(String agrs[]) {
  		Scanner scanner = new Scanner(System.in);
  		int score = scanner.nextInt(); 
  	}
  }
  ```

* `java.lang.ArithmeticException`:算数异常

  ```java
  public class T {
  	public static void main(String agrs[]) {
  		int tep = 10/0; 
  	}
  }
  ```

# 异常处理机制

* `try-catch-finally`

  * 运行时异常一般不进行处理
  * 编译时异常一定要进行处理，不然编译无法通过

  ```java
  ...
      try{
          //出现异常的代码
      }catch(NumberFormatException`e1){
          //匹配异常对象的类型进行处理
          ...     
      }catch(Exception e2){
          //匹配异常对象的类型进行处理
          ...
      }finally{
          //一定会执行的代码
          //finally部分是可选的,catch代码中再出现异常，
          //try和catch中有return语句，finally中的代码也会被执行
          //finally中的代码执行优先于return语句
          ... 
      }
  ...
  ```

* `throws`

  * `throws` + 异常类型 写在声明处，指此方法执行时，可能为抛出的异常类型
  * 方法体执行时，出现异常会产生一个异常类的对象，此对象满足`throws`类型的对象时，就会被抛出
  * 出现异常代码后面的代码就不会被执行，方法结束

  ```java
  public class T {
  	public static void main(String agrs[]) {
  		try {
  			Test();
  		} catch (FileNotFoundException e) {
  			e.printStackTrace();
  		} catch (IOException e) {
  			e.printStackTrace();
  		}
  	}
      
  	public static void Test() throws FileNotFoundException,IOException {
  		FileInputStream fileInputStream = new FileInputStream(new File("1.txt"));
  		fileInputStream.close();
  		System.out.println("以上代码出现异常，这里和以下代码就不会被执行，方法结束");
  	}
  }
  ```

* 开发中如何选择使用 `try-catch-finally` 还是 `throws`
  * 子类中重写父类的方法中没有throws方式处理异常，则子类重写的方法也不能使用throws，意味着子类重写的方法中也异常，必须使用`try-catch-finally`方式处理异常
  * 如果方法A中调用了其他三个方法，这三个方法是递归的方式执行的(例如：前一个方法的返回值作为下一个方法的参数传入)，那么这三个方法中最好不要使用`try-catch-finally`的方式进行异常处理，应该使用throws的方式进行抛出，让方法A使用`try-catch-finally`进行异常处理

* 手动抛出异常

  ```java
  public class T {
  	public static void main(String agrs[]) {
  		try {
  			new T().Test(0);
  		} catch (Exception e) {
  			System.out.println(e.getMessage());
  		}
  	}
  	
  	public int Test(int id) throws Exception  {
  		if(id != 0) {
  			return 10/id;
  		}else {
  			throw new Exception("输入的数值不能为0");//创建一个异常对象并抛出(throw)
  		}
  	}
  }
  ```

  

# 自定义异常类

* 继承于现有的异常结构：`RuntimeException`、`Exception`
* 提供全局常量：`serialVersionUID`（类的序列号，用于标识这个异常类）
* 提供重载的构造器

```java
MyException.class
    
public class MyException extends Exception {
	public MyException() {
        ...
	}
	public MyException(String msg) {
		super(msg);
        ...
	}
}
```

```java
T.class
    
public class T {
	public static void main(String agrs[]) {
		try {
			new T().Test(0);
		} catch (MyException e) {
			System.out.println(e.getMessage());
		}
	}
	
	public int Test(int id) throws MyException  {
		if(id != 0) {
			return 10/id;
		}else {
			throw new MyException("输入的数值不能为0");
		}
	}
}
```

