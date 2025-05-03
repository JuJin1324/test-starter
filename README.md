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
