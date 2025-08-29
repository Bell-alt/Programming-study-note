# Programming-study-note
Spring
# 의존성주입(DI = Dependency Injection)
A가 B를 필요로 하는 상황에서 A가 B를 만들지 않고 외부에서 주입받는 것을 의미함
이유 : 
class A {
    private B b = new B();  // 직접 만들면
}
-> A와 B가 직접적으로 연결되어 유연성 떨어짐
-> B를 다른 구현체로 바꾸거나 테스트하기 어려움

해결 : DI주입
class A {
    private B b;
    public A(B b) {          // 외부에서 주입
        this.b = b;
    }
}
->A는 B가 있어야 한다만 알고 누가 B를 제공할지는 모름
-> Spring 컨테이너가 B 객체를 만들어서 A에 넣어줌

DI 방식
1.생성자주입(Constructor Injection)
class A {
    private B b;
    public A(B b) { this.b = b; }
}
: 객체 생성 시점에 주입 -> 불변성 보장, 테스트 용이

2.세터주입(Setter Injection)
class A {
    private B b;
    public void setB(B b) { this.b = b; }
}
:나중에 주입 가능 -> 선택적 의존성에 적합

# Spring에서 A가 B를 의존한다
A라는 객체가 동작하기 위해 B라는 객체가 꼭 필요하다는 의미

1.A가 B를 직접 사용하는 경우
class B {
    public void doSomething() {
        System.out.println("B 실행");
    }
}

class A {
    private B b = new B();  // A 안에서 B를 직접 생성 → A는 B에 의존
    public void work() {
        b.doSomething();
    }
}
-> A는 자기일을 하려면 B 객체가 꼭 필요하며, B가 없으면 A는 work()도 할수없음- 이럴때 A가 B에 의존한다라고 함

2.Spring에서 의존성 주입(DI) 적용

class A {
    private B b;

    // 생성자 주입
    public A(B b) {
        this.b = b;
    }

    public void work() {
        b.doSomething();
    }
}

Spring XML
<bean id="b" class="B"/>
<bean id="a" class="A">
    <constructor-arg ref="b"/>  <!-- A가 B에 의존하므로 스프링이 B를 대신 넣어줌 -->
</bean>

정리
*의존한다 = A가 혼자서는 못쓰고 B객체가 있어야 정상적으로 작동



