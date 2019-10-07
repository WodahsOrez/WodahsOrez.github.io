# 多线程

## 线程


### Runnable接口

方法：`void run() `     重写该抽象方法来实现多线程

### Thread类

Thread类实现了Runnable接口。

#### 构造函数：

```java
Thread()
Thread(String name)
Thread(Runnable target)
Thread(Runnable target, String name)
```

#### 方法：

```java
void run() // 线程启动后需要执行的代码,单独调用不会开启新线程
void start()// 开启该线程，并执行run内的代码
String getName() // 获取线程的名字，Thread-编号(从0开始) ，主线程名字为main
int getPriority()// 获取优先级
void setPriority(int newPriority) // 设置优先级1~10，默认为5
void join() throws InterruptedException // 直到用该方法的线程死亡前一直等待。(在A的线程中间调用B.join()，则直到B结束A才会运行)
static void sleep(long millis) throws InterruptedException// 在指定的毫秒数内让当前正在执行的线程休眠（代码所在的线程休眠，实例调用也一样）,不会释放锁
boolean isAlive()// 当前线程是否处于活跃状态(线程执行结束后的状态返回false)
Thread.State getState()// 返回一个State枚举常量
static Thread currentThread()// 返回一个当前线程对象。
void interrupt()// 把当前线程中断标识重置为true，但线程的中断需要自己在代码里监测该标识并处理
static boolean	interrupted()// 返回调用该方法的当前线程的中断状态，并重置为false
boolean	isInterrupted()// 返回实例指向的线程对象的中断状态，不会重置
static void yield()// 当前线程释放cpu执行权限,进行新一轮的权限争夺
```

实现线程的两种方式：

- 继承Thread类，重写run方法。
- 调用Thread的构造函数，传入实现Runnable接口，重写run方法的实现类。

### Object类里的线程通信方法:

```java
void wait() throws InterruptedException  // 让当前线程等待,并释放锁
void wait(long timeout) throws InterruptedException// 等待指定毫秒
void wait(long timeout,int nanos) throws InterruptedException// 分别是毫秒和纳秒,(当纳秒>500,000时,timeout++)
void notify()  // 唤醒某一个当前对象线程池中的线程(任意)
void notifyAll()  //唤醒当前对象线程池中所有的线程
// notify当前线程不会释放锁，会继续执行
// 锁对象调用以上方法可以控制与之相关的线程。
```
### 线程的状态

![1689841-383f7101e6588094](images/thread/1689841-383f7101e6588094.png)

线程的状态转换如上图所示，其中可能会遇到阻塞状态（blocked）的情况有

1. 调用join()和sleep()方法，sleep()时间结束或被打断，join()中断,IO完成都会回到Runnable状态，等待JVM的调度。
2. 调用wait()，使该线程处于等待池(wait blocked pool)，直到notify()/notifyAll()，线程被唤醒被放到锁定池(lock blocked pool )，释放同步锁使线程回到可运行状态（Runnable）
3. 对Running状态的线程加同步锁(Synchronized)使其进入(lock blocked pool )，同步锁被释放进入可运行状态(Runnable)。



## 相关概念

**并发**：同时有多个线程在抢占一个cpu资源，但同时只有一个线程真正处于运行状态。这种方式成为并发。

**线程安全**：经常用来描绘一段代码。指在并发的情况之下，该代码经过多线程使用，线程的调度顺序不影响任何结果。这个时候使用多线程，我们只需要关注系统的内存，cpu是不是够用即可。反过来，线程不安全就意味着线程的调度顺序会影响最终结果。

**同步**：是指通过人为的控制和调度，保证共享资源的多线程访问成为线程安全，来保证结果的准确。

**重入锁**：同一把锁在未被释放时，可以被重复多次获得。

**死锁**：两个线程分别持有对方想要的锁。A有锁1去要锁2，B有锁2去要锁1。

**闭锁**：一种同步方法，可以延迟线程的进度直到线程到达某个终点状态。且闭锁具有一次性，一旦打开，则无法被关闭。

**公平锁**：谁等待的时间长，谁获得锁。效率比非公平锁低。synchronize是非公平锁。

## synchronized同步

### synchronized关键词

