# 装饰模式(decorator)

**定义**：动态的给一个对象添加一些额外的职责.就增加功能来说,装饰模式相比生成子类更为灵活.
**优点**：装饰类和被装饰类互相不会耦合,是继承关系的替代,动态扩展实现类.
**缺点**：多层装饰增加了系统的复杂度.

### 角色:

**抽象构件**：是一个接口或者抽象类,定义需要被装饰的对象.
**具体构件**：抽象构件的实现.
**装饰角色**：一般是抽象类,实现抽象构件,并属性有个private的抽象构件变量
**具体装饰对象**：装饰角色的实现



**抽象装饰类**

```java
public abstract class AbstractDecorator extends AbstractComponent {
    /** 被修饰者 */
    private AbstractComponent component = null;
    
    /**
     * @Description  通过构造函数传递被修饰者
     * @param component   
     */
    public AbstractDecorator(AbstractComponent component) {
        this.component = component;
    }
    
    /**  
     * @Description  委托被修饰者执行
     */
    @Override
    public void operate() {
        this.component.operate();
    }
}
```


**具体装饰类**

```java
public class ConcreteDecorator1 extends AbstractDecorator {
    /**
     * @Description  定义被修饰者
     */
    public ConcreteDecorator1(AbstractComponent component) {
        super(component);
    }
    
    private void method1() {
        System.out.println("method1 修饰");
    }
     
    /**  
     * @Description  重写父类方法
     */
    @Override
    public void operate() {
        this.method1();
        super.operate();
    }
}
```