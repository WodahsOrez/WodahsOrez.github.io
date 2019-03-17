# 组合模式(composite)

**定义**：将对象组合成树形结构以表示"部分-整体"的层次结构,使得用户对单个对象和组合对象的使用具有一致性.
**优点**：高层模块调用简单,节点自由增加
**缺点**：与依赖倒置原则冲突

### 角色

**Component抽象构件角色**：参加组合对象的共有方法和属性.
**Leaf叶子构件**：其下再也没有其他分支,遍历的最小单元
**Composite树枝构件**：组合树枝节点和叶子节点形成一个树形结构.

**使用场景**：只要是树形结构,体现局部和整体的关系的时候,这种关系还可能比较深的时候考虑组合模式

**安全模式**：把树枝节点和树叶节点彻底分开了,胜在安全.

**抽象构件**

```java
public abstract class AbstractComponent {
    /** 父节点 */
    private AbstractComponent parent = null;
    /**  
     * @Description  个体和整体都具有的共享
     */
    public void doSomething() {
        // 编写业务逻辑
    }

    public AbstractComponent getParent() {
        return parent;
    }
    
    public void setParent(AbstractComponent parent) {
        this.parent = parent;
    }
}
```


**树枝构件**

```java
public class Composite extends AbstractComponent {
    /** 构件容器 */
    private ArrayList<AbstractComponent> componentList = new ArrayList<AbstractComponent>();
    
    /**  
     * @Description  增加一个构件
     */
    public void add(AbstractComponent component) {
        component.setParent(this);
        this.componentList.add(component);
    }
    
    /**  
     * @Description  删除一个构件
     */
    public void remove(AbstractComponent component) {
        this.componentList.remove(component);
    }
    
    /**  
     * @Description  获取分支下的所有构件
     */
    public ArrayList<AbstractComponent> getChildren() {
        return this.componentList;
    }
}
```


**树叶构件**

```java
public class Leaf extends AbstractComponent {   
}
```



**透明模式**：遵循了依赖倒转原则,方便系统进行扩展,但是不太安全.
**实现**：把树枝构件的方法抽象给抽象构件,树枝构件和树叶构件都实现抽象构件.