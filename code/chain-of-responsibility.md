# Chain of Responsibility pattern

특정 요청을 순차적으로 처리할 때 보통 command 패턴을 사용하는데, 그 안에 책임의 제한을 둘 경우 chain of responsibility 패턴을 쓰면 좋을 것 같다.
이건 공부해본 적이 없다. 비교를 해보고 써봐야 또 사용할 수 있을 것 같다.

## 일단 Command 패턴

command 패턴은 명령을 그냥 순차적으로 처리할 수 있도록 하면 된다. 쉽고 재미있는 패턴이다. 명령의 집합체를 단일 메서드로 실행시킨다.

아래처럼 개발이랑 디자인 업무가 있고, 업무요청을 한다.
우리 디자이너는 프론트엔드 개발도 할 수 있지만, 업무요청을 할 수는 없다.

```java
public class Development {
	public void createBusinessLogic(){ ... }
	public void createWebPage(){ ... }
}

public class Design {
	public void drawLine(){ ... }
	public void createWebPage(){ ... }
	...
}
```

```
public interface Task {
	void execute();
}

// 백엔드 개발
public class BackEndTask implements Task {
	private Development development;
	
	public void execute(){
		development.createBusinessLogic();
	}
}

// 프론트엔드 개발
public class FrontEndTask implements Task {
	private Development development;
	
	public void execute(){
		development.createWebPage();
	}
}	

// 디자인 업무
public class DesignTask implements Task {
	private Design design;
	
	public void execute(){
		design.drawLine();
	}
}
```

```java
List<Task> taskList = new ArrayList<>();
taskList.add( new BackEndTask() );
taskList.add( new FrontEndTask() );
taskList.add( new DesignTask() );

for ( Task task: taskList ){
	task.execute();
}
```


## Chain of resposibility

command 패턴이랑 단순 구현차이인가? 싶었는데, 역할에 제한을 걸수가 있다. 순차적으로 처리는 하되, 처리를 할 수 없으면 다음으로 넘기는 것이다.


```java
public abstract class Employee {
	public static int BACKEND = 0;
	public static int FRONTEND = 1;
	public static int DESIGN = 2;
	
	protected int type;
	protected Employee nextEmployee;
	
	public Employee setNextEmployee ( Employee employee ){
		this.nextEmployee = employee;
		return this;
	}
	
	// 이 일을 수행할 수 없는 type이라면, 다음 task로 넘어간다.
	public void execute( int type, Spec spec ){
		if ( this.type >= type ){
			doWork( spec );
		}
		if ( nexTask != null ){
			nextEmployee.execute( type, spec );
		}
	}
	
	protected abstract void doWork( Spec spec );
}
```

```java
public class FrontEndDeveloper extends Employee {
	public FrontEndDeveloper( int type ){
		this.type = type;
	}
	
	protected void doWork( Spec spec ){ ... }
}

public class Designer extends Employee {
	public Designer( int type ){
		this.type = type;
	}
	
	protected void doWork( Spec spec ){ ... }
}
```

```java
// 실행
Employee employee = new FrontEndDeveloper( Employee.FRONTEND )
	.setNextEmployee( new Designer( Employee.DESIGN ) );

// 우리회사 디자이너는 괴수라서 디자인부터 백엔드까지 다 할 수 있다.
employee.execute( Employee.DESIGN, designSpec );
employee.execute( Employee.BACKEND, backendSpec );
```

## 결론

chain of resposibility 패턴은 언제 쓸지 모르겠고, 애써 쓰려고 해도 다른 방식으로 구현하는게 더 편할 것 같다. 예제로 Logger의 LogLevel이 많던데, 이건 COR패턴으로 깔끔하게 구현되는 것 같다. 아마도 나는 비슷한 상황에서 List에 Log를 담고, loop를 돌릴 것 같고 그 loop 내에서 LogLevel을 체크할 것 같다.;;; 