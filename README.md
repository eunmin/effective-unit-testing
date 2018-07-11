# 테스트 냄새

## 가독성

### 기본 타입 단언 (Primitive Assertion)

```java
@Test
public void outputHahLineNumbers() {
  String content = "1st match on #1\nand\n2nd match on #3";
  String out = grep.grep("match", "test.txt", content);
  assertTrue(out.indexOf("test.txt:1 1st match") != -1);
  assertTrue(out.indexOf("test.txt:3 2st match") != -1);
}
```

```java
@Test
public void outputHahLineNumbers() {
  String content = "1st match on #1\nand\n2nd match on #3";
  String out = grep.grep("match", "test.txt", content);
  assertThat(out.contains("test.txt:1 1st match"), equals(true));
  assertThat(out.contains("test.txt:3 2st match"), equals(true));
}
```

- `!=`, `==`를 발견하면 냄새를 의심
- 비교 대상이 `0`, `-1`이면 냄새를 의심
- 기본 타입 강박관념 냄새와 같은 것

### 광역 단언 (Hyperassertion)

- 코드 4-6

```java
assertEquals(expectedOutput, output.content());
```

- 여러 테스트를 한방에 테스트하지말고 테스트 하나(assertion)에 하나씩 테스트 하자.

### 비트 단언 (Bitwise Assertion)

- 비트 연산을 하는 테스트는 쉬운 테스트로 바꿔라.

### 부차적 상세정보 (Incidental Details)

- 코드 4-10

- 추상화 수준이 다른 코드는 추상화 수준을 맞춘다. (Extract Method)

### 다중 인격 (Split Personality)

- 코드 4-13
- 하나의 테스트에서 두번의 테스트를 위해 변수를 재사용한다.
- 공통 로직을 상위 클래스로 옮기고 상속을 받아 각각의 테스트를 구현하는 방법을 생각해본다.
- 메서드 수준으로 나누는 것도 고려한다.
- 너무 나누다 보면 쪼개진 논리 (Split logic) 냄새로 바뀔 수 있다.

### 쪼개진 논리 (Split Logic)

- 코드 4-15
- 파일을 다른 곳으로 나눴는데 그 파일을 봐야 이해할 수 있는 심볼로 구성되어 있어 이해하기 어렵다.
- 하나로 합치고 다른 냄새를 고친다.
- 생각해볼 문제? 얼마나 쪼개고 얼마나 합쳐야하나?

### 매직 넘버 (Magic Number)

```java
roll(10, 12);
```

```java
roll(pins(10), times(12));
```

- 생각해볼 문제? objective-c, swift / 함수명에 파라미터명?

### 셋업 설교

- 코드 4-21
- 긴 셋업 코드는 셋업 설교 냄새를 의심해봐야 한다. 부차적 상세정보의 셋업 버전
- 각 테스트에서 초기화 할 수 있으면 거기서 한다.
- 셋업 코드의 추상화 단계를 맞춘다.

### 과잉보호 테스트

```java
@Test
public void count() {
  Data data = project.getData();
  assertNotNull(data); // 불필요
  assertEquals(4, data.count())
}
```

- 테스트가 잘 돌아가게 하기 위한 Assertions
- 널 체크, 배열 길이 체크 등등
- 필요 없으면 지워라
