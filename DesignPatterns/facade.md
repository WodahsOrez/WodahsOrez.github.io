# 门面模式(facade)

**定义**：要求一个子系统的外部与其内部的通信必须通过一个统一的对象进行.门面模式提供一个高层次的接口,使得子系统更易于使用.
**优点**：减少系统的相互依赖,提高了灵活性,提高安全性.
**缺点**：不符合开闭原则,只能在源码上修改.

### 角色：

**Facade门面角色**：此角色知晓子系统的所有功能和责任。会将所有从客户端发来的请求委派到相应的子系统中
**subsystem子系统角色**：可以有一个或多个，每个子系统是一个类的集合，子系统不知道门面的存在，门面是它们的客户端而已。



### 使用场景：

1. 为一个复杂的模块或子系统提供一个供外界访问的接口.
2. 子系统相对独立,外界对子系统的访问只要黑箱操作即可.
3. 预防低水平人员带来的风险扩散.



### 注意事项：

1. 一个子系统可以有多个门面,可以按功能拆分门面,或者按访问权限拆分门面
2. 门面不参与子系统内的业务逻辑,具体逻辑可以通过封装类组合逻辑,门面访问封装类完成.



**子系统**

```java
public class ClassA {
    /**  
     * @Description  业务逻辑方法
     */
    public void doSomethingA() {
        System.out.println("子系统A的业务逻辑");
    }
}
```



**门面对象**

```java
public class Facade {
    /** 被委托的子系统对象 */
    private ClassA a = new ClassA();
    private ClassB b = new ClassB();
    private ClassC c = new ClassC();
    
    /**  
     * @Description  提供给外部访问的方法
     */
    public void methodA() {
        this.a.doSomethingA();
    }
    
    public void methodB() {
        this.b.doSomethingB();
    }
    
    public void methodC() {
        this.c.doSomethingC();
    }
}
```