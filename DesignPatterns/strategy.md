# 策略模式(strategy)

**定义**：定义一组算法,将每个算法都封装起来,并且使他们之间可以互换.
**优点**：算法可以自由切换,避免使用多重条件判断,扩展性良好
**缺点**：策略类数量增多,所有的策略类都需要对外暴露
**使用场景**：多个类只有在算法上不同的场景,算法需要自由切换的场景,需要屏蔽算法规则的场景

### 角色:

**封装角色**：上下文角色,起承上启下封装作用,屏蔽高层模块对策略,算法的直接访问封装可能存在的变化
**抽象策略角色**：定义每个策略或算法必须具有的方法和属性.
**具体策略角色**：实现具体的算法.



**策略枚举**

```java
public enum Calculator{
    ADD("+"){
        public int exec(int a,int b){
            return a + b;
        }
    },
    SUB("-"){
        public int exec(int a,int b){
            return a - b;
        }
    };
    
    /** 存放符号标志 */
    String value = "";
    
    private Calculator(String value){
        this.value = value;
    }
    public String getValue(){
        return this.value;
    }
    
    /**  
     * @Description  抽象算法
     */
    public abstract int exec(int a,int b);
}
```


**调用**

```java
Calculator.ADD.exec(a,b);
Calculator.SUB.exec(a, b);
```