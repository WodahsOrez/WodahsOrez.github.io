# 状态模式(state)

**定义**：当一个对象的内在状态改变时允许其改变行为,这个对象看起来像改变了它的类型.
**优点**：结构清晰,遵循开闭原则和单一职责原则,封装性好
**缺点**：子类太多,类膨胀

### 角色

**State抽象状态角色**：接口或抽象类,负责对象状态的定义,并且封装环境角色以实现状态处理.
**ConcreteState具体状态角色**：每个具体状态必须完成两个职责:本状态行为管理以及趋向状态处理,即本状态该做的事和状态过渡.
**Context环境角色**：定义客户端需要的接口,并且负责具体状态的切换.(将状态对象声明为静态常量,具有状态抽象角色定义的所有行为,具体执行使用委托方式)



### 使用场景

1. 行为随状态改变而改变的场景.
2. 条件,分支判断语句的替代者.



**抽象状态类**

```java
public abstract class AbstractState {
    /** 定义一个环境角色 */
    protected Context context;

    /** 
     * @Description  设置环境角色
     */
    public void setContext(Context context) {
        this.context = context;
    }
       
    /** 
     * @Description  行为1
     */
    public abstract void handle1();
    /** 
     * @Description  行为2 
     */
    public abstract void handle2();
}
```


**具体状态类**

```java
public class ConcreteState1 extends AbstractState {
    /** 
     * @Description  行为1
     */
    @Override
    public void handle1() {
        System.out.println("state1:状态1必须处理的逻辑");
    }
    /** 
     * @Description  行为2,转为状态2,由状态2执行行为2
     */
    @Override
    public void handle2() {
        System.out.println("state1:转为状态2");
        super.context.setCurrentState(Context.STATE2);
        System.out.println("state1:委托context执行行为2");
        super.context.handle2();
    }
}

public class ConcreteState2 extends AbstractState {
    /** 
     * @Description  行为1,转为状态1,由状态1执行行为1
     */
    @Override
    public void handle1() {
        System.out.println("state2:转为状态1");
        super.context.setCurrentState(Context.STATE1);
        System.out.println("state2:委托context执行行为1");
        super.context.handle1();
    }
    /** 
     * @Description  行为2
     */
    @Override
    public void handle2() {
        System.out.println("state2:状态2必须处理的逻辑");
    }
}
```


**具体环境类**

```java
public class Context {
    /** 状态1 */
    public static final AbstractState STATE1 = new ConcreteState1();
    /** 状态2 */
    public static final AbstractState STATE2 = new ConcreteState2();
    /** 当前状态 */
    private AbstractState CurrentState;

    /** 
     * @Description  获得当前状态
     * @return  
     */
    public AbstractState getCurrentState() {
        return CurrentState;
    }
       
    /** 
     * @Description  设置当前状态
     * @param currentState  
     */
    public void setCurrentState(AbstractState currentState) {
        this.CurrentState = currentState;
        this.CurrentState.setContext(this);
    }
       
    /** 
     * @Description  委托执行行为1
     */
    public void handle1() {
        System.out.println("context:委托执行行为1");
        this.CurrentState.handle1();
    }
       
    /** 
     * @Description  委托执行行为2
     */
    public void handle2() {
        System.out.println("context:委托执行行为2");
        this.CurrentState.handle2();
    }
}
```