# Spring Core

작성자 : junseok

---

# DI / IoC

## Q1. IoC와 DI란 무엇이며, 두 개념은 어떤 관계가 있나요?

<details>
<summary>답변 보기</summary>

IoC(Inversion of Control)는 객체의 생성, 의존관계 설정, 생명주기 관리에 대한 제어권을 개발자 코드가 아니라 프레임워크나 컨테이너에 맡기는 원칙입니다. DI(Dependency Injection)는 객체가 필요한 의존 객체를 직접 생성하거나 찾지 않고 외부에서 전달받는 방식입니다. 따라서 DI는 IoC를 구현하는 대표적인 방법이며, Spring 컨테이너는 빈을 생성한 뒤 필요한 의존성을 주입하여 객체 간 관계를 완성합니다.

</details>

### 꼬리질문

#### Q. DI를 사용하면 객체 간의 결합도가 어떻게 낮아지나요?

<details>
<summary>답변 보기</summary>

객체가 구체 구현을 직접 생성하지 않고 인터페이스나 추상 타입을 통해 의존성을 전달받기 때문입니다. 예를 들어 서비스가 `new JdbcRepository()`를 호출하지 않고 `Repository`를 생성자로 받으면, 서비스 코드를 바꾸지 않고도 다른 구현이나 테스트 대역으로 교체할 수 있습니다. 즉, 객체는 의존 객체의 생성 방법과 위치를 몰라도 되므로 결합도가 낮아집니다.

</details>

## Q2. 생성자 주입, 수정자 주입, 필드 주입의 차이점은 무엇이며 생성자 주입을 권장하는 이유는 무엇인가요?

<details>
<summary>답변 보기</summary>

생성자 주입은 객체 생성 시 의존성을 전달하고, 수정자 주입은 객체 생성 후 setter 메서드로 전달하며, 필드 주입은 필드에 직접 주입합니다. 생성자 주입은 필수 의존성을 누락한 불완전한 객체 생성을 막고, 필드를 `final`로 선언해 불변성을 보장할 수 있습니다. 또한 순환 참조 문제를 가장 빠르게 발견할 수 있어서 가장 권장됩니다.

</details>

### 꼬리질문

#### Q. 생성자 주입을 사용할 때 순환 참조가 발생하면 스프링은 어떻게 동작하나요?

<details>
<summary>답변 보기</summary>

A가 생성자로 B를 요구하고 B도 생성자로 A를 요구하면 어느 객체도 먼저 완성할 수 없습니다. Spring 컨테이너는 빈 생성 과정에서 이를 감지하여 일반적으로 `BeanCurrentlyInCreationException`을 원인으로 애플리케이션 컨텍스트 초기화에 실패합니다. `@Lazy`나 수정자 주입으로 지연시킬 수는 있지만 근본 해결책은 아니므로, 공통 책임을 별도 객체로 분리하거나 의존 방향을 단방향으로 재설계하는 것이 좋습니다.

</details>

---

# Bean

## Q3. 스프링 빈(Spring Bean)이란 무엇이며, 스프링 컨테이너는 빈을 어떻게 관리하나요?

<details>
<summary>답변 보기</summary>

Spring Bean은 Spring IoC 컨테이너가 생성하고 조립하며 관리하는 객체입니다. 컨테이너는 `@Component`, `@Bean`, XML 등의 설정을 `BeanDefinition`이라는 메타데이터로 등록하고, 이를 바탕으로 빈을 생성합니다. 이후 의존관계 주입, 초기화 콜백, 스코프에 따른 인스턴스 관리, 컨테이너 종료 시 소멸 콜백 등을 수행합니다. 기본 스코프는 컨테이너마다 하나의 인스턴스를 관리하는 싱글톤입니다.

</details>

### 꼬리질문

#### Q. `BeanFactory`와 `ApplicationContext`의 차이점은 무엇인가요?

<details>
<summary>답변 보기</summary>

`BeanFactory`는 빈 생성, 조회, 의존관계 주입 등 IoC 컨테이너의 기본 기능을 제공하는 최상위 인터페이스입니다. `ApplicationContext`는 `BeanFactory`를 확장하여 국제화, 이벤트 발행, 리소스 조회, 환경 정보, AOP 연동 같은 애플리케이션 개발 기능을 추가로 제공합니다. 또한 일반적인 `ApplicationContext`는 시작 시 싱글톤 빈을 미리 생성해 설정 오류를 조기에 발견합니다. 

