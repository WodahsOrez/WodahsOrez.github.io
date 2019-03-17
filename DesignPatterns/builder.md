# 建造者模式(builder)

定义:讲一个复杂对象的构建与它的表示分离,使得同样的构建过程可以创建不同的表示.
优点:封装性,容易扩展,便于控制细节风险
注意:建造者模式关注的是顺序,而工厂方法模式更多关注创建.

### 角色:

产品类:通常实现了模板方法模式.
抽象建造者:规范产品的组件,即规范产品制造的流程结构.
具体建造者:实现抽象类定义的方法,并且返回一组建好的对象.
导演类:负责安排已有模块的顺序,并告知建造类开始建造.

### 使用场景:

1.相同方法,不同执行顺序,产生不同的事件结果时.
2.多个部件或零件都可以装配到一个对象中,但是产生的运行结果又不相同时,可以使用该类.
3.产品类非常复杂,或者产品类中的调用顺序不同产生了不同的效能,这个时候使用建造者模式非常合适.



### 实例

**产品抽象类**

```java
public abstract class CarModel{
	private ArrayList<String> sequence = new ArrayList<String>();
	protected abstract void start();  //启动
	protected abstract void stop();   //停车
	protected abstract void alarm();  //喇叭
	protected abstract void engineBoom(); //引擎
	final public void run(){
		for(String tag:sequence){
			switch(tag.toLowerCase()){
				case "start" :
					this.start();
					break;
				case "stop" :
					this.stop();
					break;
				case "alarm" :
					this.alarm();
					break;
				case "engineBoom" :
					this.engineBoom();
					break;
			}
		}
	}
	

	final public void setSequence(ArrayList<String> sequence){
		this.sequence = sequence;
	}
}
```



**产品实现类**

```java
public class BenzModel extends CarModel{
	protected void start(){
		system.out.println("奔驰启动");
	}
	protected void stop(){
		system.out.println("奔驰停止");
	}
	protected void alarm(){
		system.out.println("奔驰喇叭");
	}
	protected void engineBoom(){
		system.out.println("奔驰引擎");
	}
}

public class BMWModel extends CarModel{
	protected void start(){
		system.out.println("宝马启动");
	}
	protected void stop(){
		system.out.println("宝马停止");
	}
	protected void alarm(){
		system.out.println("宝马喇叭");
	}
	protected void engineBoom(){
		system.out.println("宝马引擎");
	}
}
```



**抽象建造者**

```java
public abstract class CarBuilder{
	public abstract void setSequence(ArrayList<String> sequence);
	public abstract CarModel getCarModel();
}
```



**具体建造者**

```java
public BenzBuilder extends CarBuilder{
	private BenzModel benz= new BenzModel();
	public CarModel getCarModel(){
		return this.benz;
	}
	public void setSequence(ArrayList<String> sequence){
		benz.setSequence(sequence);
	}
}

public BMWBuilder extends CarBuilder{
	private BMWModel bmw= new BMWModel();
	public CarModel getCarModel(){
		return this.bmw;
	}
	public void setSequence(ArrayList<String> sequence){
		this.bmw.setSequence(sequence);
	}
}
```



**导演类**

```java
public class Director{
	private ArrayList<String> sequence = new ArrayList<String>();
	private BenzBuilder benzBuilder = new BenzBuilder();
	private BMWBuilder bmwBuilder = new BMWBuilder();
	public BenzModel getABenzModel(){
		this.sequence.clear();
		this.sequence.add("start");
		this.sequence.add("stop");
		this.benzBuilder.setSequence(sequence);
		return (BenzModel)this.benzBuilder.getCarModel();
	}
		public BenzModel getBBenzModel(){
		this.sequence.clear();
		this.sequence.add("engineBoom");
		this.sequence.add("start");
		this.sequence.add("stop");
		this.benzBuilder.setSequence(sequence);
		return (BenzModel)this.benzBuilder.getCarModel();
	}
		public BMWModel getCBMWModel(){
		this.sequence.clear();
		this.sequence.add("alarm");
		this.sequence.add("start");
		this.sequence.add("stop");
		this.bmwBuilder.setSequence(sequence);
		return (BMWModel)this.bmwBuilder.getCarModel();
	}
		public BMWModel getDBMWModel(){
		this.sequence.clear();
		this.sequence.add("start");
		this.bmwBuilder.setSequence(sequence);
		return (BMWModel)this.bmwBuilder.getCarModel();
	}
}
```