# Spring framework 4.x

이전까지는 Spring framework v3를 쓰고 있었다. 특정 라이브러리를 추가할 때마다 약간의 마이너 버전 업된 것이 필요하면 조금씩 변경했을 뿐, 버전 4로 이동은 하지 않았다. 일단 java 8의 람다를 모르기에 -_-;; 람다 표현식을 학습한 이후에 버전 업을 고려하고 있었다.

그런데 고맙게도 mongodb의 bulk operation을 써야 하는 상황이 발생했다. mongodb-java-driver의 버전만 올려도 되었지만, 이미 spring-data-mongodb와 querydsld을 쓰고 있는 상황에서 어차피 해야할 거 버전을 올리는게 낫겠다고 생각되었다. 

bulk operation은 spring-data-mongodb 1.9 버전부터 지원하기에 버전을 올릴 필요가 있었고, 의존성으로 querydsl도 버전업해야 했고, querydsl을 올리다보니 spring framework로 4버전으로 올려야 했다. 그래서 궁금해서 알아본 버전 4

## New features and Enhancements in Spring framework 4.0

<https://docs.spring.io/spring/docs/current/spring-framework-reference/html/new-in-4.0.html>

일단 2013년에 나왔고, 2017년 1월에 4.3.6.RELEASE까지 나왔다. 가장 큰 특징은 **java 8을 완전하게 지원**한다는 것이다. 또한 최소 Java SE 6 이상 되어야 한다.

Removed deprecated packages and methods
=====

deprecated된 것들은 빨리 빨리 고쳐써야지, 일단 된다고 그냥 냅두면 안된다. spring version 4에서는 deprecated 패키지는 모두 빼버리고, 많은 수의 클래스, 메서드 들도 빠졌다고 한다. 따라서 현재 프로젝트에 deprecated된 spring의 것들을 사용하고 있는 부분이 많다면 버전을 변경하는데 주의해야 한다. API diffrences report 제공 중

그 외에도 hibernate, ehcache, joda-time 등등 써드파티 의존성도 최소버전이 올랐다. 특히 json marsaller인 jackson은 2.0+를 써줘야 한다. jackson 2.0이랑 1.x랑 많이 차이난다. 이미 spring v3를 쓸때에 jackson 2.0으로 변경해두어서 다행히 잘 됨!

General Web Improvements
====

이전에는 RESTful API 만들때, controller layer의 메소드에는 `@ResponseBody` 애노테이션을 달아줘야 했는데, 이제는 `@RestController`라는 애노테이션을 controller layer의 class에 달아두면 더이상 responsebody를 코딩하지 않아도 된다.

또, `AsyncRestTemplate`라는 게 추가되었다. non-blocking asynchronouse를 지원하는 RestTemplate이다. 이것도 좋은것 같다. 이전에는 이게 없으니깐 RestTemplate를 wrapping 해서 비동기 메서드로 구현해두었다.

솔직히 위 2개는 있으면 좋고, 없어도 그만이다.

WebSocket, SockJS, and STOMP Messaging
====

새로운 `spring-websocket` 모듈은 JSR-356(Java API for WebSocket API)와 호환된다.

`spring-messaging` 모듈은 STOMP를 지원한다. STOMP는 처음 들어보는데, WebSocket sub-protocol이라고 한다. 이것 덕분에 controller 클래스 method가 `@RequestMapping` + `@MessasgeMapping` 이렇게 애노테이션을 달고 있으면, HTTP request와 WebSocket message도 처리할 수 있다고 한다. 괜찮네?


Testing Improvements
====

`spring-test`에서 deprecated된 코드들을 제거했고, 새 기능을 추가했다고 한다.

이건 뭐... 그냥 개발 편의성 파트인듯하고 있으면 좋다. 없으면 여기저기 누군가 해본 경험을 참고해야하니깐


