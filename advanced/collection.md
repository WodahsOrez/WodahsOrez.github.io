# 集合框架和泛型

集合的特点有：可以存储对象<u>不能存储基本数据类型</u> ，集合的长度是可变的。

## collection接口

特点：不唯一，无序号

### 方法：

#### 1、增加：

```java
boolean add(E e)向末尾加
boolean addAll(Collection<? extends E> c)
```

#### 2、删除：

```java
void clear()    清空集合
boolean remove(Object o)删第一个
boolean removeAll(Collection<?> c)删除c中包含的 
boolean retainAll(Colletion <?> c)仅保留c中的，其他都删除，交集 
```
#### 3、获取：

```java
int size()元素个数
Iterator<E> iterator()返回迭代器对象
```
#### 4、查找：

```java
boolean contains(Object o)      contains方法是通过equals判断的。
boolean containsAll(Collection<?> c)
boolean isEmpty()
```
#### 5、转换：

```java
Object[] toArray()转换为Object数组
<T>T[] toArray(T[] a) 把collection转换成T[]数组传出
```

##      List接口

特点：不唯一，有序号

### ArrayList类

特点：和数组一样采用同样的存储方式，在内存中分配连续的空间。遍历，随机访问快，删改慢。
方法：

1. 增加：`void add(int index,E e)`插入，后面的后移
2. 删除：`E remove(int index)`
3. 获取：`E get(int index)` 从0开始    int indexOf(Object o)返回第一个索引，没有返回-1
4. 修改：`void set(int index, E element )`



### LinkedList类

特点：采用链表存储方式。删改快，查找慢。
方法：

1. 增加：void addFirst(E e)     void addLast(E e) 
2. 获取：E getFirst()     E getLast()
3. 删除：E removeFirst() remove()     E removeLast()



### Vector类

方法：`Enumeration<E>     elements()`     返回一个

## set接口

特点：唯一，无序号,也无顺序。

### HashSet类

特点：非线程安全，允许元素为null。对象需重写hashCode和equals，当equals相等时必须hashCode也相等，反之则不必然。在集合中，比如HashSet中，要求放入的对象不能重复，怎么判定呢？首先会调用hashcode，如果hashcode相等，则继续调用equals，如果equals也相等，则认为重复。

方法：1、增加：`boolean add(E e) `   未包含则加
不存在get方法，无法用普通for循环，只能用增强for循环遍历。



### TreeSet类

特点：可以对元素进行排序，不同步的。判断唯一性的方式，是根据compareTo方法的返回值，为0即相同元素，不共存。

比较方法有二种：

1、类实现comparable 

2、比较器comparator,并通过构造函数加载.

自然排序:方法一,定制排序:方法二.

## Iterator接口

特点：凡是Collection派生的都有iterator()方法并返回Iterator对象
方法：

`boolean hasNext()`是否有下一个元素
`E next()`返回下一个元素，并下移一位
`void remove()`删除最后返回的一个元素

## Map<K,V>接口

特点：key-value映射存储，key不要求有序，不允许重复，value不要求有序，允许重复
方法：

1、增加：`V put(K key,V value)`若key有旧value则被替代 
2、删除：`V remove(Object key)`返回value，没有则返回null     void clear()
3、获取：`V get(Object key)` `Set(K) keySet()` `Collection(V) values()`
4、查找：`boolean containsKey(Object key)` ` boolean containsValue(Objet value)`     `boolean isEmpty()` `Set<Map.Entry<K,V>>` `entrySet()`     获得Map内部的Entry接口对象的Set集合



### 内部接口：Interface Map.Entry<K,V>

```java
K    getKey()
V    getValue()
V    setValue(V value)
default V     replace(K key, V value)  当K不存在时,返回Null
boolean     replace(K key, V oldValue, V newValue) 当K和V匹配时,才会执行替换
```



### HashMap<K,V>类

特点：内部结构是哈希表，不同步，允许null作为键，可以作为值。 查询指定元素效率高,键自然排序



### Hashtable<K,V>类

特点：内部结构是哈希表，同步，不允许null作为键，可以作为值。



### Properties类

特点：键和值都为字符串类型，集合中的数据可以在流中传输。
方法：     

```java
Set<String> StringPropertyNames() 返回一个key值的set集合
String getProperty(String key) 返回指定键所对应的属性
String getProperty(String key， String default) 返回指定键所对应的属性,如果没有则返回default
void list(PrintStream out)  将属性列表输出到指定的输出流
void store(OutputStream out,String comments)     把properties写入到流中,comments文件属性信息可以为空
void store(Writer writer,String comments)
void load(InputStream inStream)     把流的数据写入到properties
void load(Reader reader)
```



### TreeMap<K,V>类

特点：内部结构是二叉树，不同步，可以对键进行排序。

## comparable\<T>接口

方法：

`int compareTo(T o) `    返回一个负数，0，正数来表示小于，等于，大于

### comparator\<T>接口

方法：

`int compare(T o1,T o2)`     根据第一个参数小于，等于，大于第二个参数返回负数，零，正数。
`boolean equals(Object obj)`     不用重写的原因是实现类都会继承Object的equals()方法 

## collections类

### 方法：

```java
static void<T extends Comparable<? super T>>    sort(List<T> list)     
// 对实现了comparable接口的元素的list集合进行排序
static <T> Comparator<T>    reverseOrder()     
// 返回一个逆转了实现comparable的对象的自然顺序的比较器
static <T> Comparator<T>    reverseOrder(Comparator<T> cmp)     
// 返回一个指定比较器的逆转后的比较器
static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key)    
// 查找返回索引
static <T> void    fill(List<? super T> list, T obj)     
// 用T替换list中的所有元素          
static <T> Enumeration<T>    enumeration(Collection<T> c)      
// 把Collection转换成枚举
static <T extends Object & Comparable<? super T>>   T    max(Collection<? extends T> coll) 
// 返回最大值(最小值换成min)
static <T> T    max(Collection<? extends T> coll, Comparator<? super T> comp)      
// 通过比较器返回最大值(最小值换成min)
static <T> boolean    replaceAll(List<T> list, T oldVal, T newVal)     
// 使用另一个值替换列表中所有的指定值
```

## 其他

### 对于集合内存储null

- List 集合可以存储null，添加几个，存储几个；
- Set集合也可以存储null，但只能存储一个，即使添加多个也只能存储一个；
- HashMap可以存储null键值对，键和值都可以是null，但如果添加的键值对的键相同，则后面添加的键值对会覆盖前面的键值对，即之后存储后添加的键值对；
- Hashtable不能碰null，不管是值还是键，一见null就报空指针。



### 线程安全的同步的类：

- vector：就比arraylist多了个同步化机制（线程安全）
- statck：堆栈类，先进后出
- hashtable：就比hashmap多了个线程安全
- enumeration：枚举，相当于迭代器



### 静态导入

在 import 后加 static 可以导入类中的静态成员. 
import static 包名.类名.静态成员变量;
import static 包名.类名.静态成员函数;