# 模板方法模式(template)

定义一个算法中的操作框架，而将一些步骤延迟到子类中。使得子类可以不改变算法的结构即可重定义该算法的某些特定步骤。

使用了Java的继承机制，是一个非常广泛的模式。其中AbstractClass叫做抽象模板，它的方法分为三类：

### 基本方法

由子类实现的方法,在模板方法内被调用(尽量设计为protected类型，符合迪米特法则，不需要暴露的属性或方法尽量不要设置为protected类型。实现类若非必要，尽量不要扩大父类中的访问权限)

### 模板方法

可以有一个或几个，一般是一个具体方法，也就是一个框架，实现对基本方法的调用，完成固定的逻辑。(一般都加上final关键字，防止被覆写）

### 钩子方法

由子类覆写的方法,返回一个boolean值,在模板方法中作为用来控制代码逻辑的一个标识.

**代码举例**

```java
public abstract class AbstractClass {
    protected abstract void doAnything();  //基本方法
    protected abstract void doSomething();  //基本方法
    protected boolean isDoSomething(){ //钩子方法,默认父类方法返回真
        return true;
    }
    public final void templateMethod(){
        /*
         * 调用基本方法，完成相关的逻辑
         */
        this.doAnything();
        if(this.isDoSomething())
            this.doSomething();
    }
}
public class ConcreteClass1 extends AbstractClass {
    private boolean isDoSth;
    @Override
    protected void doAnything() {
        //子类实现具体
    }

    @Override
    protected void doSomething() {    
    }
    protected void setDo(boolean isDo){
        this.isDoSth = isDo;
    }
    protected boolean isDoSomething(){
        return isDoSth;
    }
}
```