</details>

## Q4. `@Bean`과 `@Component`의 차이점은 무엇이며, 각각 어떤 상황에서 사용하나요?

<details>
<summary>답변 보기</summary>

`@Component`는 클래스 자체에 붙여 컴포넌트 스캔으로 자동 등록할 때 사용합니다. 주로 직접 작성한 `@Controller`, `@Service`, `@Repository` 같은 클래스에 적합합니다. `@Bean`은 `@Configuration` 클래스의 메서드에 붙이며, 메서드가 반환한 객체를 빈으로 등록합니다. 생성 과정에 별도 설정이 필요하거나 소스 코드를 수정할 수 없는 외부 라이브러리 객체를 등록할 때 유용합니다.

</details>

### 꼬리질문

#### Q. 외부 라이브러리의 객체를 스프링 빈으로 등록하려면 어떤 방식을 사용해야 하나요?

<details>
<summary>답변 보기</summary>

외부 라이브러리 클래스에는 직접 `@Component`를 붙일 수 없으므로, 일반적으로 `@Configuration` 클래스에서 해당 객체를 생성하는 메서드에 `@Bean`을 선언합니다. 이 방식은 생성자 인자나 프로퍼티 등 객체 생성 과정을 명시적으로 설정할 수 있다는 장점도 있습니다.

</details>

---

# Bean Lifecycle

## Q5. 스프링 빈의 생명주기를 설명해 주세요.

<details>
<summary>답변 보기</summary>

일반적인 싱글톤 빈은 먼저 설정 메타데이터가 등록된 뒤 인스턴스가 생성되고, 의존관계가 주입됩니다. 그다음 `Aware` 계열 콜백과 `BeanPostProcessor`의 초기화 전 처리를 거쳐 `@PostConstruct`, `InitializingBean`, 지정한 init 메서드 등의 초기화 콜백이 실행됩니다. 초기화 후 처리까지 끝나면 빈을 사용할 수 있고, 컨테이너 종료 시 `@PreDestroy`, `DisposableBean`, 지정한 destroy 메서드 등의 소멸 콜백이 호출됩니다.

</details>

### 꼬리질문

#### Q. 빈 생성과 의존관계 주입 중 어떤 과정이 먼저 수행되나요?

<details>
<summary>답변 보기</summary>

원칙적으로 빈 인스턴스를 먼저 생성한 다음 의존관계를 주입합니다. 다만 생성자 주입은 생성자를 호출해야 객체가 만들어지므로, 필요한 의존 빈을 먼저 준비한 뒤 생성과 동시에 주입합니다. 수정자 주입과 필드 주입은 인스턴스를 생성한 후 수행되며, 초기화 콜백은 모든 의존관계 주입이 끝난 뒤 호출됩니다.

</details>

## Q6. 빈의 초기화 및 소멸 콜백을 구현하는 방법에는 무엇이 있으며, 어떤 방법을 권장하나요?

<details>
<summary>답변 보기</summary>

초기화와 소멸 콜백은 `@PostConstruct`·`@PreDestroy`, `InitializingBean`·`DisposableBean` 인터페이스, 그리고 `@Bean(initMethod, destroyMethod)` 또는 XML 설정으로 지정할 수 있습니다. 일반적으로는 코드가 간결하고 Spring 전용 인터페이스에 의존하지 않는 `@PostConstruct`와 `@PreDestroy`를 권장합니다. 외부 라이브러리처럼 소스 코드를 수정할 수 없는 객체에는 `@Bean`의 init/destroy 메서드 지정 방식이 적합합니다.

</details>

### 꼬리질문

#### Q. `@PostConstruct`와 `@PreDestroy`를 사용하는 방식의 장점은 무엇인가요?

<details>
<summary>답변 보기</summary>

메서드에 애너테이션만 붙이면 되어 간결하고, 초기화·정리 로직을 해당 클래스 내부에 둘 수 있습니다. 또한 Spring의 `InitializingBean`이나 `DisposableBean` 인터페이스에 의존하지 않으므로 비즈니스 코드와 프레임워크의 결합을 줄일 수 있습니다. 컴포넌트 스캔과 `@Bean`으로 등록한 빈 모두에 일관되게 적용할 수 있다는 장점도 있습니다.

