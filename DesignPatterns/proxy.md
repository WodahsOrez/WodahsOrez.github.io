# 代理模式(proxy)

**定义**:为其他对象提供一种代理以控制对这个对象的访问.
**优点**:职责清晰,高扩展性,智能化



## 普通代理

**定义**:只需要访问代理类,不需要知道真实类的存在,通过代理类的构造方法来完成真实类的创建和绑定.
```java
public interface Subject {
    public void request(); 
}

public class RealSubject implements Subject {

    @Override
    public void request() {
        // 业务处理
    }
}

public class Proxy implements Subject {
    private Subject subject = null;
    
    public Proxy() {
        this.subject = new Proxy();
    }
    
    public Proxy(Subject subject) {
        this.subject = subject;
    }
    
    public void request() {
        this.before();
        this.subject.request();
        this.after();
    }
     
    private void after() {
        // 预处理
    }
     
    private void before() {
        // 善后处理
    }
}
```



## 强制代理

定义:从真实类查找到代理类,不允许直接访问真实类,允许使用真实类管理的代理类访问真实类的方法.否则不能访问.



## 动态代理

定义:在实现阶段不关心代理谁,在运行阶段才指定代理哪个对象.
原理:java.lang.reflect.Proxy.newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)方法返回指定接口的代理类的实例，这些接口将调用方法调用到指定的调用处理程序。

```java
public interface Subject {
    public void request(); 
}

public class RealSubject implements Subject {

    @Override
    public void request() {
        // 业务处理
    }
}

public class ProxyHandler implements InvocationHandler {
    private Subject subject = null;
    
    public ProxyHandler(Subject subject) {
        this.subject = subject;
    }
    
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    	// 预处理
        Objcet obj = method.invoke(this.subject,args);
        // 善后处理
        return obj;
    }
}

public class Test {
    Subject subject = new RealSubject();
    Subject proxy = (Subject)Proxy.newProxyInstance(subject.getClass().getClassLoader(), 			subject..getClass().getInterfaces(), new ProxyHandler(subject));
    proxy.request();
}
```

