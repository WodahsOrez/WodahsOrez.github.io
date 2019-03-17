# 单例模式(singleton)

**定义**:保证一个类仅有一个实例，而且自行实例化并提供一个访问它的全局访问点。

**主要解决**:一个全局使用的类频繁地创建与销毁。

**关键代码**:构造函数是私有的。

**优点**:减少内存开支,对性能都开销,在需要频繁创建销毁,而创建和销毁的性能又无法优化的时候使用.

**缺点**:扩展困难,不利于测试,与单一职责原则有冲突.

 

### 饿汉式 (推荐使用,不用考虑同步问题)

```java
public class Singleton {  
    private static Singleton instance = new Singleton();  
    private Singleton (){}  
    public static Singleton getInstance() {  
        return instance;  
    }  
}
```

 

 

### 懒汉式(同步加synchronized)

```java
public class Singleton {  
    private static Singleton instance;  
    private Singleton (){}  
    public static synchronized Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}  
```

 

### 双检锁/双重校验锁（DCL，即 double-checked locking）

```java
public class Singleton {  
    private volatile static Singleton singleton;  
    private Singleton (){}  
    public static Singleton getSingleton() {  
        if (singleton == null) {  
            synchronized (Singleton.class) {  
            if (singleton == null) {  
                singleton = new Singleton();  
            }  
            }  
        }  
        return singleton;  
    }  
} 
```

 

### 登记式/静态内部类

```java
public class Singleton {  
    private static class SingletonHolder {  
    private static final Singleton INSTANCE = new Singleton();  
    }  
    private Singleton (){}  
    public static final Singleton getInstance() {  
    return SingletonHolder.INSTANCE;  
    }  
} 
```

 

### 枚举

```java
public enum Singleton {  
    INSTANCE;  
    public void whateverMethod() {  
    }  
} 
```

 

**经验之谈**：一般情况下，不建议使用懒汉方式，建议使用饿汉方式。只有在要明确实现 lazy loading 效果时，才会使用登记方式。如果涉及到反序列化创建对象时，可以尝试使用枚举方式。如果有其他特殊的需求，可以考虑使用双检锁方式。