# 반성🔥
컨벤션 관련 피드백만 2개 받았다.

# 컨벤션1 - 클래스 내의 구성요소 순서
> 아래와 같은 순서로 클래스를 구성하길 바랍니다.
```java
class  T {
  상수(static final) 또는 클래스 변수  
  인스턴스 변수  
  생성자  
  팩토리 메소드  
  메소드  
  기본 메소드 (equals, hashCode, toString)  
}
```

# 멤버 변수와 메서드 인자
### 피드백
> 랜덤 생성 객체를 멤버 변수로 관리하기 보다 randomGenerator 객체를 활용하는 메서드에 인자로 받으면 어떨까요?

# 인스턴스 재사용
### 피드백
> 랜덤 생성 객체는 한 번만 선언해두고 재사용하면 어떨까요? 
> 스트림에서 매번 `new RandomGenerator(new Random())`하는 부분


# 컨벤션과 역할분담
### 피드백
> -는 출력과 관련된 기능이라고 생각하는데요 
> Score는 자동차 경주 게임의 로직을 담당하는 객체이니 출력을 담당하는 OutputView 클래스로 이동하면 어떨까요? 
> 출력 기호를 변경하는 요구사항이 생긴다면 OutputView 클래스만 변경하면 되니 수정에도 좋을 것 같아요
[Google Conventions](https://google.github.io/styleguide/javaguide.html#s4.8.7-modifiers)

# View와 Model
### 피드백
> 출력 기호를 변경하는 요구사항이라고 코멘트 드려서 제 의도가 정확히 전달이 되지 못했네요 😢
> View와 관련된 변경 요구사항이 있다면 View 영역의 수정만으로 해결하는 것이 좋다는 의견입니다
> 현재 구조에서는 `-`를 변경하기 위해서 Model 영역에 해당하는 MoveStatus 를 찾아서 수정해야하는 과정이 쉽진 않을 것 같아요

# 테스트 코드
### 피드백
> 프로덕션 코드가 의도치 않게 변경되었을 때 테스트는 실패해야하는데요 
> 기능은 의도하지 않는대로 동작하더라도 예외만 발생하지 않는다면 테스트는 통과한다면 테스트를 신뢰하기 어려울 것 같아요 
> Round#race 메서드에서 진행 결과를 반환하고 이를 검증하면 어떨까요?

# 빌더와 조정자
### 피드백
> Round 객체의 책임에 대해 고민해볼 수 있는 부분인 것 같아요
> Round는 자동차들을 1회 전진을 시도하는 역할을 하고 있는데요
> 그렇다면 Round는 1회만 전진을 시도할 수 있다는 제약을 추가할 수 있을 것 같아요
> Round가 race의 결과를 알 수 있다면 2회 이상 시도하는 것을 방지할 수도 있고
> 빌더 메서드를 통해 레이스의 결과나 라운드가 진행되었음을 반환하는 기능을 제공해도 좋을 것 같은데 어떻게 생각하시나요?

# 핵심 로직에 대한 테스트 
> 우승자를 구하는 기능도 핵심 로직이니 단위 테스트를 추가하면 어떨까요?
> 자동차들의 기능이니 일급 컬렉션을 활용해 볼 수 있을 것 같아요

# 생성에 대한 단위 테스트
> static 키워드는 제거하면 어떨까요?
> 자동차를 생성하는 제약이 생겼으니 자동차 생성이 제대로 되는지 단위 테스트를 추가할 수 있겠네요

# SOLID DIP 원칙
> 구체적인 타입보다 추상화된 List 타입으로 선언하면 어떨까요? 
> SOLID DIP 원칙을 참고해 보시면 좋을 것 같아요 

# 디미터 법칙, Tell Dont Ask
> getter로 값을 꺼내어 비교하는 것이 아닌 객체에게 메시지를 보낼 수 있습니다
> 관련해서 디미터 법칙, Tell Dont Ask 를 찾아 보시면 좋을 것 같아요
`
.filter(carStringEntry -> carStringEntry.getValue().equals(maxValue))
.filter(car-> car.samePosition(maxValue))
`
[디미터 법칙](https://dkswnkk.tistory.com/687)
[Tell Dont Ask](https://martinfowler.com/bliki/TellDontAsk.html)

### 디미터 법칙
어떤 객체가 다른 객체에 대해 지나치게 많은 정보를 알고 있으면 서로에 대한 결합도가 높아지고 이로 인해 좋지 못한 설계가 발생한다. 
그래서 이를 개선하고자 다른 객체에게 어떠한 자료(내부 구조)를 가지고 있는지 숨기고 함수를 통해 공개하게 만들었는데 이것이 바로 디미터 법칙입니다.

### Tell Dont Ask 법칙
함수를 실행하라고 말한다. Tell
데이터를 물어보지 않는다. Dont Ask

객체 지향은 데이터와 해당 데이터로 작동하는 기능을 함께 묶은 것이다.

마틴 파울러가 사용한 예시를 적겠습니다. 

`Monitor`가 있고 `value`가 `limit` 보다 크면 `alarm`을 울려야하는 기능입니다.
아래와 같은 코드라면 **ASK** 해야만 합니다.

#### ASK 하게 만드는 코드
```java
class Main{

  public static void main(String[] args) {
    Monitor m = new Monitor("cho", 10, new Alarm());
    if (m.getValue() > m.getLimit()){
        m.getAlarm().warn(m.getName() + " too high");
    }
  }
}


class Monitor{

  private int value;
  private int limit;
  private boolean isTooHigh;
  private String name;
  private Alarm alarm;

  public Monitor (String name, int limit, Alarm alarm) {
    this.name = name;
    this.limit = limit;
    this.alarm = alarm;
  }

  public int getValue() {return value;}
  public void setValue(int arg) {value = arg;}
  public int getLimit() {return limit;}
  public String getName()  {return name;}
  public Alarm getAlarm() {return alarm;}

}
```
#### Tell 하게 만드는 코드
아래처럼 코드를 짜면 `Monitor`에게 **Tell**을 할 수 있습니다. 

```java
class Main{

  public static void main(String[] args) {
    Monitor m = new Monitor("cho", 10, new Alarm());
    m.setValue(11);
  }
}


class Monitor{

  private int value;
  private int limit;
  private boolean isTooHigh;
  private String name;
  private Alarm alarm;

  public Monitor (String name, int limit, Alarm alarm) {
    this.name = name;
    this.limit = limit;
    this.alarm = alarm;
  }
  
  public void setValue(int arg) {
      value = arg;
      if (value > limit)
          alarm.warn(name + " too high");
  }
  
  public int getValue() {return value;}
  public void setValue(int arg) {value = arg;}
  public int getLimit() {return limit;}
  public String getName()  {return name;}
  public Alarm getAlarm() {return alarm;}

}
```

### MVC 패턴에서 mode(도메인)의 view 의존 
도메인 패키지에 존재하는 객체가 view에 의존하고 있네요
요구사항에 맞춰서 도메인 패키지가 view 패키지에 의존하지 않도록 개선해 보시면 좋을 것 같아요


