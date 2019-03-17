# 多线程

## Runnable接口

方法：`void run() `     重写该抽象方法来实现多线程

### Thread类

#### 构造函数：

```java
Thread()
Thread(String name)
Thread(Runnable target)
Thread(Runnable target, String name)
```

#### 方法：

```java
void run()
void start()     运行该线程
String getName()     获取线程的名字，Thread-编号(从0开始) ，主线程名字为main
int getPriority()  获取优先级
void setPriority(int newPriority)     设置优先级1~10，默认为5
void join()     直到用该方法的线程死亡前一直等待。(在A的线程中间调用B.join()，则直到B结束A才会运行)
static void sleep(long millis)     在指定的毫秒数内让当前正在执行的线程休眠（代码所在的线程休眠，实例调用也一样）
boolean isAlive()  当前线程是否处于活跃状态(线程执行结束后的状态返回false)
Thread.State getState()  返回一个State枚举常量
static Thread currentThread()     返回一个当前线程对象。
void	interrupt()  把当前线程从阻塞状态中恢复到可执行状态
static void yield()  当前线程释放执行权限,进行新一轮的权限争夺
```

#### 同步代码块的格式：

```java
synchronized(对象)
{
	需要被同步的代码；
}
```

同步函数的锁是this，静态同步函数的锁是字节码对象(*.class/this.getClass)

#### Object类里的线程通信方法:

```java
void wait()  让当前线程冻结,并释放锁
void wait(long timeout,)
void wait(long timeout,int nanos)分别是毫秒和纳秒,(当纳秒>500,000时,timeout++)
void notify()  唤醒一个当前对象线程池中的线程(任意)
void notifyAll()  唤醒当前对象线程池中所有的线程
```



## java.util.concurrent.locks

### Lock接口

方法:

```java
void lock()  当前线程获取锁,
void unlock()  当前线程释放锁.
Condition  newCondition()  返回监视器对象
```

#### ReentrantLock类

`ReentrantLock()`

#### Condition接口

方法:

```java
void	await()
void	signal()
void	signalAll()
```

