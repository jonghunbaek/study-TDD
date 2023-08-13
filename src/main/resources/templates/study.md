# TDD와 테스트 코드
### 1. 단위 테스트
+ 자동화 테스트
+ 작은 코드 단위를 독립적으로 검증하는 테스트 - 클래스, 메서드
+ 빠른 검증속도, 안정적
+ JUnit5, AssertJ를 사용해서 검증 - assertj는 메서드 체이닝, 풍부한 api지원

### 2. 테스트 케이스 세분화
+ 요구사항에 대해 암묵적이거나 드러나지 않는 요구사항이 있는지 확인 - 해피 케이스, 예외 케이스에 대한 고민
+ 해당 케이스들을 도출할 때는 경계값 테스트에 유의해야 한다.
+ 테스트는 항상 통과해야 한다. -> 랜덤 값 또는 시간에 따른 변경이 되는 부분은 외부에서 주입해주는 형태로. 또 외부로 분리할 수록 테스트 가능한 영역이 많아진다.
+ 즉, 테스트를 해야하는 영역을 확실하게 구분해줘야 한다.
+ 테스트하기 어려운 영역 - 관측마다 다른 값에 의존하는 코드(현재 날짜/시간, 랜덤 값, 전역 변수/함수, 사용자 입력 등), 외부세계에 영향을 주는 코드(표준출력, 메시지 발송, DB연동)
+ 순수 함수 단위로 테스트 케이스를 작성 - 같은 입력엔 같은 결과, 외부와 단절, 테스트하기 쉽게

### 3. TDD
+ 프로덕션 코드보다 테스트 코드를 먼저 작성해 테스트가 구현 과정을 주도하도록 하는 방법론
+ 레드, 그린, 리팩토링이라는 사이클을 거쳐서 구현 
+ 레드 - 프로덕션코드가 없기 때문에 무조건 실패하는 단계
+ 그린 - 구현이 엉망이라도 테스트를 통과할 수 있게 구현
+ 리팩토링 - 프로덕션 코드를 리팩토링
+ TDD의 핵심 가치는 피드백 - 테스트 통과가 보장 되기 때문에 프로덕션 코드를 자주, 빠르게 리팩토링해 볼 수 있다.
+ 선기능 구현, 후 테스트 단점 - 테스트 작성 누락 위험, 특정 테스트만 검증할 수 있음(해피O, 예외X), 잘못된 구현에 대한 발견 가능성이 낮음
+ TDD장점 - 테스트를 작성하기 위한 구조를 처음부터 고민할 수 있음, 복잡도가 낮은 테스트 가능한 코드로 구현할 수 있게 만들어줌, 엣지 케이스(예외)를 놓치지 않게 해줌, 구현에 대한 빠른 피드백이 가능
+ TDD는 클라이언트 관점에서 피드백을 주는 방법론이라고 볼 수 있다. 또 TDD가 항상 옳은 방법론은 아니다라는 것을 유념하자.

### 4. 테스트는 문서?
+ 프로덕션 코드를 설명해주는 문서, 프로덕션 코드를 이해하는 시각, 관점을 다양화, 팀 차원에서 봤을 때 팀의 자산이 될 수 있다.
+ @Displayname을 활용해 어떤 테스트인지를 명시하자. 자세할수록 좋다. 완전한 문장형태로 ex)음료 1개를 추가하면 주문 목록에 담긴다. 테스트 행위에 대한 결과까지 적는게 바람직하다. 항상 구체적으로
+ 도메인 용어를 사용해 한층 추상화된 내용을 담자. 실패 -> 주문을 생성할 수 없다. 테스트 현상을 중점으로 기술x
+ BDD(Behavior Driven Develpment) - TDD에서 파생된 기법, 함수단위 테스트보단 시나리오 기반 테스트케이스 자체에 집중, 비개발자가 이해할 정도의 추상화 수준, given-when-then을 활용

