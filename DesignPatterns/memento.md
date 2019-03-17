# 备忘录模式(memento)

**定义**：在不破坏封装性的前提下,捕获一个对象的内部状态,并在该对象之外保存这个状态.这样以后就可将该对象恢复到原先保存的状态.

### 角色：

**Originator发起人角色**：记录当前时刻的内部状态,负责定义哪些属于备份范围的状态,负责创建和恢复备忘录数据.
**Memento备忘录角色**：负责存储发起人对象的内部状态,在需要的时候提供发起人需要的内部环境.
**Caretaker备忘录管理员角色**：对备忘录进行管理,保存和提供备忘录.

**扩展**：clone方式的备忘录,多状态的备忘录,多备份的备忘录,封装备忘录



**备忘录接口**

```java
public interface Memento {
}
```



**工具类**

```java
public class BeanUtils {
    /**  
     * @Description  更新bean的属性并存在提供的map里
     * @param bean  新的bean状态
     * @param propMap  存放bean属性的Map类
     */
    public static void restoreProp(Object bean, HashMap<String,Object> propMap) {
        try {
            BeanInfo beanInfo = Introspector.getBeanInfo(bean.getClass());
            PropertyDescriptor[] descriptors = beanInfo.getPropertyDescriptors();
            for (PropertyDescriptor des:descriptors) {
                String fieldName = des.getName();
                if (propMap.containsKey(fieldName)) {
                    Method setter = des.getWriteMethod();
                    setter.invoke(bean,propMap.get(fieldName));
                }
            }
        } catch (IntrospectionException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (IllegalArgumentException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
    }
    
    /**  
     * @Description  把bean的属性存放进Map并返回
     * @param bean  需要处理的bean
     * @return   返回的Map
     */
    public static HashMap<String,Object> backupProp(Object bean) {
        HashMap<String,Object> result = new HashMap<String,Object>();
        try {
            BeanInfo beanInfo = Introspector.getBeanInfo(bean.getClass());
            PropertyDescriptor[] descriptors = beanInfo.getPropertyDescriptors();
            for (PropertyDescriptor des:descriptors) {
                String fieldName = des.getName();
                Method getter = des.getReadMethod();
                Object fieldValue = getter.invoke(bean);
                if (!fieldName.equalsIgnoreCase("class")) {
                    result.put(fieldName, fieldValue);
                }
            }
        } catch (IntrospectionException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (IllegalArgumentException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        return result;
    }
}
```



**发起人角色**(内部封装备忘录类)

```java
public class Originator {
    /** javaBean属性 */
    private String state1 = "";
    private String state2 = "";
    private String state3 = "";
    public String getState1() {
        return state1;
    }
    public void setState1(String state1) {
        this.state1 = state1;
    }
    public String getState2() {
        return state2;
    }
    public void setState2(String state2) {
        this.state2 = state2;
    }
    public String getState3() {
        return state3;
    }
    public void setState3(String state3) {
        this.state3 = state3;
    }
    
    /**  
     * @Description  创建备忘录对象
     */
    public Memento createMemento() {
        return new ConcreteMemento(BeanUtils.backupProp(this));
    }
    
    /**  
     * @Description  更新备忘录对象
     */
    public  void restoreMemento(Memento memento) {
        BeanUtils.restoreProp(this,((ConcreteMemento)memento).getStateMap());
    }
    
    /**  
     * @Description  转换成字符串输出
     */
    public String toString() {
        return "state1 = " + this.state1 + "\nstate2 = " + this.state2 + "\nstate3 = " + this.state3;
    }
    
    /**
     * @Description  备忘录内部类
     * @author  OrezWodahs
     * @date 2018年6月20日 下午4:08:20 
     *  
     */
    private class ConcreteMemento implements Memento {
        /** bean属性容器 */
        private HashMap<String,Object> stateMap;
        
        /**
         * @Description  通过构造,保存bean的属性至map
         */
        private ConcreteMemento(HashMap<String,Object> map) {
            this.stateMap = map;
        }
    
        private HashMap<String, Object> getStateMap() {
            return this.stateMap;
        }
    
        private void setStateMap(HashMap<String,Object> stateMap) {
            this.stateMap = stateMap;
        }
    } 
}
```



**备忘录管理类**

```java
public class Caretaker {
    /** 备忘录容器 */
    private HashMap<String,Memento> memMap = new HashMap<String,Memento>();
    
    /**  
     * @Description  获取一个备忘录
     * @param idx  备忘录标识
     * @return   对应备忘录
     */
    public Memento getMemento(String idx) {
        return this.memMap.get(idx);
    }
    
    /**  
     * @Description  存储一个备忘录
     * @param idx  备忘录标识
     * @param memento   备忘录
     */
    public void setMemento(String idx,Memento memento) {
        this.memMap.put(idx, memento);
    }
}
```