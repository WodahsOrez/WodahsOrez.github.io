# 修饰符

| 修饰对象   | 可用修饰符（及书写顺序）                                     |
| ---------- | ------------------------------------------------------------ |
| 类的修饰符 | public、abstract、final                                      |
| 构造方法   | public、protected、private                                   |
| 方法       | public、protected、private、abstract、static、final、synchronized、native |
| 成员变量   | public、protected、private、static、final、transient、volatile |
| 局部变量   | final                                                        |

 

## 访问控制修饰符

| **访问修饰符**               | **当前类** | **同一包内** | **子孙类(同一包)** | **子孙类(不同包)**                                           | **其他包** |
| ---------------------------- | ---------- | ------------ | ------------------ | ------------------------------------------------------------ | ---------- |
| public                       | Y          | Y            | Y                  | Y                                                            | Y          |
| protected                    | Y          | Y            | Y                  | Y/N（[说明](http://www.runoob.com/java/java-modifier-types.html#protected-desc)） | N          |
| default(friendly/包访问权限) | Y          | Y            | Y                  | N                                                            | N          |
| private                      | Y          | N            | N                  | N                                                            | N          |

- 访问控制修饰符控制的是访问被修饰者的代码所写的位置，即影响其可见范围。
- 访问继承的方法或成员时，其可见性范围取决于被继承的方法或成员
- 对于protected，不同包的子类中，只能由子类实例或子类的子类实例对象访问，不能由父类或父类的其他分支的子孙类实例对象访问。但是可以在非静态方法内由super来访问。 

```java
package p1;
import P2.Son2;
public class Father1 {
    // 可见范围为同包P1或子类中。
    protected void f() {}    
    // Father继承自Object的clone（）的可见范围为同包java.lang和Object的子类。
    
    public static void main(String[] args) {
        Son2 son = new Son2();
        son2.clone();  // 来源Object,不同包子类，由Father的子类实例访问，可见。
    }
}

package p1;
import P2.Son2;
public class Son1 extends Father1 {
    public static void main(String[] args) {
        Son2 son = new Son2();
        //son.clone(); // 来源父类，同包子类不可见。
    }
}

package P2;
import p1.Father1;
public class Son2 extends Father1 {
    public void f() {
        super.f(); // 不同包子类，super访问可见。
    }
    
    public static void main(String[] args) {
        Father1 father1 = new Father1();
        //father1.f(); // 不同包子类，父类实例调用不可见。
    }
}

package p1;
import P2.Son2;
public class Test {
    public static void main(String[] args) {
        Son1 son1 = new Son1();
        son1.f(); // 来源Father,同包可见。
        //son1.clone(); // 来源Object,不同包子类，父类其他分支子类调用不可见。
 
        Son2 son2 = new Son2();    
        son2.f(); // 来源Father,同包可见。
        //son2.clone(); // Compile Error，因为访问的是继承的clone(),可见性等同Object的clone()，因为不同包且非子类不可见
        // 同包非子类，不可见。
    }
}
```

## 其他修饰符

### static

- 静态变量：对于同一对象类型的不同对象实例，共用一个静态变量。
- 静态方法：内部不能使用非静态变量，不需要对象实例就能使用。

两者可以直接用类名访问，如**classname.variablename** 和 **classname.methodname** 

### final

- 修饰变量：即常量，不能被重新赋值，所以必须在声明时定义初始值。
- 修饰方法：可以被子类继承，但不能被override。
- 修饰类：不能被继承。

### abstract

- 抽象方法：没有实现内容的方法，不能被final和static修饰。子类继承后必须实现该方法，除非子类也是抽象类。拥有抽象方法的类必须声明为抽象类。
- 抽象类：抽象类不能实例化对象，不能被final修饰，可以同时包含抽象方法和非抽象方法，或只有其一。

