# 객체지향의 SOLID

경험상 SOLID 원칙을 준수하지 않는다고 해서 개발을 할 수 없는 것은 아니다. SOLID 원칙을 하나도 고려하지 않은 체로 개발하는 것도 많이 겪었기 때문이다. -_-;

누구를 위한 것인가? 개발자를 위한 것이다. 리팩토링, 디자인패턴 모두 개발자가 삽질을 최대한 덜하게 만들기 위한 것이다. waterfall 개발 방식으로 기획, 디자인, 개발, 검수, 끝으로 프로젝트와 작별한다면 SOLID, 리팩토링, 디자인패턴 모두 고려하지 않고 만들어도 된다. 그 프로젝트를 유지보수 하는 사람이 개고생하는 것이지;;;

근데 이미 만든 것에 계속 무언가 추가가 되는, 솔루션을 만들고 있는 것이라면 SOLID 원칙을 고민하지 않은 체 그냥 개발하다가는 스스로 지쳐버린다. 그리고 내가 나가는 순간, 그 후임자로부터 욕을 많이 먹을 수도 있다.;;;

## SRP, 단일책임원칙

클래스는 하나의 책임만 가지고, 그 클래스가 제공하는 기능들은 모두 하나의 책임을 수행하는데 집중되어야 한다는 원칙

software component를 분리할 때 그 기준은 책임, 역할등이고, 분리하는 목적은 복잡도, 결합도의 감소에 있다. 분리만 잘해서 사용하더라도 낮은 결합도, 높은 응집도를 갖게 되는 소프트웨어를 만들 수 있고, 궁극적으로 변경 요구에 대해 개발자가 뻘짓을 덜하고 유연하게 수정할 수 있게 된다.

## OCP, 개방폐쇄의 원칙

클래스, 메소드 등이 확장에는 열려있고, 변경에는 닫혀있어야 한다는 것으로 추상화와 다형성을 잘 활용해야 이 원칙을 유지할 수 있다. OCP는 interface를 활용하는 방법으로 대부분 준수할 수 있다.

## LSP, Liskov Substitution Principle

>if S is a subtype of T, then objects of type T may be replaced with objects of type S (i.e. an object of type T may be substituted with any object of a subtype S) without altering any of the desirable properties of T (correctness, task performed, etc.)

S 클래스가 T 클래스의 서브 클라스라면, 클라이언트쪽에서는 T를 S로 치환해서 사용하더라도 아무런 문제가 없어야 한다는 것이다. Java에서 Collection 같은 것이 유사하지 않을까? 

여튼 전형적인 문제는 직사각형 클래스를 구현상속 받은 정사각형 클래스를 만들어두었는데, width, height의 setter메소드를 상속받다보니 문제가 생기는 것

이 문제는 도메인 모델링부터 잘 했으면 안생길 것이다. 애초에 직사각형 클래스가 제공하는게 단순히 값만 제공하는 value object라면 불변 객체로 만들었어야지...

예제를 찾아보다가 찾은 또 다른 위배 문제 예제

```java
public class Duck {
    protected boolean starve = true;
    public void sing() {
        System.out.println("quack quack");
    }
    public void eat() {
        System.out.println("munch munch");
        this.starve = false;
    }
    public boolean isStarve() {
        return this.starve;
    }
}

public class ToyDock extends Duck {
    @Override
    public void eat() {
        System.out.println("toy duck cannot eat!");
    }
}
```

ToyDock은 항상 먹어도 먹어도 배고프다. Duck은 한번 먹이면, 더 이상 배고프지 않아서 괜찮다.

## ISP, Interface Segregation Principle

클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 한다는 원칙이다. 
간단하게, 대형 인터페이스 하나 만들고, 그걸 인터페이스 상속 받은 대형 클래스 하나 만들지 말라는 것

보통 개발할 때 많이 일어난다. Readable, Writeable 뭐 이런식으로 역할을 수행하는 인터페이스를 만들고, 필요에 따라 조합해서 쓰란 얘기

```java
interface Printer extends Writeable
interface Scanner extends Readable
interface MultiFunctionCopier extends Writeable, Readable

class HPPrinter extends Printer {...}
class CannonScanner extends Readable {...}
class SamsungCopier extends MultiFunctionCopier {...}
```

## DIP, Dependency Inversion Principle

상위 계층이 하위 계층에 의존하는 관계를 역전시켜서 상위 계층이 하위 계층의 구현으로부터 독립되도록 만든다. 즉, 모듈을 분리해서 관리하고 의존시키지마라

1. 상위모듈은 하위모듈에 의존해서는 안된다. 둘다 추상화에 의존해야 한다.
2. 추상화는 세부사항에 의존해서는 안된다. 세부사항이 추상화에 의존해야 한다.

1, 2번 둘다 서로서로 추상화에 의존해야 한다. 라는 얘기

## Practice

의식적으로 개발할 때마다 고려하지 않으면 필시 까먹는다. 동료가 SOLID 원칙을 고려하지 않더라도, 그 회사의 기업문화가 이런 것을 중요하게 생각하지 않더라도 스스로 계속해서 상기해서 고민하고 개발해야만 한다.
