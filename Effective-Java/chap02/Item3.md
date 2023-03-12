# 생성자에 매개변수가 많다면 빌더를 고려해라.
매개변수가 많아질 경우 정적 팩토리 메서드와 생성자는 사용하기 불편해진다. 이러한 불편함을 해결할 수 있는 세 가지 대안을 살펴보자.

### 1. 생성자를 이용할 경우
```java
Nutritionfact cocaCola = new Nutritionfact(240, 8, 100, 0, 35);
```
이렇게 생성자를 만들 수 있지만 어떤 속성 값을 설정했는지 알기 힘들다. **매개변수의 수가 늘어날 수록 코드를 작성하거나 읽기 힘들어진다.** <br>
문제는 매개변수 타입이 같은 상황에서 실수로 순서를 바꿔 입력할 경우 컴파일 시점에서 알 수 없고 **런타임에 엉뚱하게 동작하게 된다.**

### 2. 자바빈
또 다른 대안으로는 매개변수가 없는 생성자를 만든 뒤 setter를 통해서 값을 설정한는 방법이 있다.
````java
Nutritionfact cocaCola = new Nutritionfact();
cocaCola.setServingSize(240);
cocaCola.setServings(8);
cocaCola.setCalories(100);
..
반복
````
생성자를 이용한 방식보다 가독성이 훨씬 올라갔다. 하지만 객체가 완전히 생성되기 전까지는 중간에 사용할 수도 있으므로 일관성을 유지할 수 없다.<br>
또한 불변 클래스로 만들 수 없다는 단점도 있으며(쓰레드 세이프하지 않다) 이를 해결하려면 추가 작업을 해줘야 한다.

### 3. 빌더
빌더 패턴은 필요한 매개변수만 전달할 수 있고 자바빈즈 패턴의 가독성을 모두 겸비한 대안이다.<br>
필수 매개변수만으로 생성자(혹은 정적팩토리)를 호출해 빌더 객체를 얻는다. 그 후 빌더 객체가 제공하는 일종의 세터 메서드들로 원하는 선택 매개변수를 설정한다. 마지막으로 매개변수가 없는build메서드를 호출하여 객체를 생성한다.

```java
// 예제에서 사용한 Builder패턴

public class NutritionFacts {

  private final int servingSize;
  private final int servings;
  private final int calories;
  private final int fat;
  private final int sodium;

  public static class Builder {

    // 필수 매개변수
    private final int servingSize;
    private final int servings;

    // 선택 매개변수 - 기본값으로 초기화 한다.
    private int calories = 0;
    private int fat = 0;
    private int sodium = 0;

    public Builder(int servingSize, int servings) {
      this.servingSize = servingSize;
      this.servings = servings;
    }

    public Builder calories(int val) {
      calories = val;
      return this;
    }

    public Builder fat(int val) {
      fat = val;
      return this;
    }

    public Builder carbohydrate(int val) {
      sodium = val;
      return this;
    }

    public NutritionFacts build() {
      return new NutritionFacts(this);
    }
  }

    private NutritionFacts(Builder builder) {
      this.servingSize = builder.servingSize;
      this.servings = builder.servings;
      this.calories = builder.calories;
      this.fat = builder.fat;
      this.sodium = builder.sodium;
  }

}
```

````java
NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
.calories(100)
.sodium(35)
.carbohydrate(27)
.build();
````

아까보다 가독성도 좋아지고 NutritionFacts 클래스는 불변이 되었다. 추상 클래스는 추상 빌더를 갖게하고 구체 클래스는 구체 빌더를 갖게 한다.