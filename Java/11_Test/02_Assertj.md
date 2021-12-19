# org.assertj.core.api.Assertions



### Assertions.assertThat( `actual` ).isEqualTo( `expected` );

- 비교하여 같은지 확인 

```java
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class SimpleTest {
    @Test
    void test() {
        int a=2;
        int b=2;

        Assertions.assertThat(a).isEqualTo(b);
    }
}
```



### Assertions.assertThat( `비교대상 자료구조` ).contains( `원소1, 원소2 ...` );

- `비교대상 자료구조` 와 에 `원소` 가 존재하는지 체크 (중복 여부, 순서에 상관 X)  

```java
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class SimpleTest {
    @Test
    void test() {
        int[] intArr = {1,2,3,4};

        Assertions.assertThat(intArr).contains(1);
        Assertions.assertThat(intArr).contains(1,2);
        Assertions.assertThat(intArr).contains(1,2,3);
        Assertions.assertThat(intArr).contains(1,3,2,4);
    }
}
```



### Assertions.assertThatThrownBy()

- 주로 예외처리에 관한 테스트를 진행할 때 좋다. 
- ()에 예외상황을 일으킬만한 람다식을 적어주면 된다.

```java
package study;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class SimpleTest {
    @Test
    void test() {
        // given
        String test = "abc";
        // when & then
        Assertions.assertThatThrownBy(() -> {
            test.charAt(3);
        }).isInstanceOf(IndexOutOfBoundsException.class)
                .hasMessageContaining("index out of range: 3");
    }
}
```