### Spring & JPA 테스트
+ 통합 테스트 - 여러 모듈이 협력하는 기능을 테스트, 작은 범위의 테스트만으론 기능 전체의 신뢰성 보장X, 풍부한 단위 테스트 + 큰 기능단위의 통합테스트
+ 레파지토리 테스트는 계층관점에서 봤을 때 단위 테스트라고 볼 수 있다.
+ @DataJpaTest(@Transactional 지원) - jpa관련 테스트만 할 수 있게 지원해주기 때문에 @SpringBootTest(@Transactional 미지원)보다 가볍다.
+ 비즈니스 레이어 - Persistence Layer와의 상호작용을 통해 비즈니스 로직을 전개, 트랜잭션 보장
+ Presentation Layer에선 하위 비즈니스, 펄시스턴스 레이어를 mocking처리해 테스트

### MockMvc
+ 테스트할 때 의존관계의 존재들을 대체하기 위한 방법
+ Mock(가짜 객체)를 사용해 스프링 MVC동작을 재현할 수 있는 테스트 프레임워크
+ @WebMvcTest - 컨트롤러에 있는 빈만 컨테이너에 올려 테스트를 경량화 시킴
+ @MockBean
+ MockBean을 통해 Mock에 대한 행위를 정의한 것을 stubbing이라고 한다.

### Test Double
+ Test Double - Dummy(깡통 객체), Fake(단순형태, 동일기능, 프로덕션에 쓰긴 어려운 객체 - ex repository), Stub(테스트 요청에 미리 준비한 것을 제공하는 객체)
+ Spy(stub이나 호출내용 기록, 일부는 실제처럼 일부는 Stubbing할 수 있는 객체), Mock(행위에 대한 기대를 명세하고 그에 따라 동작하도록 만든 객체)
+ Stub vs Mock - 상태 검증 vs 행위 검증
+ @Mock vs @Spy - 전체가 Mock이냐 일부는 실제 객체르 사용하냐의 차이
+ @InjectMock을 통해 의존성 주입처럼 Mock을 활용할 수 있다.
```java
@Mock
private MailSendClient mailSendClient;

@Mock
private MailSendHistoryRepository mailSendHistoryRepository;

@InjectMocks
private MailService mailService;
```
+ BDDMockito - Mockito의 부자연스러운 메소드명을 자연스럽게 바꿔준다. 이외 기능은 동일

### Classicist vs Mockist
+ 실제 객체 사용 vs 목 객체 사용
+ Classicist - 꼭 필요한 경우에만 Mock객체를 사용하자.
+ Mockist - Mock객체로만 테스트 코드를 작성하자.

