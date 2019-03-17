# 适配器模式(adapter)

**定义**：将一个类的接口变成客户端所期待的另一种接口,从而使原本因接口不匹配而无法一起工作的两个类能够在一起工作.
**优点**：让两个没有任何联系的类一起运行,增加了类的透明性,提高了类的复用度,灵活性非常好


### 角色：

**目标角色Target**：定义把其他类转换为何种接口,也就是我们的期望接口.
**源角色Adaptee**：被转换的角色,它是已经存在的,运行良好的类或是对象,经过适配器包装,成为一个崭新的角色.
**适配器角色Adapte**r：把源角色转换成目标角色,通过继承和类关联的方式.



**类适配器**：继承源角色类并实现目标角色接口,是类间继承.

```java
public class Adapter1 extends Adaptee1 implements Target {

    /**  
     * @Description  实现Target方法,通过调用Adaptee的方法
     */
    @Override
    public void request() {
        super.doSomething();
    }
}
```


**对象适配器**：实现目标角色接口,通过类的关联,耦合源角色.是对象的合成关系.(使用的场景较多)

```java
public class Adapter2 implements Target {
    /** 存放源角色1 */
    private Adaptee1 adaptee1 = null;

    /** 存放源角色2 */
    private Adaptee2 adaptee2 = null;
    
    /**
     * @Description  通过构造函数设置源角色
     * @param adaptee1  源角色1
     * @param adaptee2  源角色2
     */
    public Adapter2(Adaptee1 adaptee1, Adaptee2 adaptee2) {
        this.adaptee1 = adaptee1;
        this.adaptee2 = adaptee2;
    }
    
    /**  
     * @Description  实现Target的方法,调用源角色的方法
     */
    @Override
    public void request() {
        this.adaptee1.doSomething();
        this.adaptee2.doSomething();
    }
}
```