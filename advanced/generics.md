# 泛型(generics)

- 泛型就是把类型参数化，<>即存放类型参数的位置，T，E，K，V即变量名.泛型的实例化即类似于变量的赋值，把具体的类型名填入<>中，即相应位置处的T变成这种类型。
- 泛型的设计目的是用T代替Object来传入未知类型对象，而?是为了能使泛型达到协变和逆变这种关系而存在的，即使泛型类型间建立关系.



## 泛型类

### 定义泛型类的语法：

访问修饰符` class className<TypeList>`
TypeList表示类型变量列表，每个类型变量之间以逗号隔开。
例如：` Interface Collection<E>`     或者   `Interface Map<K,V>`     其中E,K,V为类型变量(参数)，和变量一样，名称可以自定义。



### 创建泛型类的语法：

`className<TypeList> =new className<TypeList>(argList);`
argList表示实际传递的类型参数列表，同样以逗号隔开。
例如：`ArrayList<String> =new ArrayList<String>();`
泛型的声明必写,后面可以只写<>,因为有类型推断机制。
即：`ArrayList<String> =new ArrayList<>()`



## 泛型方法

### 定义泛型方法的语法：

访问修饰符<类型参数> 返回值 方法名 (类型参数列表)
**类型参数放在访问修饰符的后面，返回类型的前面.泛型类的静态方法如果想用泛型，必须重新定义成泛型方法。**
例如：`public<T> void showName (T t){ }`



### 泛型方法的使用:

例如:`对象.<String>showNam(name)`  写在方法名前面,这样编译器可以判断name是否为String
有足够条件判断时,泛型方法前的<>可以省略.

**泛型方法使用时如果加<>前面必须要有对象.或者类名.或this.super.无论是否在同一个类里**



## 泛型参数

### 泛型参数

泛型直接放在方法的参数列表里,(静态方法必须定义成泛型方法)
例如:
`public void show(List<?> list)`  任意情况都可用 
`public void show(List<T> list)`  必须在泛型类里使用
`public static void show(List<?> list)`  可以不定义成泛型方法
`public static <T>void show(List<T> list)`  必须定义成泛型方法,不管是否在泛型类里



### 泛型的限定：

上限：**extends表示继承或实现或两者皆有，后面可以有一个类和多个接口，<u>类必须在前</u>。**
语法：<T extends BoundingType>     <T extends Comparable & Serializable>
下限：
**没有T super XXX的写法,只能用? super XXX**
通配符类型：
例如：`public static void printBuddies(Pair<? extends Employee> p)`
[如何理解 Java 中的 <T extends Comparable<? super T>>](http://www.cnblogs.com/xiaomiganfan/p/5390252.html)
泛型的擦除，不用泛型时，用的是原始类型，无限定类型用Object，详见P534.



### 方法参数重写

`public static <T>void getData(List<T> data)`
`public static void getData(List<?> data)`
`public static void getData(List<? extends Number> data)`
**这三种方法是同一个方法,即参数视为一致,无法看做重载.**



### 泛型T和?的差别

T只能在泛型`类<T>`或`泛型方法<T>`中使用,而`<?>`和其上下限可以在任何情况下使用,即普通方法和类的任意位置使用`<?>`.
这种情况下的`List<?> list` 中,由于list存在不确定性,不能用`list.add()`添加任意元素.只能用`list =new ArrayList<Integer>();`给其赋值.
**T是用在定义泛型的,而?是在定以后的泛型使用时对其T参数进行范围限制的存在.即先有T后有?**
**T extends 是对T进行必要的属性解释, 而T super没有意义所以不存在.**



### 泛型无法向上转型

```java
Info<String> i1 = new Info<String>() ;      // 泛型类型为String  
Info<Object> i2 = null ;  
i2 = i1 ;                               //这句会出错 incompatible types  
```



## 通配符

`<?>`即等效于`<? extends Object>`用于拓展用`<Object>`的情况,使其能兼容更多类型
`List<? extends Object>`是`List<Integer>`的父类  协变
`List<? super Integer>`是`List<Object`的父类  逆变

![generics](images/generics/generics.png)

### 通配符上限写入的情况 

```java
public class Wildcard {
	public static void main(String[] args) {
		List<Integer> list = new ArrayList();
		getData(list);
	}
	public static void getData(List<?> list){
		System.out.println("data:" + list.get(0));
		list.add(12);  不能写入
		list.add(new Object());  不能写入
	}
}
```

因为`List<?>`可能是`List<Object>`或`List<Integer>`,所以为避免出现不能写入所以一律不能写入任意对象,在`<? extends XX>`里适用这个规则



### 通配符下限写入的情况
```java
public class Wildcard {
	public static void main(String[] args) {
		List<Integer> list = new ArrayList();
		getData(list);
	}
	public static void getData(List<? super Object> list){
		System.out.println("data:" + list.get(0));
		list.add(12);  可以写入
		list.add(new Object());  可以写入
		

	}
```
由于有下限,所以下限及其子类都能写入而不用担心因为不确定性而产生错误.

[通配符讲解很详细.](http://blog.csdn.net/baple/article/details/25056169#t1)



### “捕获助手” 方法
```java
public void rebox(Box<?> box) {
    reboxHelper(box);
}
private<V> void reboxHelper(Box<V> box) {
    box.put(box.get());
}
```
解析:`Box<?>`返回的是一个Object,而`Box<V>`返回的是V



### 存取原则和PECS法则

总结 ? extends 和 the ? super 通配符的特征，我们可以得出以下结论：

1. 如果你想从一个数据类型里获取数据，使用 ? extends 通配符
2. 如果你想把对象写入一个数据结构里，使用 ? super 通配符
3. 如果你既想存，又想取，那就别用通配符。

PECS是指”Producer Extends, Consumer Super”



### 参考：

[协变逆变](http://www.cnblogs.com/en-heng/p/5041124.html)

[java泛型简明教程](http://kb.cnblogs.com/page/104265/)

[泛型代码](http://iteye.blog.163.com/blog/static/18630809620131472312201/)

[通配符](http://blog.sina.com.cn/s/blog_65554d980100ijft.html)