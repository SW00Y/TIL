# 2025-03-28~

### [ Spring 숙련 1주차 강의]

1. 객체 지향 설계와 SOLID 원칙
2. Spring의 핵심 개념
3. 싱글톤 패턴
4. Bean 등록
5. Validation

<hr>

<details>

<summary style="font-size: 16px;">
<strong>1. 객체 지향 설계와 SOLID 원칙</strong>
</summary>

1. SRP (단일 책임 원칙)
- 하나의 클래스는 하나의 책임 가진다.
  - (User 클래스가 사용자 정보 관리, 로그인, DB 저장을 모두 담당하면 SRP 위반 → AuthService, UserRepository로 분리)

2. OCP (개방-폐쇄 원칙)
- 확장에는 열려 있고, 수정에는 닫혀 있어야 한다.
  - (Shape 인터페이스를 구현한 Circle, Square 클래스를 추가할 때 AreaCalculator는 수정하지 않아도 됨)

3. LSP (리스코프 치환 원칙)
- 자식 클래스는 부모 클래스를 대체할 수 있어야 한다.
  - (Car의 accelerate()를 ElectricCar에서 오버라이드해 예외를 던지면 LSP 위반 → 인터페이스(Acceleratable)로 분리)

4. ISP (인터페이스 분리 원칙)
- 클라이언트는 사용하지 않는 메서드에 의존하지 않아야 한다.
  - (Animal 인터페이스에 fly(), run(), swim()이 모두 있으면 Dog는 fly()를 사용하지 않음 → Runnable, Swimmable로 분리)

5. DIP (의존관계 역전 원칙)
- 구체적인 클래스가 아닌 인터페이스에 의존해야 합니다.
  - (NotificationService가 EmailNotifier에 직접 의존하면 DIP 위반 → Notifier 인터페이스에 의존하도록 변경)

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>2. Spring의 핵심 개념</strong>
</summary>

1. Spring Container
- Bean의 생성, 의존성 주입, 생명주기 관리.
- 종류
  - BeanFactory : 기본적인 Bean 관리.
  - ApplicationContext : 국제화, 이벤트 처리 등 추가 기능 제공.

2. Spring Bean
- Spring이 관리하는 객체
- @Component, @Service 등으로 등록
- 특징
  - 기본적으로 싱글톤으로 관리됨.
  - 의존성 주입(DI)을 통해 다른 Bean과 연결됨.

3. IOC (제어의 역전)
- 객체 생성과 관리를 개발자가 아닌 Spring이 담당.
- 예시: new MyService() 대신 @Autowired로 주입.

4. DI (의존성 주입)
- 생성자 주입 (권장) : 불변성 보장, 테스트 용이.
  ```java
  @Service
  public class OrderService {
      private final PaymentService paymentService;
      
      @Autowired // 생성자가 하나면 생략 가능
      public OrderService(PaymentService paymentService) {
          this.paymentService = paymentService;
      }
  }
  ```
- Setter 주입 : 선택적 의존성에 사용.
- 필드 주입 : 비권장 (테스트 어려움).

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>3. 싱글톤 패턴</strong>
</summary>

1. 싱글톤 패턴: 하나의 인스턴스만 생성해 메모리 효율성 향상

    ```java
    public class Singleton {
        private static Singleton instance;
        
        private Singleton() {}
        
        public static Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
    }
    ```
- 주의사항
  - 무상태(stateless) 설계 필수 : 공유 필드(private int value) 사용 시 동시성 문제 발생
  - Spring의 싱글톤 컨테이너 : 직접 구현하지 않아도 Spring이 Bean을 싱글톤으로 관리

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>4. Bean 등록</strong>
</summary>

1. 등록 방법
- 자동 등록 : @ComponentScan + @Component, @Service, @Repository
- 수동 등록 : @Configuration + @Bean
  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public MyService myService() {
          return new MyServiceImpl();
      }
  }
  ```

2. 충돌 해결
- @Qualifier : 특정 Bean을 명시적으로 선택.
  ```java
  @Autowired
  @Qualifier("firstService")
  private MyService myService;
  ```
- @Primary: 기본 Bean으로 지정.

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>5. Validation</strong>
</summary>

1. 검증 계층

- 프론트 : 사용자 경험 향상 (예: JavaScript 유효성 검사)
- 서버 : 필수 검증 (예: @NotNull, @Size).
- DB : NOT NULL, UNIQUE 등.

2. Bean Validation

```java
public class MemberRequest {
    @NotNull
    private String name;
    
    @Min(0)
    private int age;
}
```

BindingResult: 검증 오류 처리

```java
@PostMapping("/member")
public String createMember(@Valid MemberRequest request, BindingResult result) {
    if (result.hasErrors()) {
        return "error-page";
    }
    return "success";
}
```

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>핵심요약</strong>
</summary>

1. Spring Container
    - Bean 관리, 의존성 주입
2. Spring Bean
    - Spring Container가 관리하는 Java 객체
3. IOC/DI
    - 제어의 역전(IOC)
        - 객체 생성과 의존성 관리를 개발자가 아닌 Spring이 담당
    - 의존성 주입(DI)
        - 객체 간의 의존성을 Spring Container가 주입
4. 싱글톤 패턴
    - Spring Bean은 기본적으로 싱글톤 패턴으로 관리되어 하나의 인스턴스만 생성
        - 상태를 가져서는 안된다.
5. Spring Bean 등록
    - 자동 등록(권장)
        - @ComponentScan(Spring Boot) + @Component(@Controller, @Service, @Repository)
    - 수동 등록
        - @Configuration + @Bean
6. 의존관계 주입
    - 생성자 주입
        - 생성자 + @Autowired를 통해 의존성을 주입받는다. 불변, 실수 방지
    - @RequiredArgsConstructor
        - final 필드에 자동으로 생성자를 생성해 주입한다.
7. Validation
    - BindingResult
        - Validation 오류 정보를 담는 객체
    - Bean Validation
        - Annotation 기반으로 Validation이 가능하도록 만들어진 표준 기술

</details>