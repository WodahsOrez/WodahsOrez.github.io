# 责任链模式(Chain of Responsibility)

**定义**：使多个对象都有机会处理请求,从而避免请求的发送者和接受者之间的耦合关系.将这些对象连成一条链,并沿着这条链传递该请求,直到有对象处理它为止.

**优点**：把请求和处理分开.   

**缺点**：性能慢,调试不方便.



**抽象处理者**

```java
public abstract class AbstractHandler {
    /** 存放下一个处理者 */
    private AbstractHandler nextHandler;
    
    /**  
     * @Description  对请求作出处理
     */
    public final Response handleMessage(Request request) {
        Response response = null;
        if (this.getHandlerLevel().equals(request.getRequestLevel())) {
            response = this.echo(request);
        } else {
            if (this.nextHandler != null) {
                response = this.nextHandler.handleMessage(request);
            } else {
                // 当没有适当的处理者,业务自行处理
            }
        }
        return response;
    }
     
    /**  
     * @Description  设置下一个处理者
     */
    public void setNext(AbstractHandler handler) {
        this.nextHandler = handler;
    }
    
    /**  
     * @Description  获取处理者的处等级
     * @return   
     */
    protected abstract Level getHandlerLevel();
    
    protected abstract Response echo(Request request);
}
```