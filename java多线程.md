[TOC]

# 线程的创建和使用

## 线程的创建方式

### 继承于Thread类

1. 创建一个继承于Thread类的子类

2. 重写Thread类的run()方法

3. 创建Thread类的子类的对象

4. 通过此对象的调用start()方法启动线程

   ```java
   public class ThreadTest {
       public static void main(String[] args) {
           MyThread myThread = new MyThread();
           myThread.start();
           //通过匿名子类的方式
           new Thread(){
               @Override
               public void run() {
                   for (int i = 0;i < 100; i++){
                       if(i % 2 != 0){
                           System.out.println(i);
                       }
                   }
               }
           }.start();
           //以下的代码还是在主线程执行
           ...
       }
   }
   class MyThread extends Thread{
       @Override
       public void run() {
           for (int i = 0;i < 100; i++){
               if(i % 2 == 0){
                   System.out.println(i);
               }
           }
       }
   }
   ```

   5. 注意：
      * 通过对象调用`start()`会启动线程和调用线程中的`run()`
      * 不能通过直接调用`run()`来启动线程
      * 已经`start()`的线程不能再去`start()`，否则会报`java.lang.IllegalThreadStateException`

### 实现Runnable接口

1. 声明一个实现Runnable接口的类

2. 实现`run()`抽象方法

3. 创建实现类的对象

4. 将实现类的对象传入Thread类的构造器中，创建Thread类的对象

5. 通过Thread类的对象调用`start()`

   ```java
   public class ThreadTest {
       public static void main(String[] args) {
           MyRunnable myRunnable = new MyRunnable();
           Thread thread = new Thread(myRunnable);
           thread.start();
           //以下代码主线程执行
           for (int i = 0;i < 100; i++){
               if(i % 2 != 0){
                   System.out.println(Thread.currentThread().getName()+":"+i);
               }
           }
       }
   }
   
   class MyRunnable implements Runnable {
       @Override
       public void run() {
           for (int i = 0;i < 100; i++){
               if(i % 2 == 0){
                   System.out.println(Thread.currentThread().getName()+":"+i);
               }
           }
       }
   }
   ```

   原理：

   ```java
   public
   class Thread implements Runnable {
       private Runnable target;
       
       public Thread(Runnable target){
           //把形参target赋值给属性
           ...
       }
       
       @Override
       public void run() {
           if (target != null) {
               target.run();
           }
       }
   ```

### 对比两种创建方式

* 优先选择实现Runnable接口的方式
  * 实现的方式没有类的单继承的局限性
  * 实现的方式更适合来处理多个线程共享数据的情况

* 两种方式的联系性
  * `public class Thread implements Runnable`
  * 都需要实现(重写)`run()`

## Thread类的常用方法

* `public synchronized void start()`：启动当前线程，调用当前线程的`run()`
* `public void run()`：通常需要重写Thread类中的此方法，将创建的线程要执行的操作声明在此方法中
* `public static native Thread currentThread()`：静态、本地方法，返回执行当前代码的线程
* `public final String getName()`：获取当前线程的名字
* `public final synchronized void setName(String name)`：设置当前线程的名字
* `public static native void yield()`：释放CPU的执行权
* `public final void join() throws InterruptedException`：在线程A中调用线程B的join()，此时线程B就会进入阻塞状态，直到线程B执行完毕，线程A才结束阻塞状态
* `public final void stop()`：强制停止线程，此方法已被抛弃，不建议使用
* `public static native void sleep(long millis) throws InterruptedException`：强制线程进入阻塞状态，阻塞时间为`millis`毫秒
* `public final native boolean isAlive()`：判断线程是否存活

## 线程的优先级

`public final static int MIN_PRIORITY = 1;`

`public final static int NORM_PRIORITY = 5;` 默认

`public final static int MAX_PRIORITY = 10;`

`public final int getPriority()`：获取线程的优先级

`public final void setPriority(int newPriority)`：设置线程的优先级，`newPriority`越大优先级越高

