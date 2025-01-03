# 제어의 역전 IoC(Inversion of Control)

우리가 프레임워크 없이 개발할 때에는 객체의 생성, 설정, 초기화, 메소드 호출, 소멸(이하 객체의 생명주기)을 프로그래머가 직접 관리한다.<br> 
또한 전통적인 프로그래밍에서는 외부 라이브러리를 사용할 때, 개발자가 직접 외부 라이브러리를 호출하는 형태로 이용한다.<br>
하지만, **프레임워크**를 사용하면 객체의 생명 주기를 모두 프레임워크에 위임할 수 있다.<br> 
즉, 외부 라이브러리가 프로그래머가 작성한 코드를 호출하고, 흐름을 제어한다.

이와 같이 개발자가 작성한 객체나 메서드의 제어를 개발자가 아니라 외부에 위임하는 설계 원칙을 제어의 역전이라고 한다.<br> 
즉, **프레임워크**는 **제어의 역전** 개념이 적용된 대표적인 기술이라고 할 수 있다.

좀 더 간단히 이야기해보자. 전통적인 방식으로 라이브러리를 사용하는 것은 우리의 프로젝트의 일부분으로서 라이브러리를 가져와 우리가 직접 제어하는 것이다.<br> 
반면 IoC는 우리의 코드가 프레임워크의 일부분이 되어 **프레임워크에 의해 제어되는 것** 이라고 생각하면 될 것 같다.

어플리케이션의 제어 책임이 프로그래머에서 프레임워크로 위임되므로, 개발자는 핵심 비즈니스 로직에 더 집중할 수 있다는 장점이 있다.

- 기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고, 연결하고, 실행했다.<br>
한마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종했다. 개발자 입장에서는 자연스러운 흐름이다.<br>
- 반면에 `AppConfig`가 등장한 이후에 구현 객체는 자신의 로직을 **실행**하는 역할만 담당한다. 프로그램의 **제어 흐름은 이제 AppConfig가 가져간다**.<br> 
예를 들어서 `OrderServiceImpl` 은 필요한 인터페이스들을 호출하지만 어떤 구현 객체들이 실행될지 모른다.<br>
- 프로그램에 대한 제어 흐름에 대한 권한은 모두 AppConfig가 가지고 있다. 심지어 `OrderServiceImpl` 도 AppConfig가 생성한다.<br> 
그리고 AppConfig는 `OrderServiceImpl` 이 아닌 OrderService 인터페이스의 다른 구현 객체를 생성하고 실행할 수 도 있다.<br> 
그런 사실도 모른체 `OrderServiceImpl` 은 묵묵히 자신의 로직 을 실행할 뿐이다.<br>
이렇듯 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 **외부에서 관리하는 것**을 제어의 역전(IoC)이라 한다.

- 프레임워크가 내가 작성한 코드를 제어하고, 대신 실행하면 그것은 프레임워크가 맞다. (`JUnit`)
- 반면에 **내가 작성한 코드가 직접 제어의 흐름을 담당**한다면 그것은 프레임워크가 아니라 **라이브러리**다.

<br>

# 의존관계 주입 DI(Dependency Injection)
IoC는 DI(Dependency Injection)과 밀접한 관련이 있다. DI는 IoC 원칙을 실현하기 위한 여러 디자인패턴 중 하나이다.<br> 
IoC와 DI 모두 객체간의 결합을 느슨하게 만들어 유연하고 확장성이 뛰어난 코드를 작성하기 위한 패턴이다.

- `OrderServiceImpl` 은 `DiscountPolicy` 인터페이스에 의존한다. 실제 어떤 구현 객체가 사용될 지는 **모른다**.
- 의존관계는 **정적인 클래스 의존 관계와, 실행 시점에 결정되는 동적인 객체(인스턴스) 의존 관계** 둘을 분리해서 생각해야 한다.

### 정적인 클래스 의존관계
클래스가 사용하는 import 코드만 보고 의존관계를 쉽게 판단할 수 있다. 정적인 의존관계는 애플리케이션을 실행하지 않아도 분석할 수 있다. 
클래스 다이어그램을 보자
`OrderServiceImpl` 은 `MemberRepository` , `DiscountPolicy` 에 의존한다는 것을 알 수 있다.
그런데 이러한 클래스 의존관계 만으로는 실제 어떤 객체가 `OrderServiceImpl` 에 주입 될지 알 수 없다.

### 클래스 다이어그램
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/39196c0e-c1f4-421a-a944-ae5b92d83972" width="60%"/><br>

### 동적인 객체 인스턴스 의존 관계
애플리케이션 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계다.

### 객체 다이어그램
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/f8ae1663-5ce6-4fdd-9534-c3d09238c52f" width="60%"/><br>

- 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것을 **의존관계 주입**이라 한다.
- 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결된다.
- 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변 경할 수 있다.
- 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경 할 수 있다.
