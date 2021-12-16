# 템플릿 콜백 패턴 (Template Callback Pattern)

> OCP(개방 폐쇄 원칙) & DIP(의존 역전 원칙) 을 활용한 설계 패턴





### ***<u>"전략을 익명 내부 클래스로 구현한 전략 패턴"</u>***



**템플릿 콜백 패턴**은 **전략 패턴의 변형**으로 DI(의존성 주입)에서 사용하는 특별한 형태의 전략 패턴이다.

템플릿 콜백 패턴은 전략 패턴과 모든 것이 동일한데, **전략을 익명 내부 클래스로 정의해서 사용한다는 특**징이 있다.



## 예시

*(Strategy)*

```java
package strategy;

public interface Coffee {
    public abstract void makeCoffee();
}
```

*(Context)*

```java
package context;

import strategy.Coffee;

public class Cafe {
    public void work(String recipe){
        System.out.println("Welcome!");
        doWork(recipe).makeCoffee();
        System.out.println("See you!");
    }

    private Coffee doWork(final String recipe){
        return new Coffee() {
            @Override
            public void makeCoffee() {
                System.out.println(recipe);
            }
        };
    }
}
```

*(Client)*

```java
package client;

import context.Cafe;

public class Client {
    public static void main(String[] args) {
        Cafe cafe = new Cafe();

        // 아메리카노 주세요
        cafe.work("Water + espresso");

        System.out.println();

        // 라떼 주세요
        cafe.work("Milk + espresso");
    }
}
```

> 출력 결과:
>
> Welcome!
> Water + espresso
> See you!
>
> 
>
> Welcome!
> Milk + espresso
> See you!



스프링은 위와같은 코드를 DI에 적극 활용하고 있다. 

**스프링을 이해하고 활용하기 위해서는 전략패턴, 템플릿 콜백 패턴을 잘 기억해 두어야 한다**