```java
// 同步代码块，括号内填实例对象，或者.class对象作为锁
synchronized(对象)
{
	// 需要被同步的代码；
}

// 同步方法，锁是this对象
public synchronized void fn(){
    // 需要被同步的代码；
}

// 同步静态方法，锁是字节码对象对象*.class/this.getClass
public synchronized void fn(){
    // 需要被同步的代码；
}
```

#### synchronized同步机制的原理简述：

- 同步代码：synchronized修饰的代码块区域或者方法区域
- 锁对象：实例对象或字节码对象，由synchronized后括号内的变量指定
- 进入同步代码需要获得相应的锁对象，同一个锁对象只能同时被一个线程持有
- 当线程获取锁对象时，该锁对象被其他线程持有时，就会阻塞等待锁对象被释放，然后进入可执行状态争夺cpu控制权
- 同一个线程可以同时持有多个锁，即同步代码内可以访问其他同步代码，无论锁相不相同。

#### 使用synchronized的注意事项

- 同步方法被锁后，不影响多线程对非同步方法和锁不同的同步方法的访问。
- 对于synchronized线程抛出异常锁就会被释放。
- synchronized(o)，o指向的对象属性改变不影响锁的使用，但如果o的引用指向了其他对象，那么同步区域的锁对象也发生改变。
- 不要用字符串常量作为锁定对象，容易锁到同一个常量而导致死锁。

#### synchronized可重入锁

**概念**：同一线程对同一个对象锁是可重入的，即该线程可以获取多次该锁对象。

**应用情景**：

- 同一线程在锁A的方法内部访问另一个锁是A的同步方法。
- 子类this同步方法内访问父类this同步方法。

**实现方法**：

为每个锁关联一个线程持有者和计数器，当计数器为0时表示该锁没有被任何线程持有，那么任何线程都可能获得该锁而调用相应的方法；当某一线程请求成功后，JVM会记下锁的持有线程，并且将计数器置为1；此时其它线程请求该锁，则必须等待；而该持有锁的线程如果再次请求这个锁，就可以再次拿到这个锁，同时计数器会递增；当线程退出同步代码块时，计数器会递减，如果计数器为0，则释放该锁。

## 共享变量可见性

### 导致共享变量在线程间不可见的原因：

1. 线程的交叉执行
2. 重排序结合线程交叉执行
3. 共享变量更新后的值没有在工作内存与主内存间及时更新
   - 所有的变量都存储在主内存中，每个线程都有自己独立的工作内存，里面保存该线程使用到的变量的副本（主内存中该变量的一份拷贝）
   - 线程对共享变量的所有操作都必须在自己的工作内存中进行，不能直接从主内存中读写
   - 不同线程之间无法直接访问其他线程工作内存中的变量，线程间变量值的传递需要功过主内存来完成。
   - 线程A修改了共享变量，但还没更新到主存，线程B读取这个变量的值时读到的就不是线程A修改后的值。



### volatile关键词

- 对volatile变量执行写操作时，会在写操作后加入一条store屏障指令
  - store指令会在写操作后把最新的值强制刷新到主内存中。同时还会禁止cpu对代码进行重排序优化。这样就保证了值在主内存中是最新的。
- 对volatile变量执行读操作时，会在读操作前加入一条load屏障指令
  - load指令会在读操作前把内存缓存中的值清空后，再从主内存中读取最新的值。
- volatile变量在每次被线程访问时，都强迫从主内存中重读该变量的值，而当变量发生变化时，又强迫线程将最新的值刷新到主内存。这样任何时刻，不同的线程总能看到该变量的最新的值。
- volatile要比synchronize效率高的多，但只保证可见性，不保证原子性。（即读的时候是最新的值，但写的时候就不会检查是否还是之前读的那个值了）

原子性包：Atomic开头的类基本都保证了原子性和线程安全，且性能比synchronize高，只保证它提供的操作的原子性，和其他代码合用时总体的原子性不保证。如：java.util.concurrent.atomic包。原子类底层都使用了volatile保证了可见性。

#### volatile的适用场景

1. 对变量的写入操作不依赖其当前值
   - 不满足：number++、count=count*5
   - 满足：boolean变量、记录温度变化的变量等
2. 该变量没有包含在具有其他变量的不变式中
   - 不满足：不变式 low<up





### synchronized的可见性

- 线程解锁前，必须把共享变量的最新值刷新到主内存中。
- 线程加锁时，将清空工作内存中共享变量的值,从而使用共享变量时需要从主内存中重新读取最新的值（注意：加锁与解锁需要的是同一把锁）

