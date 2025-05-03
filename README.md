# Test Starter

## Phase 1: 단위 테스트 마스터하기 (가장 중요!)

### 테스트의 기본 개념 및 JUnit 이해

> * 테스트 자동화의 필요성, 테스트 피라미드 개념 이해
> * **JUnit 5**: `@Test`, `@BeforeEach`, `@AfterEach`, `@DisplayName`, 기본 Assertions (`assertEquals`, `assertTrue` 등)
    사용법 익히기
> * **AssertJ**: JUnit 기본 Assertion보다 더 풍부하고 읽기 좋은 검증을 제공하는 AssertJ 라이브러리 사용법 익히기 (강력 추천!)
> * **실습**: 순수 자바 클래스(도메인 객체, 유틸리티 클래스 등)에 대한 단위 테스트 작성 연습

### Mock 객체와 Mockito 활용:

> * **Mocking의 필요성**: 단위 테스트에서 의존성을 격리해야 하는 이유 이해
> * **Mockito**: `mock()`, `when().thenReturn()`, `verify()`, `@Mock`, `@InjectMocks` 등 핵심 기능 익히기
> * **실습**: Service 클래스 테스트 시 Repository 인터페이스 Mocking, 외부 API 호출 컴포넌트 Mocking 등 연습

### Spring Boot 환경에서의 단위 테스트

> * Spring 컨텍스트 로딩 없이(`@SpringBootTest` 사용 X) 순수하게 특정 Bean(주로 Service)을 테스트하는 방법 연습
> * Mockito의 `@InjectMocks`와 `@Mock`을 활용하여 의존성 주입 및 Mocking

---

## Phase 2: 통합 테스트 익히기

### Spring Boot 통합 테스트 기초

> * `@SpringBootTest`: 전체 Application Context를 로딩하여 테스트하는 방법 이해
> * `@Autowired`: 테스트 코드에서 실제 Bean을 주입받아 사용하는 방법
> * **실습**: Service와 Repository를 연동하여 DB까지 포함하는 간단한 통합 테스트 작성 (초기에는 H2 같은 내장 DB 활용)

### Web Layer (Controller) 통합 테스트

> * `@WebMvcTest`: Web Layer(Controller) 관련 Bean들만 로딩하여 테스트하는 방법 이해 (Service 등은 Mocking)
> * `MockMvc`: 가짜 HTTP 요청을 보내고 응답을 검증하는 방법 익히기 (Controller 테스트의 핵심!)
> * **실습**: 특정 Controller의 API 엔드포인트가 요청을 잘 받고, 예상된 응답(Status Code, Response Body)을 반환하는지 테스트

### Data Layer (Repository) 통합 테스트

> * `@DataJpaTest`: JPA 관련 설정만 로딩하여 Repository 계층을 테스트하는 방법 이해
> * 내장 DB (H2) 또는 **Testcontainers** (더 현실적인 테스트 환경 제공)를 활용한 DB 테스트 방법 익히기
> * **실습**: Repository의 CRUD 메소드, Query Method 등이 DB와 상호작용하며 올바르게 동작하는지 테스트

---

## Phase 3: 심화 및 실전 적용

### API 테스트 (넓은 의미의 통합 테스트/E2E 테스트)

> * `@SpringBootTest` + `TestRestTemplate`: 실제 서버를 띄우고 HTTP 클라이언트로 API를 호출하여 테스트
> * **RestAssured**: API 테스트를 위한 강력하고 편리한 라이브러리 사용법 익히기 (추천!)
> * **실습**: 여러 API 엔드포인트를 순차적으로 호출하며 시나리오 기반 테스트 작성 연습

### 테스트 주도 개발 (TDD) 맛보기

> * TDD 개념 (Red-Green-Refactor) 이해
> * 작은 기능 단위로 TDD 사이클을 직접 경험해보며 장단점 느껴보기

### 좋은 테스트 코드 작성 원칙

> * FIRST 원칙 (Fast, Independent, Repeatable, Self-Validating, Timely)
> * 테스트 코드의 가독성, 유지보수성 높이는 방법 고민 (Arrange-Act-Assert 패턴 등)
> * 테스트 리팩토링

### (선택) 고급 주제

> * **Contract Testing (MSA 환경)**: Spring Cloud Contract 등
> * **Performance Testing**: JMeter, k6, Gatling 등 도구 학습
> * **Security Testing** 개념

---