</details>

---

# Bean Scope

## Q7. 스프링이 제공하는 빈 스코프의 종류와 각각의 특징을 설명해 주세요.

<details>
<summary>답변 보기</summary>

기본 스코프인 `singleton`은 하나의 Spring 컨테이너에서 빈 정의당 인스턴스 하나를 공유하고, `prototype`은 빈을 조회하거나 주입할 때마다 새 인스턴스를 만듭니다. 웹 환경에서는 HTTP 요청마다 생성되는 `request`, HTTP 세션마다 생성되는 `session`, `ServletContext`마다 생성되는 `application`, WebSocket 세션마다 생성되는 `websocket` 스코프를 제공합니다. 특히 프로토타입은 컨테이너가 생성과 초기화까지만 담당하고 소멸 콜백은 관리하지 않는다는 점에 주의해야 합니다.

</details>

### 꼬리질문

#### Q. 싱글톤 빈이 프로토타입 빈을 주입받을 때 발생할 수 있는 문제와 해결 방법은 무엇인가요?

<details>
<summary>답변 보기</summary>

싱글톤 빈은 생성될 때 의존성을 한 번만 주입받으므로, 프로토타입 빈을 직접 주입해도 동일한 인스턴스를 계속 사용하게 됩니다. 매번 새 인스턴스가 필요하다면 `ObjectProvider<PrototypeBean>`의 `getObject()`를 호출하거나, `Provider`, 조회 메서드 주입, 스코프 프록시 등을 사용할 수 있습니다. 이 중 `ObjectProvider`는 필요한 시점에 컨테이너에서 새 프로토타입 빈을 조회하는 간단한 방법입니다.

</details>

---

# Component Scan

## Q8. 컴포넌트 스캔은 어떻게 동작하며, 빈 이름이 중복되면 어떻게 처리되나요?

<details>
<summary>답변 보기</summary>

컴포넌트 스캔은 지정된 기준 패키지부터 클래스를 탐색하여 `@Component`와 이를 포함하는 `@Controller`, `@Service`, `@Repository`, `@Configuration` 등을 찾아 `BeanDefinition`으로 등록합니다. 기본 빈 이름은 클래스명의 첫 글자를 소문자로 바꾼 값이며 애너테이션 값으로 직접 지정할 수도 있습니다. 서로 다른 클래스가 같은 빈 이름으로 등록되면 충돌이 발생해 일반적으로 애플리케이션 시작이 실패합니다. 이름 충돌을 피하려면 명시적인 빈 이름을 사용하거나 스캔 범위를 조정해야 합니다.

</details>

### 꼬리질문

#### Q. 컴포넌트 스캔의 탐색 범위를 지정하거나 특정 클래스를 제외하려면 어떻게 해야 하나요?

<details>
<summary>답변 보기</summary>

`@ComponentScan`의 `basePackages` 또는 타입 안전한 `basePackageClasses`로 탐색 시작 위치를 지정할 수 있습니다. 특정 대상을 제외하려면 `excludeFilters`에 애너테이션, 할당 가능한 타입, 정규식 등의 필터를 설정합니다. Spring Boot의 `@SpringBootApplication`에는 컴포넌트 스캔이 포함되어 있으므로, 보통 메인 클래스를 최상위 패키지에 두어 하위 패키지를 자동으로 탐색하게 합니다.

</details>

---

# Spring Boot

## Q9. Spring Framework와 Spring Boot의 차이점은 무엇인가요?

<details>
<summary>답변 보기</summary>

Spring Framework는 DI, AOP, 트랜잭션, MVC 등 애플리케이션 개발의 핵심 인프라를 제공하지만 필요한 의존성과 설정을 개발자가 직접 구성해야 하는 부분이 많습니다. Spring Boot는 Spring Framework 위에서 동작하며, 스타터 의존성, 자동 설정, 내장 웹 서버, 외부 설정, Actuator 등을 제공해 초기 설정과 배포를 단순화합니다. 즉 서로 경쟁하는 기술이 아니라, Spring Boot가 Spring 기반 애플리케이션을 더 빠르게 구성하고 실행하도록 돕는 관계입니다.

</details>