```java
public class ThreadTest {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.setPriority(Thread.MAX_PRIORITY);//设置分线程优先级
        Thread.currentThread().setPriority(Thread.MIN_PRIORITY);//设置主线程优先级
        myThread.start();
        //以下代码主线程执行
        for (int i = 0;i < 100; i++){
            if(i % 2 != 0){
                System.out.println(Thread.currentThread().getName()+":"+i);
            }
        }
    }
}
class MyThread extends Thread{
    @Override
    public void run() {
        //setPriority(Thread.MAX_PRIORITY);
        for (int i = 0;i < 100; i++){
            if(i % 2 == 0){
                System.out.println(getName()+":"+i);
            }
        }
    }
}
```

说明：高优先级的线程要抢占低优先级线程的CPU执行权，从概率上讲高优先级高概率被执行，并不意味着高优先级的线程执行完毕之后低优先级的线程才能执行

# 线程的声明周期

![线程的声明周期流程图](https://revenge-img.oss-cn-guangzhou.aliyuncs.com/img/%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%A3%B0%E6%98%8E%E5%91%A8%E6%9C%9F%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

# 线程的同步

## 出现线程安全问题的原因

当多条语句在操作同一个线程共享数据时，一个线程对多条语句只执行了一部分，还没有 执行完，另一个线程参与进来执行。导致共享数据的错误。

## 解决线程安全问题的方法

对多条操作共享数据的语句，只能让一个线程都执行完，在执行过程中，其他线程不可以 参与执行。

### 同步代码块

* 同步代码：操作共享数据的代码。
* 共享数据：多个线程共同操作的数据(变量)。
* 同步监视器(锁)：如何一个类的对象，都可以充当锁。
* 需要解决安全问题的多个线程共用同一把锁。

```java
synchronized(同步监视器){
    //需要同步的代码，操作共享数据的代码
}
```

```java
//实现Runnable接口的方式
public class WindowTest {
    public static void main(String[] args) {
        WindowRunnable windowRunnable = new WindowRunnable();
        Thread thread1 = new Thread(windowRunnable);
        Thread thread2 = new Thread(windowRunnable);
        Thread thread3 = new Thread(windowRunnable);
        thread1.setName("窗口1");
        thread2.setName("窗口2");
        thread3.setName("窗口3");
        thread1.start();
        thread2.start();
        thread3.start();
    }
}

class WindowRunnable implements Runnable {
    private int ticket = 100;
    @Override
    public void run() {
        while (true) {
            synchronized (this) {
                //需要同步的代码
                if (ticket > 0) {
                    try {
                        Thread.sleep(50);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() 
                                       + " 票号为：" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }
            //同步代码结束
        }
    }
}
```

```java
//继承于Thread类的方式
public class WindowTest1 {
    public static void main(String[] args) {
        WindowThread windowThread1 = new WindowThread();
        WindowThread windowThread2 = new WindowThread();
        WindowThread windowThread3 = new WindowThread();
        windowThread1.setName("窗口1");
        windowThread2.setName("窗口2");
        windowThread3.setName("窗口3");
        windowThread1.start();
        windowThread2.start();
        windowThread3.start();
    }
}

class WindowThread extends Thread {
    private static int ticket = 100;
    @Override
    public void run() {
        while (true) {
            synchronized(WindowThread.class) {
                //需要同步的代码
                if (ticket > 0) {
                    try {
                        Thread.sleep(50);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(getName() + " 票号为：" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }
            //同步代码结束
        }
    }
}
```

说明：

* 在实现`Runnable`接口创建多线程的方式中，可以考虑使用`this`充当监视器。
* 在继承Thread类创建多线程方式中，可以考试使用`类名.class`充当监视器。
* 使用`synchronized`时需要注意同步代码的范围。

### 同步方法

* 同步方法仍然涉及到同步监视器，只是不需要显式声明。

* 非静态的同步方法，同步监视器是：`this`

  ```java
  public class WindowTest2 {
      public static void main(String[] args) {
          WindowRunnable2 windowRunnable2 = new WindowRunnable2();
          Thread thread1 = new Thread(windowRunnable2);
          Thread thread2 = new Thread(windowRunnable2);
          Thread thread3 = new Thread(windowRunnable2);
          thread1.setName("窗口1");
          thread2.setName("窗口2");
          thread3.setName("窗口3");
          thread1.start();
          thread2.start();
          thread3.start();
      }
  }
  
  class WindowRunnable2 implements Runnable {
      private int ticket = 100;
      @Override
      public void run() {
          while (SellingTickets());
      }
  
      //非静态同步方法
      public synchronized boolean SellingTickets() {//非静态方法同步监视器：this
          //需要同步的代码
          if (ticket > 0) {
              try {
                  Thread.sleep(50);
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
              System.out.println(Thread.currentThread().getName()
                                 + " 票号为：" + ticket);
              ticket--;
          }else{
              return false;
          }
          return true;
      }
  }
  ```

* 静态方的同步方法，同步监视器是：当前类本身(`类名.class`)

  ```java
  public class WindowTest3 {
      public static void main(String[] args) {
          WindowThread3 windowThread1 = new WindowThread3();
          WindowThread3 windowThread2 = new WindowThread3();
          WindowThread3 windowThread3 = new WindowThread3();
          windowThread1.setName("窗口1");
          windowThread2.setName("窗口2");
          windowThread3.setName("窗口3");
          windowThread1.start();
          windowThread2.start();
          windowThread3.start();
      }
  }
  
  class WindowThread3 extends Thread {
      private static int ticket = 100;
      @Override
      public void run() {
          while (SellingTickets());
      }
  
      public static synchronized boolean SellingTickets() {//静态方法同步监视器：WindowThread3.class
          if (ticket > 0) {
              try {
                  Thread.sleep(50);
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
              System.out.println(Thread.currentThread().getName() 
                                 + " 票号为：" + ticket);
              ticket--;
          }else{
              return false;
          }
          return true;
      }
  }
  ```

说明：

* 实现Runnable接口方式的同步方法一般为非静态方法。
* 继承于Thread类方式的同步方法一般为静态方法。
* 声明为非静态和静态方法取决于同步监视器。

### Lock锁(JDK5.0新增)

1. 实例化`ReentrantLock`
2. 上锁,调用`lock()`
3. 解锁,调用`unlock()`
4. 建议在`finally`中解锁，确保一定会被执行

```java
public class WindowTest4 {
    public static void main(String[] args) {
        WindowRunnable4 windowRunnable4 = new WindowRunnable4();
        Thread thread1 = new Thread(windowRunnable4);
        Thread thread2 = new Thread(windowRunnable4);
        Thread thread3 = new Thread(windowRunnable4);
        thread1.setName("窗口1");
        thread2.setName("窗口2");
        thread3.setName("窗口3");
        thread1.start();
        thread2.start();
        thread3.start();
    }
}

class WindowRunnable4 implements Runnable {
    private int ticket = 100;
    //1.实例化ReentrantLock
    private  ReentrantLock reentrantLock= new ReentrantLock();
    @Override
    public void run() {
        while (SellingTickets());
    }

    //同步方法
    public  boolean SellingTickets() {//非静态方法同步监视器：this
        //2.上锁,调用lock()
        reentrantLock.lock();
        try {
            //需要同步的代码
            if (ticket > 0) {
                try {
                    Thread.sleep(50);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + 
                                   " 票号为：" + ticket);
                ticket--;
            }
        } finally {
            //3.解锁,调用unlock()
            reentrantLock.unlock();
            if(ticket <= 0){
                return false;
            }
        }
        return true;
    }
}
```

### synchronized 与 Lock 的对比

1.  Lock是显式锁（手动开启和关闭锁，别忘记关闭锁），synchronized是
隐式锁，出了作用域自动释放
2. Lock只有代码块锁，synchronized有代码块锁和方法锁
3. 使用Lock锁，JVM将花费较少的时间来调度线程，性能更好。并且具有
    更好的扩展性（提供更多的子类）

#### 优先使用顺序

Lock --> 同步代码块（已经进入了方法体，分配了相应资源） -->同步方法 （在方法体之外）

## 单例模式(懒汉式)的线程安全问题

```java
class Bank{
    private static Bank instance = null;

    public static Bank getInstance() {
        if (instance == null) {
            //效率较高
            synchronized(Bank.class) {
                if (instance == null) {
                    instance = new Bank();
                }
            }
            return instance;
        }
        return instance;
    }
}
```

## 线程的死锁问题

### 出现死锁的原因

* 不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃 自己需要的同步资源，就形成了线程的死锁。
* 出现死锁后，不会出现异常，不会出现提示，只是所有的线程都处于 阻塞状态，无法继续

```java
//出现死锁的例子1
public class Deadlock {
    public static void main(String[] args) {
        StringBuffer stringBuffer1 = new StringBuffer();
        StringBuffer stringBuffer2 = new StringBuffer();
        //第一个线程
        new Thread(){
            @Override
            public void run() {
                synchronized(stringBuffer1){
                    stringBuffer1.append("a");
                    stringBuffer2.append("1");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized(stringBuffer2){
                        stringBuffer1.append("b");
                        stringBuffer2.append("2");
                    }
                }
                System.out.println(stringBuffer1);
                System.out.println(stringBuffer2);
            }
        }.start();
        
        //第二个线程
        new Thread(){
            @Override
            public void run() {
                synchronized(stringBuffer2){
                    stringBuffer1.append("c");
                    stringBuffer2.append("3");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized(stringBuffer1){
                        stringBuffer1.append("d");
                        stringBuffer2.append("4");
                    }
                    System.out.println(stringBuffer1);
                    System.out.println(stringBuffer2);
                }
            }
        }.start();
    }
}
//出现死锁的例子2
public class Deadlock2 {
    public static void main(String[] args) {
        T1 t1 = new T1();
        T2 t2 = new T2();
        new Thread(new Runnable() {
            @Override
            public void run() {
                t2.front(t1);
            }
        }).start();
        t1.front(t2);
    }
}

class T1{
    public synchronized void front(T2 t2){
        System.out.println("T1 front(T2 t2)");
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //到这里时t2已经被锁住,等待释放
        t2.rear();
    }
    public synchronized void rear(){
        System.out.println("T1 rear()");
    }
}

class T2{
    public synchronized void front(T1 t1){
        System.out.println("T2 front(T1 t1)");
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //到这里时t1已经被锁住,等待释放
        t1.rear();
    }
    public synchronized void rear(){
        System.out.println("T2 rear()");
    }
}
```

### 解决死锁的方法

* 专门的算法、原则
* 尽量减少同步资源的定义
* 尽量避免嵌套同步

# 线程的通信

## 涉及到的方法

* `public final void wait() throws InterruptedException`：当前线程进入阻塞并释放同步监视器。
* `public final native void notify()`：唤醒一个被`wait()`的线程，如果有多个线程被`wait()`，就唤醒优先级高的线程。
* `public final native void notifyAll()`：唤醒所有被`wait()`的线程。

说明：

1. `wait()`、`notify()`、`notifyAll()`三个方法必须在同步代码块和同步方法中。
2. `wait()`、`notify()`、`notifyAll()`三个方法的调用者必须是同步代码块或同步方法中的监视器，否则会出现`java.lang.IllegalMonitorStateException`

```java
public class CommunicationTest {
    public static void main(String[] args) {
        NumberRunnable numberRunnable = new NumberRunnable();
        Thread thread1 = new Thread(numberRunnable);
        Thread thread2 = new Thread(numberRunnable);
        thread1.setName("线程1");
        thread2.setName("线程2");
        thread1.start();
        thread2.start();
    }
}

class NumberRunnable implements Runnable{
   private int number = 0;
   Object object = new Object();
    @Override
    public void run() {
        while (true) {
            synchronized (object) {
                //唤醒线程
                object.notify();
                if (number <= 100) {
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() 
                                       + "number:" + number);
                    number++;
                    try {
                        //进入阻塞状态,释放锁
                        object.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                } else {
                    break;
                }
            }
        }
    }
}
```

## sleep()和wait()的异同

* 相同点
  
* 都可以使当前线程进入阻塞状态。
  
* 不同点
  * 声明的位置不同
    * `sleep()`声明在Threadle类中
    * `wait()`声明在Object类中
    
  * 调用的要求不同
  
    * `sleep()`可以在任何需要的场景下调用
  
    * `wait()`只能在同步代码块和同步方法中
  
  * 是否释放同步监视器
  
    * 如果两个方法都使用在同步代码块和同步方法中，`sleep()`不会释放同步监视器，`wait()`会释放同步监视器

# JDK5.0新增线程创方式

## 实现Callable接口

### 与使用Runnable相比

* 相比run()方法，可以有返回值
* 方法可以抛出异常
* 支持泛型的返回值
* 需要借助FutureTask类，比如获取返回结果

### 使用方法

1. 声明一个实现Callable接口的类
2. 实现call方法，将此线程需要执行的操作声明在call()中
3. 创建一个Callable接口实现类的对象
4. 创建FutureTask对象,Callable接口实现类的对象作为构造器参数
5. 创建Thread对象,FutureTask的对象作为构造器参数
6. 通过get()获取Callable中call()的返回值(需要时才使用)

```java
public class NumThreadTest{
    public static void main(String[] args) {
        //3.创建一个Callable接口实现类的对象
        NumThread numThread = new NumThread();
        //4.创建FutureTask对象,Callable接口实现类的对象作为构造器参数
        FutureTask futureTask = new FutureTask(numThread);
        //5.创建Thread对象,FutureTask的对象作为构造器参数
        new Thread(futureTask).start();
        try {
            //通过get()获取Callable中call()的返回值
            Object sum = futureTask.get();
            System.out.println("总和为："+sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
//1.声明一个实现Callable接口的类
class NumThread implements Callable {
    //2.实现call方法，将此线程需要执行的操作声明在call()中
    @Override
    public Object call() throws Exception {
        int sum = 0;
        for(int i = 0; i <= 100;i++) {
            if (i % 2 == 0) {
                System.out.println(i);
                sum += i;
            }
        }
        return sum;
    }
}
```

## 使用线程池

### 相关API

#### ExecutorService:线程池接口

* `public static ExecutorService newCachedThreadPool()`：根据需要创建新线程的线程池
* `public static ExecutorService newFixedThreadPool(int nThreads)` ：可重用固定线程数的线程池
* `public static ExecutorService newSingleThreadExecutor()`：只有一个线程的线程池
* `public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize)`：可安排在给定延迟后运 行命令或者定期地执行的线程池

#### Executors:线程池的工具类

* `<T> Future<T> submit(Callable<T> task)`:执行任务，有返回值，一般又来执行 `Callable`
* `void execute(Runnable command)`：执行任务/命令，没有返回值，一般用来执行 `Runnable`
* `void shutdown()`：关闭连接池

```java
public class ThreadPool {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        //实现Runnable的方式
        executorService.execute(new Runnable() {
            @Override
            public void run() {
                for (int i = 0;i < 100;i ++) {
                    if (i % 2 == 0) {
                        try {
                            Thread.sleep(20);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        System.out.println(Thread.currentThread().getName() + ":" + i);
                    }
                }
            }
        });
        //实现Callable的方式
        Future<Integer> future = executorService.submit(new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                int sum = 0;
                for (int i = 0; i < 100; i++) {
                    if (i % 2 != 0) {
                        try {
                            Thread.sleep(10);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        System.out.println(Thread.currentThread().getName() + ":" + i);
                        sum += i;
                    }
                }
                return sum;
            }
        });
        try {
            int sum = future.get();
            System.out.println("总和为："+sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
        executorService.shutdown();
    }
}
```

### 使用线程池的好处

* 提高响应速度（减少了创建新线程的时间）
* 降低资源消耗（重复利用线程池中线程，不需要每次都创建）
* 便于线程管理(以下参数是通过`ExecutorService`接口实现类对象的set方法来设置)
  * `corePoolSize`：核心池的大小
  * `maximumPoolSize`：最大线程数
  * `keepAliveTime`：线程没有任务时最多保持多长时间后会终止