### 좋은 테스트 작성하기
+ 코딩은 글쓰기와 같다. 한 문단에 한 주제처럼 테스트 하나엔 하나의 주제만 테스트해야 한다.
+ 분기문, 반복문 같은 논리구조가 들어갔다는 것은 두 가지 이상의 주제를 내포한다는 방증이다.
+ 테스트 작성 시 모든 환경을 완벽하게 제어할 수 있는지 고민을 끊임없이 해야 한다.
+ 테스트 환경의 독립성을 보장하자. - given절에서 테스트가 깨지면 안된다가 적절한 예시 
+ 테스트간의 독립성을 보장하자 - 공유자원 사용금지 또는 @AfterEach등을 활용해 테스트 끝날 때마다 clean작업을 진행
+ **Test Fixture** 
```
0. 테스트를 위해 원하는 상태로 고정시킨 일련의 객체 - given절에 생성한 객체들이 대표적인 예
1. @BeforeAll, @BeforeEach의 단점 - 테스트와의 결합도가 높아져 수정 시 모든 테스트에 영향을 주게 된다. 또 문서로써의 역할을 하기 어려움(given절을 생략하기에)
1-1. 단, 각 테스트에서 전혀 몰라도 이해하는데 문제가 없고, 수정해도 모든 테스트에 영향을 주지 않으면 사용해도 괜찮다. 

2. 객체를 생성할 때는 테스트 클래스마다 생성 메서드를 만들어 필요한 파라미터만 주입해주는 방법이 좋다. 

3. Fixture를 만들기 위해 빌더를 남용하는 것은 코드의 복잡도가 올라가니 지양하자.
```
+ **Test Fixture Cleansing**
```
deleteAll vs deleteAllInBatch
1. deleteAllInBatch - 테이블의 전체 데이터를 한번에 삭제 한다.
2. deleteAll - select -> delete(where절을 사용해 하나씩)을 통해 연관된 테이블의 데이터도 삭제해준다.
3. 성능 이슈가 있음 - 2번의 경우 연관관계로 매핑된 테이블까지 같이 지우고, 하나씩 삭제하기 때문에 다수의 쿼리 전달로 비용이 많이 든다.

@Transactional
1. 사이드 이펙트만 고려하면 @Transactional을 사용하는 것이 편하다.
```
+ **@ParameterizedTest** - 값만 변경해서 다양한 테스트를 하고 싶을 때 사용(@CsvSource와 같은 소스 어노테이션을 활용)
```java
@DisplayName("상품 타입이 재고 관련 타입인지를 체크한다.")
@CsvSource({"HANDMADE,false","BOTTLE,true","BAKERY,true"})
@ParameterizedTest
void containsStockType4(ProductType productType, boolean expected) {
    // when
    boolean result = ProductType.containsStockType(productType);

    // then
    assertThat(result).isEqualTo(expected);
    }
```
```java
private static Stream<Arguments> provideProductTypesForCheckingStockType() {
    return Stream.of(
        Arguments.of(ProductType.HANDMADE, false),
        Arguments.of(ProductType.BOTTLE, true),
        Arguments.of(ProductType.BAKERY, true)
    );
}

@DisplayName("상품 타입이 재고 관련 타입인지를 체크한다.")
@MethodSource("provideProductTypesForCheckingStockType")
@ParameterizedTest
void containsStockType5(ProductType productType, boolean expected) {
    // when
    boolean result = ProductType.containsStockType(productType);

    // then
    assertThat(result).isEqualTo(expected);
}
```
+ [Junit5 Parmeterized Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests)
+ **@DynamicTest** - 테스트에 시나리오를 짜려고 할 때. 예를 들어 순차적으로 테스트를 하며 검증해야 할 경우
```java
@DisplayName("재고 차감 시나리오")
@TestFactory
Collection<DynamicTest> stockDeductionDynamicTest() {
    // given
    Stock stock = Stock.create("001", 1);

    return List.of(
        DynamicTest.dynamicTest("재고를 주어진 개수만큼 차감할 수 있다.", () -> {
            // given
            int quantity = 1;

            // when
            stock.deductQuantity(quantity);

            // then
            assertThat(stock.getQuantity()).isZero();
        }),
        DynamicTest.dynamicTest("재고보다 많은 수의 수량으로 차감 시도하는 경우 예외가 발생한다.", () -> {
            // given
            int quantity = 1;

            // when // then
            assertThatThrownBy(() -> stock.deductQuantity(quantity))
                .isInstanceOf(IllegalArgumentException.class)
                .hasMessage("차감할 재고 수량이 없습니다.");
        })
    );
}
```
+ 테스트 수행 비용 고려 - 테스트 환경을 공통으로 비슷하게 만들어 시간 단축. 예를 들어 추상 클래스를 만들어 상속받게 하는 방법
+ 이 때, Mock객체를 사용하는 클래스는 서버를 새로 띄워야 하기 때문에 1. MockBean을 상위클래스로 이전 // 2. 테스트환경을 여러개로 나누는 방법들이 있다.
+ Controller는 테스트 환경을 따로 만들어 주는게 유리 - Service-Repository와 보통 분리해서 환경을 구축
+ private에 대한 테스트 - 테스트를 작성할 필요 없다. 하지만 꼭 필요하다면 객체의 책임을 분리해야 할 시점인지 고민해 보자.
+ 프로덕트엔 필요없지만 테스트에만 필요한 메서드는 작성해도 되는가? - 남용x 충분한 고민을 거쳐 다양하게 사용될 수 있는 상황에 적용하자. 보수적인 관점으로
+ **정리**
```
1. 테스트 1개에 목적은 1개만
2. given절에서 생성하는 객체는 완벽하게 제어(ex - 시간, 랜덤 값 등 상황에 따라 변하는 것들은 지양)
3. 테스트 <-> 테스트 독립성 보장, 테스트 환경의 독립성을 보장
4. Test Fixture(공유변수 사용지양 - @BeforeEach,@BeforeAll, 객체 생성 시 필요 파라미터만 주입, 빌더패턴 남용 지양, given절 내용을 명확하게)
5. @Parameterized, @DynamicTest - 동적인 테스트를 하고 싶을 때 활용하자.
6. deleteAll vs deleteAllInBatch 장단점
7. 테스트 수행 비용을 고려해 환경을 통합 - 추상 클래스를 만들어 상속받도록 유도. 즉, 스프링부트가 실행되는 횟수를 감소시키는게 목적
8. private 메소드 테스트를 해야할지에 대한 고민
9. 테스트 작성에만 필요한 프로덕션 코드가 있을 수 있다. 하지만 충분하게 고려 후 사용하자.
```