### 꼬리질문

#### Q. Spring Boot의 스타터(Starter)는 어떤 역할을 하나요?

<details>
<summary>답변 보기</summary>

스타터는 특정 기능을 개발하는 데 필요한 라이브러리 의존성을 목적별로 묶어 제공하는 의존성 집합입니다. 예를 들어 `spring-boot-starter-web`을 추가하면 Spring MVC, JSON 처리, 기본 내장 웹 서버 등 웹 개발에 필요한 호환 버전의 의존성이 함께 들어옵니다. 덕분에 라이브러리를 하나씩 선택하거나 버전 호환성을 직접 관리하는 부담을 줄일 수 있습니다.

</details>

## Q10. Spring Boot의 자동 설정(Auto Configuration)은 어떤 원리로 동작하나요?

<details>
<summary>답변 보기</summary>

`@SpringBootApplication`에 포함된 `@EnableAutoConfiguration`이 자동 설정을 활성화합니다. Spring Boot는 클래스패스에 존재하는 라이브러리, 이미 등록된 빈, 설정 프로퍼티, 웹 애플리케이션 여부 등을 조건 애너테이션으로 검사합니다. `@ConditionalOnClass`, `@ConditionalOnMissingBean`, `@ConditionalOnProperty` 같은 조건이 충족된 자동 설정만 적용해 필요한 빈을 등록합니다. 사용자가 같은 용도의 빈을 직접 정의하면 `@ConditionalOnMissingBean` 조건이 맞지 않아 기본 자동 설정이 물러나는 방식으로 커스터마이징할 수 있습니다.

</details>

### 꼬리질문

#### Q. 특정 자동 설정을 제외하거나 직접 정의한 빈으로 대체하려면 어떻게 해야 하나요?

<details>
<summary>답변 보기</summary>

자동 설정 전체 클래스를 제외하려면 `@SpringBootApplication(exclude = SomeAutoConfiguration.class)` 또는 `spring.autoconfigure.exclude` 프로퍼티를 사용합니다. 일부 기본 빈만 바꾸고 싶다면 같은 타입이나 이름의 사용자 빈을 직접 정의하여 `@ConditionalOnMissingBean` 기반 자동 설정이 적용되지 않게 할 수 있습니다. 단, 모든 자동 설정이 같은 조건을 사용하는 것은 아니므로 `--debug`로 조건 평가 보고서를 확인해 실제 적용 조건을 파악하는 것이 좋습니다.

</details>

---

# 핵심 요약

- IoC는 객체 관리의 제어권을 컨테이너에 맡기는 원칙이고, DI는 이를 구현하는 대표적인 방법입니다.
- Spring Bean은 컨테이너가 설정 메타데이터를 바탕으로 생성하고 의존관계, 생명주기, 스코프를 관리하는 객체입니다.
- 의존성 주입은 필수 의존성과 불변성, 테스트 편의성, 순환 참조의 조기 발견에 유리한 생성자 주입을 우선합니다.
- 기본 빈 스코프는 싱글톤이며, 프로토타입과 웹 전용 스코프는 수명과 관리 범위가 서로 다릅니다.
- 컴포넌트 스캔은 애너테이션이 붙은 클래스를 자동 등록하고, `@Bean`은 생성 과정을 직접 제어하거나 외부 객체를 등록할 때 사용합니다.
- Spring Boot는 스타터와 조건 기반 자동 설정으로 Spring 애플리케이션의 구성과 실행을 단순화합니다.

---

# 추가 학습 내용

- `BeanPostProcessor`와 프록시 기반 AOP가 빈 생명주기에 개입하는 방식
- `@Primary`, `@Qualifier`를 이용한 동일 타입 빈 선택 방법
- 지연 초기화(`@Lazy`)와 스코프 프록시의 동작 원리
- Spring Boot 조건 평가 보고서를 이용한 자동 설정 디버깅

---

# 참고 자료

- [Spring Framework - IoC Container](https://docs.spring.io/spring-framework/reference/core/beans.html)
- [Spring Framework - Dependency Injection](https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-collaborators.html)
- [Spring Framework - Bean Scopes](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html)
- [Spring Boot - Auto-configuration](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)
- [Spring Boot - Build Systems and Starters](https://docs.spring.io/spring-boot/reference/using/build-systems.html)