#### 线程执行互斥代码的过程：

```
获得互斥锁
清空工作内存
从主内存拷贝最新副本到工作内存中。
执行代码
将更改过后的共享变量的值刷新到主内存中去。
释放互斥锁。
```



## 闭锁

#### CountDownLatch类

所属包：java.util.concurrent包

构造方法：

```java
public CountDownLatch(int count) // 指定计数的初始值，只能被设置一次，当为0时，被阻塞的线程恢复运行
```

方法：

```java
public void countDown();   // 调用此方法则计数减1
Public Long getCount();    // 得到当前的计数
public void await() throws InterruptedException   // 调用此方法会一直阻塞当前线程，直到计数的值为0，除非线程被中断。
Public boolean await(long timeout, TimeUnit unit) // 调用此方法会一直阻塞当前线程，直到计数的值为0，除非线程被中断或者计时器超时，返回false代表计时器超时。timeout是时间，unit是单位。
```

例子：

```java
public class Latch {
    private CountDownLatch count = new CountDownLatch(1); 
    
    public void th1() {
        System.out.println("th1:thread start");
        System.out.println("th1:count is "+this.count.getCount());
        try {
            if(this.count.getCount() != 0) {
                count.await();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("th1:thread end");
    }
    
    public void th2() {
        System.out.println("th2:thread start");
        count.countDown();
        System.out.println("th2:thread end");
    }
    
    public static void main(String[] args) {
        Latch latch = new Latch();
        Thread th1 = new Thread(latch::th1);
        Thread th2 = new Thread(latch::th2);
        th1.start();
        
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        th2.start();
    }
}
/* output:
    th1:thread start
    th1:count is 1
    th2:thread start
    th2:thread end
    th1:thread end
*/
```






## Lock手动锁同步

**相关包**：java.util.concurrent.locks包

### Lock接口

方法

```java
void lock()  当前线程获取锁
void lockInterruptibly() throws InterruptedException 同上，区别在于当锁不可用而阻塞时，如果当前线程的中断标识为true，就会抛出InterruptedException来中断获取锁的操作
void unlock()  当前线程释放锁
Condition  newCondition()  返回绑定到此Lock实例的新监视器实例
boolean tryLock() 仅在调用锁为空闲状态才获取该锁，同时返回true，如果锁不可用，返回false
boolean	tryLock(long time, TimeUnit unit) throws InterruptedException同上， 区别在于会等待指定的时间，时间内获得锁可用就获得锁，返回true，等到超时也没获取锁时，结束等待，返回false
```

### ReentrantLock类

实现了Lock接口，构造方法如下：

```java
ReentrantLock() 无参构造
ReentrantLock(Boolean fair) fair为true时，创建公平锁，默认为false是非公平锁
```

### Condition接口

方法:

```java
void await() throws InterruptedException当前线程阻塞等待，直到被signal或signalAll唤醒，或是中断标识变为true时抛出InterruptedException来解除阻塞
boolean	await(long time, TimeUnit unit) throws InterruptedException同上，设定超时，时间内获得锁为true，反之，为false
long awaitNanos(long nanosTimeout) throws InterruptedException同上，参数填long型的纳秒数，返回值是剩余时间，超时返回一个小于等于0的值
boolean	awaitUntil(Date deadline) throws InterruptedException同2，但设定的是截止时间
void awaitUninterruptibly() 同await()，区别在于中断不会影响该方法的阻塞
void signal() 唤醒当前监视器内的等待的某一个线程
void signalAll() 唤醒当前监视器内的等待的所有线程
```

## ThreadLocal类

由于同步访问会降低并发性，影响系统性能。可以用空间换时间，给每个线程都分配一个独立的变量，就可以用非同步的方式使用非线程安全的变量，我们称这种变量为线程局部变量。

### 方法

```java
T get() 获取当前线程的局部变量
void set(T value) 设置当前线程的局部变量
void remove() 移除当前线程的局部变量，让GC可以回收该变量
protected T	initialValue() java8之前，继承ThreadLocal类，重写该方法来实现设置初始值，第一次调用get时被调用，但如果get之前已经set了一个值，则不调用
static <S> ThreadLocal<S> withInitial(Supplier<? extends S> supplier) java8之后，可以传递一个函数对象，用来设置初始值的方法
```

