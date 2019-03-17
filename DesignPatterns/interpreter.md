# 解释器模式(interpreter)

**定义**：给定一门语言,定义它的文法的一种表示,并定义一个解释器,该解释器使用该表示来解释语言中的句子.
**优点**：扩展性好,只需增加非终结符类就行.
**缺点**：解释器模式会引起类膨胀,采用递归调用方法,效率问题.

### 角色：

**AbstractExpression抽象解释器**
T**erminalExpression终结符表达式**：实现与文法中的元素关联的解释操作,一般只有一个,但有多个实例,对应不同的终结符.
**NonterminalExpression非终结符表达式**：文法中的每条规则对应一个非终结表达式,根据逻辑的复杂程度而增加,原则上一个文法规则都对应一个非终结表达式.
**Context环境角色**：具体到例子中是HashMap代替



### 使用场景：

1. 重复发生的问题可以使用解释器模式
2. 一个简单语法需要解释的场景
3. 可以使用shell,JRuby,Groovy等脚本语言来代替解释器模式
4. Expression4J,MESP(Math Expression String Parser),Jep等开源解析工具包



#### 抽象解释器

```java
public abstract class AbstractExpression {
    public abstract Object interpret(Context context);
}
```



#### 终结符表达式

```java
public class TerminalExpression extends AbstractExpression {
    public Object interpret(Context context) {
        return null;
    }
}
```


#### 非终结符表达式

```java
public class NonterminalExpression extends AbstractExpression {
    public NonterminalExpression(AbstractExpression... Expression) {
       
    }
       
    public Object interpret(Context context) {
        return null;
    }
}
```


#### 客户类

```java
public class Client {
    public static void main(String[] args) {
        Context ctx = new Context();
        Stack<AbstractExpression> stack = null;
        for(;;) {
            // 进行语法判断,并产生递归调用
        }
        // 产生一个完整的语法树,由各个具体的语法分析进行解析
        AbstractExpression exp = stack.pop();
        // 具体元素进入场景
        exp.interpret(ctx);
    }
}
```