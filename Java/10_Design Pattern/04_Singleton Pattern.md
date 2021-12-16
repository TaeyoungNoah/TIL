# 싱글톤 패턴 (Singleton Pattern)



### **<u>*"클래스의 인스턴스, 즉 객체를 하나만 만들어 사용하는 패턴"*</u>**



싱글톤 패턴이란, 인스턴스를 **하나만** 만들어 사용하기 위한 패턴

싱글톤 패턴은 **인스턴스를 단 하나만 만들고, 그것을 계속하여 재사용한다.**



**조건**

- new 를 실행할 수 없도록 생성자에 private 접근 제어자 지정
- 유일한 단일 객체를 반환할 수 있는 정적(static) 메서드 필요
- 유일한 단일 객체를 참조할 정적(static) 참조 변수 필요



## 예시

*(Singleton)*

```java
package singleton;

public class Singleton {
    static Singleton singletonObject; // 정적 참조변수 (조건 3)

    private Singleton() {} // private 생성 (조건 1)

    // 객체 반환 정적 메서드 (조건 2)
    public static Singleton getInstance() {
        if(singletonObject == null){
            singletonObject=new Singleton();
        }
        return singletonObject;
    }
}
```

*(Client)*

```java
package singleton;

public class Client {
    public static void main(String[] args) {
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();
        Singleton s3 = Singleton.getInstance();

        System.out.println("s1 = " + s1);
        System.out.println("s2 = " + s2);
        System.out.println("s3 = " + s3);
    }
}
```

> 출력 결과
>
> s1 = singleton.Singleton@7cef4e59
> s2 = singleton.Singleton@7cef4e59
> s3 = singleton.Singleton@7cef4e59
>
> (모두 하나의 객체를 참조하고 있음을 알 수 있음!)



싱글톤 패턴에는 다양한 종류가 있다.

https://yaboong.github.io/design-pattern/2018/09/28/thread-safe-singleton-patterns/

위 블로그에 자세하게 설명이 되어있다. 아직 모든 내용이 완벽하게 이해가 가지는 않지만 조금 더 성장한 후 다시 한번 내용을 정독해보자.