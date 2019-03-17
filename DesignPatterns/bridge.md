# 桥梁模式(bridge)

**定义**：将抽象和实现解耦，使得两者可以独立地变化。
**优点**：抽象和实现分离,优秀的扩充能力,实现细节对客户透明

### 理解：

- 即把不变的特性留在Abstraction内,把可变的特性提取出来单独放入Implementor。
- Abstraction通过引用Implementor来具有这部分可变特性,形成一个完整地事物抽象。
- 而Implementor通过继承来实现这部分特性的变化.而Abstraction通过继承来实现事物整体的变化。

### 角色：

**Abstraction抽象化角色**：定义出该角色的行为,同时保存一个对实现化角色的引用,该角色一般是抽象类.
**Implementor实现化角色**：它是接口或抽象类,定义角色必须的行为和属性.
**RefinedAbstraction修正抽象化角色**：它引用实现化角色对抽象化角色进行修正.
**ConcreteImplementor具体实现化角色**：它实现接口或抽象类定义的方法和属性.

### 使用场景：

1. 不希望或不适用使用继承的场景
2. 接口或抽象类不稳定的场景
3. 重用性要求较高的场景



#### 抽象化角色

```java
public abstract class Abstraction {
    /** 对实现化角色的引用 */
    private Implementor imp;

    /**
     * @Description  设置实现化角色
     * @param imp  实现化角色
     */
    public Abstraction(Implementor imp) {
        this.imp = imp;
    }
       
    /** 
     * @Description  自身行为和属性
     */
    public void request() {
        this.imp.doSomething();
    }
       
    public Implementor getImp() {
        return this.imp;
    }
}
```


#### 实现化角色

```java
public interface Implementor {
    /** 
     * @Description  业务逻辑
     */
    void doSomething();
    /** 
     * @Description  业务逻辑
     */
    void doAnything();
}
```


#### 修正抽象化角色

```java
public class RefinedAbstraction extends Abstraction {
    /**
     * @Description  覆写构造函数
     */
    public RefinedAbstraction(Implementor imp) {
        super(imp);
    }
    /** 
     * @Description  修正父类行为
     */
    @Override
    public void request() {
        super.request();
        super.getImp().doAnything();
    }
}
```


#### 具体实现化角色

```java
public class ConcreteImplementor1 implements Implementor {
    /** 
     * @Description  业务逻辑
     */
    @Override
    public void doSomething() {
        System.out.println("ConcreteImplementor1----doSomething");
    }
    /** 
     * @Description  业务逻辑
     */
    @Override
    public void doAnything() {
        System.out.println("ConcreteImplementor1----doAnything");
    }
}
```