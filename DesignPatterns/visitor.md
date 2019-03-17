# 访问者模式(visitor)

**定义**：封装一些作用于某种数据结构中的各元素的操作，它可以在不改变数据结构的前提下定义作用于这些元素的新的操作。
**优点**：符合单一职责原则,优秀的扩展性,灵活性非常高
**缺点**：具体元素对访问者公布细节,具体元素变更比较困难,违背了依赖倒置原则

### 角色

**Visitor抽象访问者**： 声明访问者可以访问哪些元素,具体到程序就是visit方法的参数定义哪些对象是可以被访问的.
**ConcreteVisitor具体访问者**：它影响访问者访问到一个类后具体该怎么干,要做什么事情.
**Element抽象元素**： 声明接受哪一类访问者访问,程序上通过accept方法中的参数来定义.
**ConcreteElement具体元素**： 实现accept方法,通常是visitor.visit(this).
**ObjectStruture结构对象**：元素生产者,一般容纳在多个不同类,不同接口的容器.如List,Set,Map

### 使用场景：

1. 一个对象结构包含很多类对象,它们有不同的接口,而你想对这些对象实施一些依赖于其具体类的操作,也就是说用迭代器模式已经不能胜任的场景.
2. 需要对一个对象结构中的对象进行很多不同并且不相关的操作,而你想避免让这些操作"污染"这些对象的类.
3. 业务规则要求遍历多个不同的对象,这本身也是访问者模式出发点,迭代器模式只能访问同类或同接口的数据(除非使用instanceof),而访问者模式可以遍历不同的对象,执行不同的操作,访问者模式还有一个用途,就是充当拦截器角色.



**抽象访问者**

```java
public abstract class AbstractElement {
    /**  
     * @Description  业务逻辑方法
     */
    public abstract void doSomething();
    
    /**  
     * @Description  访问者访问方法
     * @param visitor   允许哪个访问者
     */
    public abstract void accept(Visitor Visitor);
}
```



**具体访问者**

```java
public class ConcreteElement1 extends AbstractElement {
    /**  
     * @Description  业务逻辑方法
     */
    public void doSomething() {
        System.out.println("Element2业务逻辑");
    }
    
    /**  
     * @Description  访问者访问方法
     * @param visitor   允许哪个访问者
     */
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```


**抽象访问者**

```java
public interface Visitor {
    /**  
     * @Description  访问对象方法
     * @param element1   可以访问哪些对象
     */
    void visit(ConcreteElement1 element1);
    
    /**  
     * @Description  访问对象方法
     * @param element2   可以访问哪些对象
     */
    void visit(ConcreteElement2 element2); 
}
```



**具体访问者**

```java
public class VisitorImpl implements Visitor {

    /**  
     * @Description  访问对象方法
     * @param element1   可以访问哪些对象
     */
    @Override
    public void visit(ConcreteElement1 element1) {
        element1.doSomething();
    }
    
    /**  
     * @Description  访问对象方法
     * @param element2  可以访问哪些对象
     */
    @Override
    public void visit(ConcreteElement2 element2) {
        element2.doSomething();
    }
}
```


**结构对象**

```java
public class ObjectStructure {
    /**  
     * @Description  对象生成器,通过一个工厂方法模式模拟
     * @return   
     */
    public static AbstractElement createElement() {
        Random rand = new Random();
        if (rand.nextInt(100) > 50) {
            return new ConcreteElement1();
        } else {
            return new ConcreteElement2();
        }
    }
}
```