### Tip
+ lombok 사용가이드 - @Data, @Setter, @AllArgsConstructor 사용 지양, 양방향 연관관계시 @ToString 순환참조 문제
+ 테스트 작성시  builder패턴을 이용해 생성시 코드가 너무 길어지면 메소드로 추출해 기본값을 넣어주고 필요한 값들만 매개 변수로 받아준다.
```java
private Product createProduct(ProductType type, String productNumber, int price) {
        return Product.builder()
                .type(type)
                .productNumber(productNumber)
                .price(price)
                .sellingStatus(SELLING)
                .name("메뉴 이름")
                .build();
     }
```
+ isEqualByComparingTo() enum값 비교시에 유용하다.
+ repository.deleteAll() vs repository.deleteAllInBatch()
+ 서비스에서 유효성 체크, 도메인에서 유효성 체크는 완전히 다른 상황이다. 다시말해 중복이 아니다.
+ @Transactional에 대한 깊은 이해를 바탕으로 사용해야 한다.
+ 속성 중 readOnly = true : 
+ CRUD중 CUD 동작 X, JPA: CUD스냅샷 저장, 변경 감지X -> 성능향상
+ CQRS(Command/Query분리) : 
+ 서비스에서 로직이 많지 않을 땐 Repository 테스트와 크게 차이가 없다. 하지만 비슷하더라도 작성해 주는 것이 추후 기능확장을 할 때 유리하다.
+ 직렬화, 역직렬화를 도와준느 ObjectMapper는 해당 작업을 할 때 객체의 기본 생성자를 필요로 한다.
+ 컨트롤러는 파라미터의 유효성 체크가 주 역할 이다.
+ @NotNull - "", " " 가능
+ @NotEmpty - " " 가능, ""불가능
+ @NotBlank - 공백, 빈문자열 전부 안돼
+ 유효성 체크 범위와 책임에 대한 고민이 필요 - Controller단에선 좀 더 포괄적인(@NotBlank같은)검증을 Service에선 좀 더 구체적인(글자수 20자)검증을 하는 식으로
+ 매우중요!!!!!!!!!! -> 하위레이어가 상위레이어를 알고있는것은 좋지 않다. Controller에서 사용하는 dto를 서비스에서까지 사용하게 되면 의존관계가 생기므로 dto를 별도로 만들어주는 것이 좋다.
+ 이로 인해 얻어지는 효과는 이후 컨틀로러 - 서비스간 모듈 분리시에 유리하고, @BeanValidation의 책임을 컨트롤러에 한정시킬 수 있다.
+ + Layered Architecture의 단점 - 도메인(엔티티)객체와 db와 강결합
+ 이를 위해 Hexagonal Architecture가 나오게 된다. - 시스템이 커질 경우
+ 단위 테스트 vs 통합 테스트
+ @SpringBootTest vs @DataJpaTest vs @WebMvcTest
+ Optimistic Lock, Pessimistic Lock
+ mail 전송같은 테스트에선 @Transactional을 안붙이는 것이 좋다.