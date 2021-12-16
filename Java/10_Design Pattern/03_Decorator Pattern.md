# 데코레이터 패턴 (Decorator Pattern)

> OCP(개방 폐쇄 원칙) & DIP(의존 역전 원칙) 을 활용한 설계 패턴



### ***<u>"메서드의 호출의 반환값에 변화를 주기 위해 중간에 장식자를 두는 패턴"</u>***



**데코레이터**는 **도장/도배업자**로 직역할 수 있다. (장식자 정도로 이해하자)

프록시 패턴과 거의 같다고 볼 수 있지만 다른점은 

**프록시 패턴**은 최종적으로 돌려받는 **반환값을 조작하지 않고, 그대로 전달**

**데코레이터 패턴**은 최종적으로 돌려받는 **반환값에 장식을 덧입힘**





## 예시

*(IService)*

```java
package work;

public interface IService {
    String runSomething();
}
```

*(Decorator)*

```java
package work;

public class Decorator implements IService{
    IService service;
    @Override
    public String runSomething() {
        checkSomething();
        service = new Service();
        return "Wow! " + service.runSomething();
    }

    private void checkSomething(){
        System.out.println("Check Something...");
    }
}

```

*(Service)*

```java
package work;

public class Service implements IService{
    @Override
    public String runSomething() {
        return "Hi there!";
    }
}
```

*(Client)*

```java
package client;

import work.IService;
import work.Proxy;

public class Client {
    public static void main(String[] args) {
        IService proxy = new Proxy();
        System.out.println(proxy.runSomething());
    }
}
```

> 출력 결과:
>
> Check Something...
>
> Wow! Hi there!



