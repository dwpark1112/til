# Builder pattern

이건 정말 자주 쓰이고, 편해서 자주 쓰게 되는 패턴 중 하나이다.  

![](https://dzone.com/sites/all/files/builder_pattern.PNG)

처음에는 도메일 모델이 간단해서 대충 아래 처럼 만든다.

```java
public class User {
	private String email;
	private String name;
 
	public static User newInstance( String email, String name ) {
		User instance = new User();
  		instance.email = email;
  		instance.name = name;
  		
  		return instance;
 	}
 
 	private User(){}
}
```

그런데 사용자 정보에 달랑 저것만 있을리없고, 갑자기 서비스가 커져가면서 전화번호와 주소가 들어가야 하면 갈등이 시작된다. 새로운 생성자를 추가하든지, 이전처럼 생성하고 setter 메소드로 전화번호와 주소를 추가해야 한다. 그리고 계속 늘어나면 코드는 쓰기 어려워지고 보기에도 불편해진다.

```java
// User를 생성하는 어딘가
User user = User.newInstance( email, name );
user.setPhoneNo( number );
user.setAddress( address );
```

더 심각한 것은 새로 추가되는 어떤 값은 필수값이어야 하는 조건이 붙을 때다. 이때쯤 되면 고민스러워지는게 코드로 어떤 제약을 걸지 않으면 어디선가 오류가 생긴다. 내가 실수하거나 같이 개발하던 동료가 실수하게 된다.

크게 제약사항이 없는거라면 아래 처럼 호출해서 쓸 수 있도록 구현하면 된다.

```java
User user = new User.Builder()
		.setName( name )
		.setEmail( email )
		.setAddress( address )
		.build();
```

마지막 build 메소드에서 필수값 검증이나 포맷 검증이 필요하면 저기다가 구현해두면 편하다.
