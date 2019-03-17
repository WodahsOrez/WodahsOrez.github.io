# 观察者模式(observer)

**定义**：定义对象间一种一对多的依赖关系,使得每当一个对象改变状态,则所有依赖它的对象都会得到通知并被自动更新
**优点**：观察者和被观察者之间是抽象耦合,容易扩展,建立一套触发机制
**缺点**：开发效率和运行效率堪忧,推荐采用异步的方式.

### 角色

**Subject被观察者**：定义被观察者必须实现的职责,它必须能够动态的增加,取消观察者.一般是抽象类或实现类.
**Observer观察者**：观察者接收到消息后,即进行update操作,对接收到的信息进行处理
**ConcreteSubject具体被观察者**：定义被观察者自己的逻辑,同时定义对哪些时间进行通知
**ConcreteObject具体观察者**：每个观察者在接收到消息后的处理反应是不同,拥有自己的处理逻辑



**被观察者**

```java
public class Observable {
    /** Vector利用同步方法来线程安全，线程安全在多线程情况下不会造成数据混乱 */
    private Vector<Observer> obs = new Vector();;       

    /**  
     * @Description  新增一个观察者
     * @param o   
     */
    public synchronized void addObserver(Observer o) {
        if (o == null)
            throw new NullPointerException();
        if (!obs.contains(o)) {
            obs.addElement(o);
        }
    }
    
    /**  
     * @Description 删除观察者
     * @param o   
     */
    public synchronized void deleteObserver(Observer o) {
        obs.removeElement(o);
    }
    
    /**  
     * @Description  通知观察者 
     */
    public void notifyObservers() {
        for(Observer o :this.obs) {
            o.update();
        }
    }
}
```


**观察者**

```java
public interface Observer {
    /**  
     * @Description 更新方法
     */
    void update();
}
```