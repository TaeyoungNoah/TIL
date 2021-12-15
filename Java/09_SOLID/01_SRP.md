# SRP - 단일 책임 원칙

> **S**OLID
>
> *"어떤 클래스를 변경해야 하는 이유는 오직 하나뿐이어야 한다" -로버트  C.마틴*



하나의 클래스에 너무 많은 역할(책임)이 부여되면 객체지향의 세계에서는 나쁜냄새가 난다고 한다.

이를 **역할(책임)에 맞게 분리하는 것이 단일 책임 원칙이다**



```java
class Dog{
  final static Boolean male = true;
  final static Boolean female = false;
  Boolean sex;
  
  void pee(){
    if(this.sex==male){
      // 한쪽 다리를 들고 소변을 본다
    }else{
      // 뒷다리 두 개를 굽혀 앉은 자세로 소변을 본다.
    }
  }
}
```



위 코드에서 보면 **Dog** 라는 클래스에서 **male** 과 **female** 의 행위를 모두 구현하려고 함 → **<u>단일 책임 원칙을 위배!</u>**

이 코드를 단일 책임 원칙을 지켜 수정하면 아래와 같이 표현할 수 있다.



```java
abstract class Dog{
  abstract void pee();
}

class MaleDog{
  void pee{
    // 한쪽 다리를 들고 소변을 본다
  }
}

class FemaleDog{
  void pee{
    // 뒷다리 두 개를 굽혀 앉은 자세로 소변을 본다.
  }
}
```



***애플리케이션의 경계를 정하고 추상화를 통해 클래스들을 선별하고 속성과 메서드를 설계할 때,***

***반드시 단일 책임 원칙을 고려하는 습관을 들이자!***