## 단위 테스트 모델링 전략 패턴

### 테스트 데이터 빌더 (Test Data Builder) 패턴 (가장 강력하고 추천!)

> * **개념**: 테스트 대상 객체(특히 도메인 엔티티나 VO) 생성을 전담하는 별도의 빌더 클래스를 만드는 패턴입니다. 이 빌더는 테스트에 필요한 속성만 설정하거나 기본값을 제공하고, 유연하게 객체를 생성할 수
    있도록 도와줍니다.
> * **장점**:
    >
* **가독성**: 테스트 코드 본문에서는 `ProductBuilder.aProduct().withName("테스트 상품").build()` 와 같이 어떤 데이터를 만드는지 명확하게 표현할 수 있습니다. 객체 생성
  로직이 캡슐화됩니다.
>     * **유연성**: 특정 테스트에 필요한 속성만 `withXxx()` 메소드로 오버라이딩하여 설정할 수 있습니다. 기본값을 사용하면 코드가 간결해집니다.
>     * **유지보수성**: 도메인 객체의 생성자나 속성이 변경되어도 빌더 클래스만 수정하면 되므로, 테스트 코드의 변경을 최소화할 수 있습니다.
>     * **재사용성**: 여러 테스트 케이스에서 동일한 빌더를 재사용하여 일관된 테스트 데이터를 생성할 수 있습니다.
> * **예시 (Java)**:
    >
    >     ```java
>     // Product.java (도메인 객체)
>     public class Product {
>         private Long id;
>         private String name;
>         private long price;
>         private int stockQuantity;
>         // 생성자, getter 등
> 
>         // Lombok @Builder와 유사하지만, 테스트에 더 특화된 제어가 가능
>         public static ProductBuilder builder() {
>             return new ProductBuilder();
>         }
>     }
> 
>     // ProductBuilder.java (테스트 소스셋에 위치)
>     public class ProductBuilder {
>         private Long id = 1L; // 기본값 설정
>         private String name = "기본 상품";
>         private long price = 10000L;
>         private int stockQuantity = 10;
> 
>         public ProductBuilder id(Long id) {
>             this.id = id;
>             return this;
>         }
> 
>         public ProductBuilder name(String name) {
>             this.name = name;
>             return this;
>         }
> 
>         public ProductBuilder price(long price) {
>             this.price = price;
>             return this;
>         }
> 
>         public ProductBuilder stockQuantity(int stockQuantity) {
>             this.stockQuantity = stockQuantity;
>             return this;
>         }
> 
>         // 특정 상태를 나타내는 메소드 추가 가능
>         public ProductBuilder soldOut() {
>             this.stockQuantity = 0;
>             return this;
>         }
> 
>         public Product build() {
>             // 실제 Product 객체 생성 로직 (생성자 호출 또는 리플렉션 등 활용)
>             // 예시: 실제 Product 생성자가 private이고 정적 팩토리 메소드가 있다면 그것을 사용
>             Product product = new Product(/* 생성자 인자 */);
>             // ReflectionUtils 등을 사용하여 필드 설정 가능 (접근 제어자 우회 필요 시)
>             // 혹은 Product 내부에 package-private 생성자나 빌더 지원 메소드 마련
>             // ... product 필드 설정 ...
>             return product;
>         }
> 
>         // 정적 팩토리 메소드로 가독성 향상
>         public static ProductBuilder aProduct() {
>             return new ProductBuilder();
>         }
>     }
> 
>     // 테스트 코드에서 사용
>     @Test
>     void testSomething() {
>         Product defaultProduct = ProductBuilder.aProduct().build();
>         Product specificProduct = ProductBuilder.aProduct()
>                                             .name("특별 상품")
>                                             .price(50000L)
>                                             .soldOut()
>                                             .build();
>         // ... 테스트 로직 ...
>     }
>     ```

### 2. 오브젝트 마더 (Object Mother) 패턴*

> * **개념**: 테스트에서 자주 사용되는 특정 상태의 객체들을 미리 정의해놓고, 정적 팩토리 메소드를 통해 제공하는 클래스입니다. "어머니"가 필요한 객체를 "만들어" 주는 것에 비유합니다.
> * **장점**:
    >
* **단순성**: 매우 흔하게 사용되는 표준적인 객체(예: 유효한 사용자, 비활성 사용자)를 빠르게 가져와 사용할 수 있습니다.
>     * **일관성**: 모든 테스트에서 동일한 상태의 표준 객체를 사용하도록 보장합니다.
> * **단점**:
    >
