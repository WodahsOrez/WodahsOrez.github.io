# 命令模式(command)

**定义**:将一个请求封装成一个对象,从而让你使用不同的请求把客户端参数化,对请求排队或者记录请求日志,可以提供命令的撤销和恢复功能.
**优点**:类间接耦合,可扩展性,命令模式结合其他模式会更优秀.
**缺点**:命令类会很膨胀.

### 角色:

**接受者角色**：干活的角色,命令传递到这里应该被执行的.
**命令角色**：需要执行的所有命令都在这里声明
**调用者角色**：接收到命令,并执行命令.

### 实现:

**抽象命令类**

```java
public abstract class AbstractCommand {
    /** 定义一个子类全局共享变量 */
    protected final AbstractReceiver receiver;
    
    /**
     * @Description  实现类必须传入一个接收者
     */
    public AbstractCommand(AbstractReceiver receiver) {
        this.receiver = receiver;
    }
    /**  
     * @Description  每个命令类都必须有一个执行命令的方法  
     */
    public abstract void execute();
}
```


**具体命令类**

```java
public class ConcreteCommand1 extends AbstractCommand {
    /**
     * @Description  设置默认接收者
     */
    public ConcreteCommand1() {
        super(new ConcreteReceiver1());
    }
    /**
     * @Description  构造函数传递接收者
     * @param receiver  接收者对象
     */
    public ConcreteCommand1(AbstractReceiver receiver) {
        super(receiver);
    }
    
    /**  
     * @Description  每个命令类必须实现执行命令方法
     */
    @Override
    public void execute() {
        // 业务逻辑
        this.receiver.doSomething();
    }
}
```


**接收者**

```java
public class ConcreteReceiver1 extends AbstractReceiver {
    /**  
     * @Description  每个接收者都必须完成的业务逻辑 
     */
    @Override
    public void doSomething() {
        // 业务逻辑处理
        System.out.println("receiver1 is doing Something");
    }
}
```


**调用者**

```java
public class Invoker {
    /** 需要执行的命令类 */
    private AbstractCommand command;
    

    /**  
     * @Description  设置命令类
     */
    public void setCommand(AbstractCommand command) {
       this.command = command; 
    }
    
    /**  
     * @Description  执行命令
     */
    public void action() {
        this.command.execute();
    }
}
```