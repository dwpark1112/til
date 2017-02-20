## 디자인 패턴

자주 발생하는 설계, 구조상의 문제를 해결하기 위한 잘 알려진 패턴

**참고**

* tutorial points - <https://www.tutorialspoint.com/design_pattern>
* GoF Design Patterns Using Java (Part 1), 2017-02-02 - <https://dzone.com/articles/gof-design-patterns-using-java-part-1>


Behavioral Patterns
----------

**스트래티지 패턴 (strategy pattern)**

- 교환 가능한 행동을 캡슐화하고 위임을 통해서 어떤 행동을 사용할지 결정한다.
- [이 곳 설명이 참 좋음](https://dzone.com/articles/design-patterns-strategy)

Structural Patterns
----------
###어댑터 패턴 (adaptor pattern/wrapper pattern)

- 객체를 감싸서 다른 인터페이스를 제공한다.
  - 상속을 통해서 하거나, 위임을 통해서 하거나 둘다 가능, java에서는 다중상속 위장인가?
  - 그런데 상속으로 통해서 하면 의도치 않게 Adaptee 메소드가 노출될 수 있겠다.
- Querydsl-mongodb에서 커스텀 쿼리 만들때랑 유사한 듯 하다.
- 남이 만들어둔 거 재 사용할 때도 좋다.
![](http://java.dzone.com/sites/all/files/adapter_pattern_0.PNG)

###데코레이터 패턴 (decorator pattern)

- 객체에 책임을 덧붙이는 패턴으로, 기능 확장이 필요할 때 서브클래싱 대신 쓸 수 있는 유연한 대안
- `Open/Closed priciple`을 지킬때 매우 좋은 것 같다.
![](http://java.dzone.com/sites/all/files/decorator_pattern_0.png)

> 개인적으로 Adapter pattern이 필요한 상황에서는 왠지 decorator pattern을 쓸까? adapter를 쓸까 고민할 것 같다.
> > - 외부 라이브러리를 사용하는 경우에는 adapter가 좋을 것 같고,
> > - 내부 개발 로직에 기능 확장 따위를 하는 경우에는 decorator가 좋을 것 같다.

###퍼사드 패턴 (facade pattern)

- 일련의 클래스에 대해서 간단한 인터페이스를 제공한다.
- 예를 들어, 여러가지 API를 호출해서 통일된 결과를 배출해야하는 API에서는 여러가지 API를 통합한 단일 API를 제공한다. 
- API 예시로는 기관 가입 신청 시, `회원가입 + 기관 생성 + 기관 소속 + 승인`까지의 API통합

![](http://java.dzone.com/sites/all/files/facade_seq.PNG)

###브리지 패턴 (bridge pattern)

- 추상체와 구현체에 브릿지 구조를 제공함으로서 decoupling을 유도하는 것?
![](http://java.dzone.com/sites/all/files/bridge_pattern.PNG)

- 파생 클래스가 폭발적으로 증가하는 상황을 막기 위한 패턴, 객체에서 변화되는 것을 별도로 분리해서 관리하는 것이다.
- `The Bridge pattern is a composite of the Template and Strategy patterns.`


```
> 이런 상황이 발생하면?

	                   ----Shape---
	                  /            \
	         Rectangle              Circle
	        /         \            /      \
	BlueRectangle  RedRectangle BlueCircle RedCircle

```	

```		
> refactor to	

          ----Shape---                        Color
         /            \                       /   \
Rectangle(Color)   Circle(Color)           Blue   Red

```	


###컴포지트 패턴 (composite pattern)

- 객체들의 관계를 트리 구조로 구성하여 부분-전체 계층을 표현하는 패턴으로, 사용자가 단일 객체와 복합 객체 모두 동일하게 다루도록 한다. 

![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Composite_UML_class_diagram_%28fixed%29.svg/960px-Composite_UML_class_diagram_%28fixed%29.svg.png)

- 생각보다 재미있는 패턴이다. 처음에 GoF의 정의는 별로 와닿지 않는다.
  - 사용자가 단일 객체와 복합객체를 동일하게 다루겠다는 것은 알겠는데, 트리구조로 구성한다는건 이해가 되질 않았는데, 생각해보니 아래 그림처럼 Composite자체도 Composite으로 포함될 수 있으니, 트리 구조로 구성할 수 있겠다.

```
 
      Composite
      /       \
    Leaf    Composite
             /      \
          Leaf      Leaf
                
```


###플라이웨이트 패턴 (flyweight pattern)

- 동일하거나 유사한 객체들 사이에 가능한 많은 데이터를 서로 공유하여 사용하도록 하여 메모리 사용량을 최소화하는 패턴

![](https://dzone.com/sites/all/files/flyweight_pattern.png)

- 이분 자료가 가장 좋은 것 같다. <http://blog.javarouka.me/2012/02/javascripts-pattern-3-flyweight-pattern.html>
  - 잘못 쓰면 코드 가시성이 떨어질 수 있다고 한다.
  - 대부분 게임 예제가 많은데, 게임에서는 괜찮게 쓰일 것 같다.
- 서버 개발하면서 이 패턴을 써야할 유사한 상황이 없었던 것 같다. 아님 이해를 잘 못했던가;;


###프록시 패턴 (proxy pattern)

- 객체를 감싸서(대리인이다.) 그 객체에 대한 접근을 제어한다.
  - java.rim 패키지가 proxy pattern을 사용하고 있다고 한다.

![](http://java.dzone.com/sites/all/files/proxy_pattern.png)

sequence

![](http://java.dzone.com/sites/all/files/proxy_seq.png)

> 딱히 어려운 건 아니지만, 왠지 클라이언트 사이드에서 더 많이 쓰일 패턴일 것 같다.
> 객체를 감싸는 점에서 adapter pattern이랑 같은 것 같은데, adapter pattern은 realsubject와 다른 인터페이스를 제공한다.
> 객체를 감싸는 점에서 decorator pattern이랑 같은 것 같은데, decorator pattern은 객체의 기능(책임)을 추가한다. 제어 목적이 아니다.