* **유연성 부족**: 빌더처럼 세부 속성을 테스트마다 다르게 설정하기 어렵습니다. 새로운 상태가 필요할 때마다 팩토리 메소드를 추가해야 합니다.
>     * 객체 상태가 많아지면 클래스가 비대해질 수 있습니다.
> * **예시 (Java)**:
    >
    >   ```java
>   // TestProducts.java (테스트 소스셋에 위치)
>   public class TestProducts {
> 
>       // Object Mother는 내부적으로 Test Data Builder를 사용할 수 있음
>       public static Product aValidProduct() {
>           return ProductBuilder.aProduct()
>                                .id(1L)
>                                .name("유효한 상품")
>                                .price(20000L)
>                                .stockQuantity(5)
>                                .build();
>       }
> 
>       public static Product aSoldOutProduct() {
>           return ProductBuilder.aProduct()
>                                .id(2L)
>                                .name("품절된 상품")
>                                .price(15000L)
>                                .soldOut() // 빌더의 상태 설정 메소드 활용
>                                .build();
>       }
> 
>       public static Product aNewProductWithoutId() {
>           // ID가 없는 신규 상품 (DB 저장을 테스트할 때 등)
>            return ProductBuilder.aProduct()
>                                .id(null) // ID를 명시적으로 null로 설정
>                                .name("신규 상품")
>                                .price(30000L)
>                                .stockQuantity(20)
>                                .build();
>       }
>   }
> 
>   // 테스트 코드에서 사용
>   @Test
>   void testWithValidProduct() {
>       Product product = TestProducts.aValidProduct();
>       // ... 테스트 로직 ...
>   }
>   ```

### 3. 테스트 픽스처 클래스 / 유틸리티 클래스

> * **개념**: 특정 도메인이나 모듈 테스트에 공통적으로 필요한 데이터 생성 로직이나 설정(예: Mock 객체 설정)을 모아놓은 클래스입니다. `@BeforeEach` 등에서 이 클래스의 메소드를 호출하여
    테스트
    > 환경을 설정할 수 있습니다.
> * **장점**: 관련된 테스트 데이터 생성 로직을 한 곳에 모아 관리할 수 있습니다.
> * **단점**: 잘못 사용하면 테스트 간의 의존성이 생기거나 이해하기 어려운 코드가 될 수 있습니다. 빌더나 오브젝트 마더와 함께 보조적으로 사용하는 것이 좋습니다.

### DDD 관점에서의 고려사항

> * **유효한(Valid) 상태 표현**: 빌더나 오브젝트 마더는 도메인 규칙에 맞는 유효한 상태의 객체를 생성하는 것을 기본으로 해야 합니다.
> * **경계값(Edge Case) 표현**: 특정 비즈니스 규칙의 경계값에 해당하는 데이터를 쉽게 만들 수 있도록 빌더나 오브젝트 마더 메소드를 설계하는 것이 좋습니다. (예:
    > `ProductBuilder.aProduct().withMaximumDiscount().build()`)
> * **불변성(Immutability)**: 가능하다면 도메인 객체를 불변으로 설계하고, 테스트 데이터 빌더가 최종적으로 불변 객체를 생성하도록 하면 테스트의 안정성을 높일 수 있습니다.

### 결론 및 추천

> * **가장 우선적으로 테스트 데이터 빌더(Test Data Builder) 패턴 도입을 강력히 추천합니다.** 유연성, 가독성, 유지보수성 측면에서 가장 효과적입니다.
> * **오브젝트 마더(Object Mother) 패턴은 빌더와 함께 사용하면 시너지 효과를 낼 수 있습니다.** 매우 자주 쓰이는 표준적인 객체들을 제공하는 용도로 활용하세요. (Object Mother가
    내부적으로
    > Builder를 사용하도록 구현하면 좋습니다.)
> * 간단한 유틸리티 메소드나 `@BeforeEach` 설정은 필요에 따라 보조적으로 사용하되, 과도하게 복잡해지지 않도록 주의합니다.
    > 이 패턴들을 활용하면 DDD 환경에서 단위 테스트와 통합 테스트 전반에 걸쳐 테스트 데이터를 효과적으로 관리하고, 더 견고하고 유지보수하기 좋은 테스트 코드를 작성하는 데 큰 도움이 될 것입니다.
