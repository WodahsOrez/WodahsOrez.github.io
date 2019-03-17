# 原型模式(prototype)

**定义**:用原型实例指定创建对象的种类,并且通过拷贝这些原型创建新的对象.
**优点**:性能优良,逃避构造函数的约束
**原理**:实现Cloneable接口,并重写clone方法,如:(浅拷贝)

```java
public class Thing implements Cloneable{
    /** 非基础类型的可变引用型变量,在浅拷贝时是不会被拷贝的 */
    private ArrayList<String> arrayList = new ArrayList<String>();

    /**  
     * @Description  Cloneable的接口是没有方法的,覆盖的是Object的clone方法
     * @return    拷贝后的实例类
     */
    @Override
    protected Object clone() {
        Thing thing = null;
        try {
            // 调用Object的clone完成浅拷贝
            thing = (Thing)super.clone();
            // 手动拷贝arraylist来实现深拷贝
            thing.arrayList = (ArrayList<String>) this.arrayList.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return thing;
    }
}
```



### 使用场景:

1. 类初始化需要消耗非常多的资源,这个资源包括数据,硬件资源等.
2. 通过new产生一个对象需要非常繁琐的数据准备或访问权限.
3. 一个对象需要提供给其他对象访问,而且哥哥调用者都可能需要修改其值时.



### 注意:

1. 构造函数不会被执行
2. 浅拷贝:只拷贝本对象,对其内的数组,引用对象等都不拷贝,还是指向原生对象的内部元素地址.
3. 深拷贝:所有的都拷贝.完全不会影响到原生对象. 实现:把不被拷贝的变量单独手动拷贝并添加到拷贝类里.
4. 要使用clone就不能在成员变量上加final