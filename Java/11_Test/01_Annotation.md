# Test코드 Annotation 정리

### @Test

- Test 메소드 임을 선언

```java
    @Test
    void simpleTest() {
        // given
        String test = "1,2";
        // when
        String[] testArr = test.split(",");
        // then
        assertThat(testArr).contains("1","2");
    }
```



### @BeforeEach

- 각각의 테스트 실행 전에 `@BeforeEach` 어노테이션으로 정의된 메서드를 실행

```java
package study;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class SimpleTest {
    @BeforeEach
    void beforeEach() {
        System.out.println("before");
    }

    @Test
    void test1() {
        System.out.println(1);
    }

    @Test
    void test2() {
        System.out.println(2);
    }
}
```

> 테스트 클래스 전체 실행시, 출력 결과:
>
> before
>
> 1
>
> before
>
> 2

> 주의: **테스트가 위에서부터 아래로 순서대로 실행되리라는 보장은 없음! 위는 단순한 예시**



### @AfterEach

- 각각의 테스트 실행 후에 `@AfterEach` 어노테이션으로 정의된 메서드를 실행

```java
package study;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class SimpleTest {
    @AfterEach
    void afterEach() {
        System.out.println("after");
    }

    @Test
    void test1() {
        System.out.println(1);
    }

    @Test
    void test2() {
        System.out.println(2);
    }
}
```

> 테스트 클래스 전체 실행시, 출력 결과:
>
> 1
>
> after
>
> 2
>
> after



### @DisplayName

- 테스트클래스 또는 테스트 메서드에 사용자 정의 이름을 선언

```java
package study;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class simpleTest {
    @AfterEach
  	@DisplayName("@AfterEach 테스트")
    void afterEach() {
        System.out.println("after");
    }

    @Test
    @DisplayName("테스트케이스 1")
    void test1() {
        System.out.println(1);
    }

    @Test
    @DisplayName("테스트케이스 2")
    void test2() {
        System.out.println(2);
    }
}
```

> 테스트 실행시 테스트 이름이 `@DisplayName` 에 적은 부분이 나오게 됨



### @ParameterizedTest

- 매개변수를 받는 테스트를 작성할 수 있다.

#### @ValueSource

- 해당 Annotation 에 지정한 배열을 파라미터 값으로 순서대로 넘겨준다.

- test method 실행 당 하나의 인수(argument) 만을 전달할 때 사용할 수 있다.

- 리터럴 값의 배열을 테스트 메서드에 전달한다.

- c.f. literal values 종류

  > short, byte, int, long, float, double, char, java.lang.String, java.lang.Class

```java
package study;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import java.util.HashSet;
import java.util.Set;

import static org.assertj.core.api.Assertions.assertThat;

public class SimpleTest {
    Set<Integer> integers;
    @BeforeEach
    void setUp() {
        integers = new HashSet<>();
        integers.add(1);
        integers.add(2);
        integers.add(3);
    }

    @ParameterizedTest
    @ValueSource(ints = {1,2,3})
    void test(int testNumber){
        assertThat(integers.contains(testNumber)).isTrue();
    }
}

```

> 1, 2, 3 을 각각 넣었을 때 테스트 결과를 알 수 있음 (저 메서드 하나에 3번의 테스트를 진행함)
>
> 주의: **저 3개의 파라미터를 넣은 결과 모두가 테스트를 통과할 수 있어야함**



#### @CsvSource

- 위에서 살펴본 `@ValueSource` 에서 주의사항인 *'<u>3개의 파라미터를 넣은 결과 모두가 테스트를 통과할 수 있어야함</u>'* 의 단점(?)을 보완할 수 있음
- **입력값에 따라 결과값이 다를 때 사용**
- `delimiter` 을 통해 구분자를 직접 정의할 수 있음

```java
package study;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;


import java.util.HashSet;
import java.util.Set;

import static org.assertj.core.api.Assertions.assertThat;

public class adsff {
    Set<Integer> integers;
    @BeforeEach
    void setUp() {
        integers = new HashSet<>();
        integers.add(1);
        integers.add(2);
        integers.add(3);
    }

    @ParameterizedTest
    @CsvSource(value = {"1:true","2:true","3:true","4:false","5:false"},delimiter = ':')
    void test(int testNumber, boolean expected){
        assertThat(integers.contains(testNumber)).isEqualTo(expected);
    }
}

```

