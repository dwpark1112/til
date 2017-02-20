# Template method pattern

쉽고 재미있는데, 활용도가 매우 많던 패턴

Gang Of Four 책에 나온 정의는 아래와 같다.

> Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithms structure.

- 순서대로, 메소드 내에 알고리즘 골격을 정의하고
- 구현은 서브 클래스에게 맡긴다.

템플릿 메서드는 알고리즘 구조의 변경없이 서브 클래스가 알고리즘의 특정 스텝을 재정의하게 해준다.  

실제 코드를 작성하다보면 비슷한데 약간 처리를 해야 하는 상황을 겪게 된다. 다 같은데 조금씩 코드 블럭 위아래로 다르다던지? 위아래는 같은데, 중간만 살짝 다른 경우가 있다. 보통 몇개 안되면 그냥 분기문으로 처리하는게 편한데, 3~4개가 넘어가면 분기문으로 처리하는 것보다 템플릿 메서드 패턴을 사용하는게 편하다.

먼저 GoF가 가르쳐준것처럼 알고리즘 골격만 만든다.

```java
public abstract class Domain {
	abstract void initialize();
	abstract void doSomething();
	
	public void run(){
		initialize();
		doSomething();	
	}
}
```

```java
public class MyDomain extends Domain{
	void initialize(){...}
	void doSomething(){...}
}

public class YourDomain extends Domain{
	void initialize(){...}
	void doSomething(){...}
}
```

예전에 `ISO 27789 — Audit trails for electronic health records`에서 상황별 감사기록 메시지를 XML로 생성해야 하는 개발을 담당한 적이 있었는데, 그때 템플릿 메서드 패턴을 사용해서 60여개의 클래스를 만들었던 기억이 난다. 60여개의 메세지는 Event, Participant, Details, Options 등으로 구성되어 있었는데 서로 각각 달라서 템플릿 메서드 패턴을 쓰는게 좋을 것 같다는 생각이 들었다.당시에는 그거 만들고 좋아했는데, 지금 생각해보면 그냥 XML 템플릿만 두고, 상황에 따라 XPath로 이리저리 값만 바꾸는게 훨씬 나았을것 같다.

## Template method vs Strategy pattern

템플릿 메서드랑 스트레티지 패턴이랑 둘은 매우 비슷해보인다. 이들 패턴의 차이점을 찾아보았는데, 찾아보며 사람들의 의견을 보다보니 접근이 틀렸다는 생각이 든다. 알고리즘 골격이 필요하면 Template method이고, 알고리즘 골격이 필요 없다면 그냥 strategy가 되는 것이다. -_-;

**알고리즘 골격이 필요 없는 경우**

예를 들면 데이터베이스를 교체하며 연결하는 경우이다. 단순히  connect 메서드 이것 밖에 없다면, strategy로 쓰면 되는 것이다. 알고리즘 골격은 없고, 그냥 로직만 담겨있고 그들간의 연산 순서가 중요한 것도 아니라면 이렇게 쓰면 되는 것이다.

```java
public interface Database {
	public void connect();
	public void close();
}

public class MySQL implements Database {
	public void connect(){ ... }
	public void close(){ ... }
}

public class MongoDB implements Database {
	public void connect(){ ... }
	public void close(){ ... }
}

public class Application {
	private Database database;
	
	public void connect() {
		database.connect();
	}
	
	public void disconnect() {
		database.close();
	}
}
```

**같은 예제에서 알고리즘 골격이 필요한 경우**

억지로 무언가 만들자면, Database 연결을 종료할 때 슬로우 로그를 뽑아서 어딘가로 전송하고 연결을 종료해야 한다면 아래처럼 구성할 수 있다. 

데이터베이스마다 슬로우로그를 가져오는 방법과 연결을 종료하는 방법이 다른 상황인데, 데이터베이스 연결중단 시에는 반드시 로그 전송을 해야하는 환경이라면 아래처럼 Template method를 쓰면 된다.

구현체랑 알고리즘을 구현한 메서드를 외부에 노출시키지 않고, 같은 패키지 내에 패키지 접근자로 처리하는 것도 좋은 방법, 노출되면 안되는 걸 노출시키면 쓰지마라고 해도 누군가는 쓴다. 스스로도 쓴다. -_-;

```java
public abstract class Database {
	public abstract void connect();
	abstract List<Log> getSlowLogs();
	abstract void disconnect();
	
	public void close() {
		List<Log> slowLogs = getSlowLogs();
		sendSlowLog( slowLogs );
		disconnect();
	}
	
	private void sendSlowlog( List<Log> slowLogs ){ ... }
}

class MySQL extends Database {
	public void connect(){ ... }
	List<Log> getSlowLogs(){ ... }
	void disconnect(){ ... }
}

class MongoDB extends Database {
	public void connect(){ ... }
	List<Log> getSlowLogs(){ ... }
	void disconnect(){ ... }
}

public class Application {
	private Database database;
	
	public void connect() {
		database.connect();
	}
	
	public void close() {
		database.close();
	}
}
```

## Strategy pattern to Template method

고민 거리는 이거다. 구현 예제나 다른 패턴과 결합해서 쓰는걸 보면, 대부분 Strategy도 interface가 아니라 abstract class로 만든걸 많이 보았는데 이러한 이유때문이 아닐까 싶기도 하다. 경험이 더 쌓이면 언젠가 풀리겠지

IDE에서만 코딩하다가 markdown으로 코드 쓰려면 힘들줄 알았는데, 해보니깐 더 편하다.;;