# 享元模式(flyweight)

**定义**：使用共享对象可以有效的支持大量的细粒度的对象.
**优点**：减少创建对象,降低内存占用,增强程序的性能
**缺点**：分离出外部状态和内部状态,提高了系统的复杂性.

### 角色：

**Flyweight抽象享元角色**：一个产品的抽象类,定义出对象的外部状态和内部状态的接口或实现.
**ConcreteFlyweight具体享元角色**：实现抽象角色定义的业务,该角色中需要注意的是内部状态处理应该与环境无关,不应该出现一个操作改变了内部状态,同时修改了外部状态.
**unsharedConcreteFlyweight不可共享的享元角色**：不存在外部状态或者安全要求(线程安全)不能够使用共享技术的对象,该对象一般不会出现在享元工厂里.
**FlyweightFactory享元工厂**：就是一个池容器,提供获取对象的方法.

### 使用场景：

1. 系统中存在大量相似的对象.
2. 细粒度的对象都具备较接近的外部状态,而且内部状态与环境无关.也就是说对象没有特定身份.
3. 需要缓冲池的场景.



#### 抽象享元类

```java
public abstract class AbstractFlyweight {
    /** 内部状态 */
    private String intrinsic;
    /** 外部状态 */
    protected final String EXTRINSIC;

    /**
     * @Description  设置外部状态
     * @param EXTRINSIC  外部状态
     */
    public AbstractFlyweight(String EXTRINSIC) {
        this.EXTRINSIC = EXTRINSIC;
    }
       
    /** 
     * @Description  业务逻辑
     */
    public abstract void operate();
       
    public String getIntrinsic() {
        return intrinsic;
    }
       
    public void setIntrinsic(String intrinsic) {
        this.intrinsic = intrinsic;
    }
}
```



#### 具体享元类

```java
public class ConcreteFlyweight1 extends AbstractFlyweight{
    /**
     * @Description  设置外部状态
     * @param EXTRINSIC  外部状态
     */
    public ConcreteFlyweight1(String EXTRINSIC) {
        super(EXTRINSIC);
    }

    /** 
     * @Description  具体业务逻辑
     */
    @Override
    public void operate() {
        System.out.println("业务逻辑");
    }
}
```



#### 享元工厂类

```java
public class FlyweightFactory {
    /** 池容器 */
    private static HashMap<String,AbstractFlyweight> pool = new HashMap<String,AbstractFlyweight>();

    /** 
     * @Description  工厂方法
     * @param EXTRINSIC  外部状态
     * @return 
     */
    public static AbstractFlyweight getFlyweight(String EXTRINSIC) {
        AbstractFlyweight flyweight = null;
        if (pool.containsKey(EXTRINSIC)) {
            flyweight = pool.get(EXTRINSIC);
        } else {
            flyweight = new ConcreteFlyweight1(EXTRINSIC);
            pool.put(EXTRINSIC, flyweight);
        }
        return flyweight;
    }
}
```