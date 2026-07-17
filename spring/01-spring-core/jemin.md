# Spring Core

작성자 : 정재민

---

# DI / IoC

## Q1. DI란 무엇인가요?

<details>
<summary>답변 보기</summary>

DI는 객체가 사용할 의존 객체를 직접 생성하지 않고 외부에서 전달받는 설계 방식입니다. Spring에서는 컨테이너가 Bean을 생성한 뒤 생성자나 메서드 등을 통해 의존관계를 연결합니다. 객체 간 결합도가 낮아지고 구현 교체와 단위 테스트가 쉬워집니다.

</details>

### 꼬리질문

#### Q1. 의존성을 직접 생성하면 어떤 문제가 생기나요?

<details>
<summary>답변 보기</summary>

구현 클래스와 생성 방식에 강하게 결합되어 변경과 테스트가 어려워집니다.

</details>

#### Q2. DI의 장점은 무엇인가요?

<details>
<summary>답변 보기</summary>

관심사가 분리되고 대체 구현이나 테스트 대역을 쉽게 주입할 수 있습니다.

</details>


## Q2. IoC란 무엇인가요?

<details>
<summary>답변 보기</summary>

IoC는 객체 생성과 생명주기, 의존관계 제어권이 애플리케이션 코드가 아니라 프레임워크나 컨테이너로 넘어가는 원칙입니다. Spring IoC Container가 BeanDefinition을 바탕으로 객체를 만들고 관리하며, 개발자는 객체의 역할과 관계를 선언합니다. IoC는 Spring만의 개념이 아니라 프레임워크의 콜백이나 템플릿 메서드에서도 나타나는 일반적인 설계 원칙입니다.

</details>

### 꼬리질문

#### Q1. IoC Container에는 무엇이 있나요?

<details>
<summary>답변 보기</summary>

대표적으로 BeanFactory와 이를 확장한 ApplicationContext가 있습니다.

</details>


## Q3. DI와 IoC의 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

IoC는 제어권을 외부로 넘긴다는 넓은 설계 원칙이고, DI는 그 원칙을 구현하는 대표적인 방법입니다. 즉 Spring 컨테이너가 객체 관리를 맡는 것이 IoC이고, 필요한 의존 객체를 주입해 관계를 완성하는 과정이 DI입니다.

</details>

### 꼬리질문



## Q4. 생성자 주입을 사용하는 이유는 무엇인가요?

<details>
<summary>답변 보기</summary>

생성자 주입은 객체 생성 시 필수 의존성을 모두 전달하므로 불완전한 객체가 만들어지는 것을 막습니다. 필드를 final로 선언해 불변성을 확보할 수 있고, Spring 없이도 생성자를 호출해 테스트하기 쉽습니다. 또한 생성자 의존관계는 순환 참조를 더 이른 시점에 드러냅니다.

</details>

### 꼬리질문

#### Q1. Setter Injection은 언제 사용하나요?

<details>
<summary>답변 보기</summary>

선택적이거나 생성 이후 변경 가능해야 하는 의존성에 제한적으로 사용합니다.

</details>

#### Q2. Field Injection을 지양하는 이유는 무엇인가요?

<details>
<summary>답변 보기</summary>

의존성이 숨겨지고 final을 쓸 수 없으며 컨테이너 없는 단위 테스트가 어려워집니다.

</details>

#### Q3. 생성자가 하나인데 @Autowired를 생략할 수 있는 이유는 무엇인가요?

<details>
<summary>답변 보기</summary>

Spring 4.3부터 Bean에 생성자가 하나뿐이면 그 생성자를 자동 주입 대상으로 선택합니다.

</details>


## Q5. @Autowired는 어떻게 동작하나요?

<details>
<summary>답변 보기</summary>

컨테이너가 Bean 생성 과정에서 AutowiredAnnotationBeanPostProcessor를 통해 주입 지점을 찾습니다. 기본적으로 타입으로 후보를 검색하고 후보가 여러 개면 @Primary, @Qualifier, 이름 등의 정보를 이용해 하나를 결정한 뒤 주입합니다.

</details>

### 꼬리질문

#### Q1. 같은 타입의 Bean이 여러 개면 어떻게 되나요?

<details>
<summary>답변 보기</summary>

우선순위를 정할 정보가 없으면 NoUniqueBeanDefinitionException이 발생합니다.

</details>


## Q6. @Qualifier와 @Primary의 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

@Primary는 같은 타입 후보가 여러 개일 때 기본으로 선택할 Bean을 지정합니다. @Qualifier는 주입 지점에서 원하는 후보를 더 구체적으로 한정합니다. 둘이 함께 있으면 해당 주입 지점의 @Qualifier 조건이 우선합니다.

</details>

### 꼬리질문

#### Q1. @Qualifier는 단순히 Bean 이름으로 찾나요?

<details>
<summary>답변 보기</summary>

이름도 qualifier 값으로 활용될 수 있지만 본질은 타입 후보를 의미적으로 좁히는 것입니다.

</details>

#### Q2. 언제 @Primary를 사용하나요?

<details>
<summary>답변 보기</summary>

대부분의 주입 지점에서 사용할 기본 구현이 명확할 때 사용합니다.

</details>

## Q7. 순환 참조란 무엇인가요?

<details>
<summary>답변 보기</summary>

A가 B를 필요로 하고 B가 다시 A를 필요로 해 의존관계가 순환하는 상태입니다. 생성자 주입에서는 두 객체 중 어느 것도 먼저 완성할 수 없어 컨테이너 시작 시 오류가 납니다. 보통 책임 분리가 부족하다는 신호이므로 구조를 재설계하는 것이 우선입니다.

</details>

### 꼬리질문

#### Q1. 순환 참조를 해결하는 방법은 무엇인가요?

<details>
<summary>답변 보기</summary>

공통 책임을 별도 객체로 분리하거나 의존 방향을 단방향으로 바꾸는 것이 좋습니다.

</details>

#### Q2. @Lazy로 해결해도 되나요?

<details>
<summary>답변 보기</summary>

프록시로 생성을 지연해 우회할 수 있지만 설계 문제를 숨길 수 있어 최후의 수단입니다.

</details>

### 실무에서는

- 실무에서는 생성자 주입으로 서비스·리포지토리 관계를 명시하고 인터페이스 구현이나 테스트 대역을 교체합니다.
- 동일 타입 구현이 여러 개면 @Qualifier로 비즈니스 의미를 분명히 표현합니다.

### 주의할 점

- DI는 객체 생성 자체가 아니라 의존관계를 외부에서 연결하는 방식입니다.
- 생성자 주입이 순환 참조를 모두 해결하는 것은 아니며 문제를 빠르게 발견하게 해줍니다.

---

# Bean

## Q1. Bean이란 무엇인가요?

<details>
<summary>답변 보기</summary>

Bean은 Spring IoC Container가 생성하고 의존관계, 생명주기, 스코프를 관리하는 객체입니다. 일반 Java 객체와 클래스 자체가 다른 것이 아니라 컨테이너에 BeanDefinition이 등록되어 관리 대상이 되었다는 점이 다릅니다. BeanDefinition에는 클래스, 스코프, 생성 방식과 의존관계 같은 Bean 생성 메타데이터가 담깁니다.

</details>

### 꼬리질문


#### Q1. 모든 객체를 Bean으로 등록해야 하나요?

<details>
<summary>답변 보기</summary>

공통 정책이나 의존관계 관리가 필요한 객체를 등록하고 단순 값 객체까지 모두 등록할 필요는 없습니다.

</details>

## Q2. Bean 등록 방법은 무엇인가요?

<details>
<summary>답변 보기</summary>

대표적으로 @Component 계열을 컴포넌트 스캔으로 자동 등록하거나, @Configuration 클래스의 @Bean 메서드로 명시적으로 등록합니다. XML 설정이나 ImportSelector 같은 확장 방식도 있지만 일반 애플리케이션에서는 두 방식이 주로 사용됩니다.

</details>

### 꼬리질문

#### Q1. 언제 @Bean을 사용하나요?

<details>
<summary>답변 보기</summary>

외부 라이브러리 객체처럼 소스에 @Component를 붙일 수 없거나 생성 과정을 직접 제어할 때 사용합니다.

</details>


## Q3. @Component, @Service, @Repository, @Controller의 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

모두 컴포넌트 스캔 대상이 되는 stereotype입니다. @Component가 범용이고 나머지는 계층의 역할을 표현합니다. @Repository는 데이터 접근 예외 변환, @Controller는 MVC 요청 처리 탐지처럼 추가 의미가 있으며 @Service는 서비스 계층의 의도를 명확히 합니다.

</details>

### 꼬리질문

#### Q1. @RestController는 무엇인가요?

<details>
<summary>답변 보기</summary>

@Controller와 @ResponseBody를 결합해 반환 값을 HTTP 응답 본문으로 처리합니다.

</details>


## Q4. @Configuration과 @Bean의 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

@Configuration은 하나 이상의 Bean 정의를 담는 설정 클래스임을 나타내고, @Bean은 메서드 반환 객체를 Bean으로 등록합니다. 기본 @Configuration은 프록시를 사용해 설정 클래스 내부 @Bean 메서드 호출에서도 singleton 의미를 보장합니다.

</details>

### 꼬리질문



## Q5. BeanFactory와 ApplicationContext의 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

BeanFactory는 Bean 생성, 조회와 의존관계 주입을 제공하는 최소 컨테이너입니다. ApplicationContext는 이를 확장해 메시지 국제화, 이벤트 발행, 리소스 조회, 환경 정보와 애노테이션 기반 기능 등 애플리케이션에 필요한 기능을 추가합니다.

</details>

### 꼬리질문

#### Q1. 실무에서는 무엇을 사용하나요?

<details>
<summary>답변 보기</summary>

일반적인 Spring 애플리케이션과 Spring Boot에서는 ApplicationContext를 사용합니다.

</details>


## Q6. Singleton Bean이란 무엇인가요?

<details>
<summary>답변 보기</summary>

Spring singleton은 하나의 IoC Container에서 하나의 BeanDefinition당 객체 인스턴스 하나를 공유하는 스코프입니다. 애플리케이션 전체 JVM에 객체가 반드시 하나라는 GoF Singleton과는 다릅니다. 여러 요청이 공유하므로 상태를 두지 않는 방식으로 설계해야 합니다.

</details>

### 꼬리질문

#### Q1. 왜 기본 스코프가 singleton인가요?

<details>
<summary>답변 보기</summary>

서비스처럼 상태가 없는 객체를 재사용해 생성 비용을 줄이고 일관되게 관리할 수 있기 때문입니다.

</details>

#### Q2. Singleton Bean은 thread-safe한가요?

<details>
<summary>답변 보기</summary>

스코프가 thread safety를 보장하지 않으므로 변경 가능한 공유 상태를 두지 않아야 합니다.

</details>

#### Q3. 왜 인터페이스에 의존해야 하나요?

<details>
<summary>답변 보기</summary>

구체 구현보다 인터페이스에 의존하면 구현 교체의 영향이 줄고 테스트 대역을 주입하기 쉬워집니다. 다만 대체 가능성이나 의미 있는 역할 추상화가 없다면 모든 클래스에 기계적으로 인터페이스를 만들 필요는 없습니다.

</details>

### 실무에서는

- 서비스, 리포지토리, 인프라 구성 요소를 Bean으로 등록해 의존관계와 공통 정책을 중앙에서 관리합니다.

### 주의할 점

- Spring singleton은 컨테이너 단위이며 thread safety를 자동 보장하지 않습니다.
- stereotype 애노테이션을 단순 별칭으로만 설명하지 말고 계층 의미와 부가 기능을 함께 말합니다.

---

# Bean Lifecycle

## Q1. Bean Lifecycle을 설명해주세요.

<details>
<summary>답변 보기</summary>

일반적으로 BeanDefinition 로딩, 객체 생성, 의존관계 주입, aware 콜백, BeanPostProcessor의 전처리, 초기화 콜백, 후처리 순서로 사용 준비가 됩니다. 컨테이너 종료 시에는 소멸 콜백이 호출됩니다. 프록시 생성 등으로 실제 세부 순서는 확장 지점에 따라 달라질 수 있습니다.

</details>

### 꼬리질문

#### Q1. 초기화는 생성자에서 하면 안 되나요?

<details>
<summary>답변 보기</summary>

생성자는 값 설정에 집중하고 외부 연결처럼 의존관계 주입 이후 필요한 작업은 초기화 콜백에서 수행합니다.

</details>





## Q2. @PostConstruct와 @PreDestroy란 무엇인가요?

<details>
<summary>답변 보기</summary>

@PostConstruct는 의존관계 주입이 끝난 뒤 초기화 메서드를, @PreDestroy는 Bean 소멸 직전 정리 메서드를 지정합니다. 설정과 코드가 분리되지 않고 간단하며 Jakarta 표준 애노테이션이라 일반적인 초기화·정리에 권장됩니다.

</details>

### 꼬리질문

#### Q1. destroyMethod와 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

@Bean의 destroyMethod는 외부 라이브러리처럼 소스에 애노테이션을 붙일 수 없을 때 유용합니다.

</details>

#### Q2. @PreDestroy에서는 무엇을 하나요?

<details>
<summary>답변 보기</summary>

스레드 풀, 네트워크 연결 등 Bean이 소유한 자원을 안전하게 해제합니다.

</details>

### 실무에서는

- 초기화 콜백은 캐시 준비나 외부 연결 검증에, 소멸 콜백은 Bean이 소유한 자원 정리에 사용합니다.

### 주의할 점

- 생성자 호출과 초기화 콜백의 시점을 혼동하지 않습니다.
- prototype의 소멸 콜백은 컨테이너가 자동 관리하지 않습니다.

---

# Bean Scope

## Q1. Bean Scope란 무엇인가요?

<details>
<summary>답변 보기</summary>

Bean Scope는 Bean 인스턴스가 생성되고 공유되며 유지되는 범위를 정의합니다. 기본 singleton 외에 prototype, request, session, application, websocket 등이 있고 @Scope나 전용 애노테이션으로 지정합니다.

</details>

### 꼬리질문

#### Q1. 기본 Scope는 무엇인가요?

<details>
<summary>답변 보기</summary>

Spring Bean의 기본 스코프는 singleton입니다.

</details>

#### Q2. 웹 스코프는 어디서 사용할 수 있나요?

<details>
<summary>답변 보기</summary>

웹 인식 ApplicationContext에서 사용할 수 있습니다.

</details>

## Q2. Singleton Scope와 Prototype Scope의 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

singleton은 컨테이너마다 하나의 인스턴스를 공유하지만 prototype은 조회하거나 주입을 요청할 때마다 새 인스턴스를 생성합니다. singleton은 생명주기 전체를 관리하지만 prototype은 생성과 초기화 이후 컨테이너가 추적하지 않습니다.

</details>

### 꼬리질문



## Q3. Request Scope와 Session Scope는 언제 사용하나요?

<details>
<summary>답변 보기</summary>

request scope는 HTTP 요청 하나 동안만 유지할 요청별 정보에, session scope는 사용자 세션 동안 유지할 상태에 사용합니다. singleton에 주입할 때는 실제 인스턴스 조회를 지연하는 scoped proxy가 필요할 수 있습니다.

</details>

### 꼬리질문



### 실무에서는

- 요청 추적 정보처럼 생명주기가 명확한 데이터에 적절한 scope를 적용하되 상태 전달 방식과 비교해 선택합니다.

### 주의할 점

- scope와 thread safety는 별개입니다.
- prototype은 주입받을 때마다가 아니라 컨테이너에 요청할 때마다 생성됩니다.

---

# Component Scan

## Q1. Component Scan이란 무엇인가요?

<details>
<summary>답변 보기</summary>

지정한 패키지에서 @Component 및 이를 포함한 stereotype 애노테이션이 붙은 클래스를 탐색해 BeanDefinition으로 자동 등록하는 기능입니다. 명시적 @Bean 등록을 줄이지만 탐색 범위와 중복 등록을 관리해야 합니다.

</details>

### 꼬리질문

#### Q1. 기본 Bean 이름은 무엇인가요?

<details>
<summary>답변 보기</summary>

명시하지 않으면 일반적으로 클래스명의 첫 글자를 소문자로 바꾼 이름을 사용합니다.

</details>


## Q2. @SpringBootApplication은 어떤 역할을 하나요?

<details>
<summary>답변 보기</summary>

@SpringBootApplication은 @SpringBootConfiguration, @EnableAutoConfiguration, @ComponentScan을 결합한 애노테이션입니다. 설정 클래스를 표시하고 자동 설정을 활성화하며 해당 클래스가 위치한 패키지부터 하위 패키지를 컴포넌트 스캔합니다.

</details>

### 꼬리질문

#### Q1. 메인 클래스는 어디에 두는 것이 좋은가요?

<details>
<summary>답변 보기</summary>

애플리케이션 최상위 패키지에 두어 필요한 하위 패키지를 자연스럽게 스캔하게 합니다.

</details>

#### Q2. @EnableAutoConfiguration과 Component Scan은 같은 기능인가요?

<details>
<summary>답변 보기</summary>

전자는 조건 기반 자동 설정을 가져오고 후자는 애플리케이션 컴포넌트를 탐색하므로 역할이 다릅니다.

</details>

#### Q3. Component Scan 범위를 변경하려면 어떻게 하나요?

<details>
<summary>답변 보기</summary>

@ComponentScan의 basePackages나 basePackageClasses를 지정하거나 @SpringBootApplication의 scanBasePackages를 사용할 수 있습니다. 문자열 패키지명보다 marker class를 쓰는 basePackageClasses가 리팩터링에 안전합니다.

범위를 너무 넓게 잡으면 의도하지 않은 Bean 등록, 이름 충돌, 시작 시간 증가와 테스트 격리 문제가 생길 수 있습니다.

</details>


### 실무에서는

- 메인 클래스를 루트 패키지에 배치하고 모듈 경계가 분명하면 @Import나 명시적 스캔 범위를 사용합니다.

### 주의할 점

- @SpringBootApplication의 스캔 기준은 프로젝트 루트가 아니라 애노테이션이 선언된 클래스의 패키지입니다.

---

# Spring Boot

## Q1. Spring과 Spring Boot의 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

Spring Framework는 DI, AOP, MVC 등 애플리케이션 개발 기반을 제공합니다. Spring Boot는 Spring 위에서 의견 있는 기본값, 자동 설정, starter 의존성, 내장 서버와 운영 기능을 제공해 설정과 배포 비용을 줄입니다. 별도 프레임워크가 아니라 Spring을 빠르게 구성하는 도구입니다.

</details>

### 꼬리질문

#### Q1. Spring Boot 없이 Spring을 사용할 수 있나요?

<details>
<summary>답변 보기</summary>

가능하지만 의존성 버전, 설정과 서버 배포 등을 더 직접 구성해야 합니다.

</details>


#### Q2. Spring Boot가 왜 등장했나요?

<details>
<summary>답변 보기</summary>

기존 Spring 애플리케이션은 의존성 버전 조합, 반복 설정과 외부 서버 배포를 개발자가 직접 관리해야 했습니다. Spring Boot는 기본값, 자동 설정, Starter와 내장 서버를 제공해 초기 구성과 배포를 단순화하고 비즈니스 개발에 집중할 수 있도록 등장했습니다.

</details>

## Q2. Auto Configuration이란 무엇인가요?

<details>
<summary>답변 보기</summary>

classpath의 라이브러리, 기존 Bean, 설정 프로퍼티 등의 조건을 평가해 필요한 Bean을 자동 등록하는 기능입니다. 보통 @ConditionalOnClass와 @ConditionalOnMissingBean 등을 사용하므로 사용자가 같은 종류의 Bean을 정의하면 기본 설정이 물러나는 방식입니다.

</details>

### 꼬리질문


#### Q1. 자동 설정을 제외할 수 있나요?

<details>
<summary>답변 보기</summary>

@SpringBootApplication의 exclude 또는 spring.autoconfigure.exclude로 제외할 수 있습니다.

</details>

## Q3. Spring Boot Starter란 무엇인가요?

<details>
<summary>답변 보기</summary>

특정 기능에 필요한 의존성을 목적별로 묶어 제공하는 의존성 모음입니다. 예를 들어 spring-boot-starter-web은 Spring MVC, JSON 처리와 기본 내장 서버 관련 의존성을 일관된 버전으로 가져옵니다.

</details>

### 꼬리질문

#### Q1. Starter의 장점은 무엇인가요?

<details>
<summary>답변 보기</summary>

개별 라이브러리 선택과 호환 버전 관리 부담을 줄입니다.

</details>


## Q4. Embedded Tomcat이란 무엇인가요?

<details>
<summary>답변 보기</summary>

애플리케이션에 Tomcat이 라이브러리로 포함되어 별도 WAS 설치 없이 실행 가능한 jar로 웹 서버를 시작하는 방식입니다. Spring Boot의 spring-boot-starter-web은 기본적으로 내장 Tomcat을 포함하며 Jetty나 Undertow 등으로 교체할 수 있습니다.

</details>

### 꼬리질문

#### Q1. 장점은 무엇인가요?

<details>
<summary>답변 보기</summary>

환경별 외부 WAS 설정 차이를 줄이고 실행과 배포 단위를 단순화합니다.

</details>


## Q5. application.yml과 application.properties의 차이는 무엇인가요?

<details>
<summary>답변 보기</summary>

둘 다 Spring Boot 외부 설정 파일이며 표현 형식이 다릅니다. properties는 평평하고 단순하며 YAML은 계층 구조와 목록 표현이 편하지만 들여쓰기 오류에 주의해야 합니다. 같은 위치에 둘 다 있으면 properties가 우선하므로 한 형식으로 통일하는 것이 좋습니다.

</details>

### 꼬리질문


#### Q1. 비밀값을 파일에 넣어도 되나요?

<details>
<summary>답변 보기</summary>

저장소에 커밋하지 말고 환경 변수나 secret manager 등 외부 설정으로 주입해야 합니다.

</details>

## Q6. Profile은 언제 사용하나요?

<details>
<summary>답변 보기</summary>

local, test, prod처럼 환경에 따라 Bean이나 설정값을 다르게 적용할 때 사용합니다. @Profile로 Bean 구성을 제한하거나 application-{profile}.yml로 설정을 분리하고 spring.profiles.active 등으로 활성화합니다.

</details>

### 꼬리질문

#### Q1. Profile별 파일만 있으면 공통 설정은 어떻게 되나요?

<details>
<summary>답변 보기</summary>

기본 application 설정 위에 활성 profile의 설정이 우선순위에 따라 덮어씌워집니다.

</details>


### 실무에서는

- 자동 설정을 기본으로 사용하되 Condition Report로 적용 이유를 확인하고 필요한 Bean만 명시적으로 재정의합니다.
- 환경별 설정은 Profile과 외부 설정을 활용하고 비밀값은 저장소 밖에서 관리합니다.

### 주의할 점

- Spring Boot는 Spring을 대체하지 않습니다.
- YAML과 properties를 함께 둘 때 우선순위를 반드시 확인합니다.
- 내장 서버도 운영 환경의 스레드·커넥션·종료 설정이 필요합니다.

---

# 핵심 요약

- IoC는 객체 관리의 제어권을 컨테이너에 넘기는 원칙이고, DI는 의존 객체를 외부에서 전달하는 구현 방식입니다.
- 생성자 주입은 필수 의존성과 불변성을 명확히 하고 테스트와 순환 참조 발견에 유리합니다.
- Bean은 Spring Container가 BeanDefinition을 기반으로 생성하고 생명주기와 스코프를 관리하는 객체입니다.
- singleton Bean에는 변경 가능한 공유 상태를 두지 않으며, scope와 thread safety를 구분해야 합니다.
- Component Scan은 메인 설정 클래스의 패키지를 기준으로 동작하므로 패키지 구조가 중요합니다.
- Spring Boot는 자동 설정, starter, 내장 서버와 외부 설정으로 Spring 애플리케이션 구성을 단순화합니다.

---

# 추가 학습 내용

- Reflection
- Service Locator
- BeanDefinition
- BeanPostProcessor
- AOP Proxy
- JDK Dynamic Proxy
- CGLIB
- 영속성 컨텍스트
- Dirty Checking
- Filter와 Interceptor 차이
- HttpMessageConverter

---

# 참고 자료

- [Spring Framework: The IoC Container](https://docs.spring.io/spring-framework/reference/core/beans.html)
- [Spring Framework: Using @Autowired](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config/autowired.html)
- [Spring Framework: Bean Scopes](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html)
- [Spring Framework: Classpath Scanning](https://docs.spring.io/spring-framework/reference/core/beans/classpath-scanning.html)
- [Spring Boot: Auto-configuration](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)
- [Spring Boot: Externalized Configuration](https://docs.spring.io/spring-boot/reference/features/external-config.html)
- [Spring Boot: Profiles](https://docs.spring.io/spring-boot/reference/features/profiles.html)
- [Spring Boot: Embedded Web Servers](https://docs.spring.io/spring-boot/how-to/webserver.html)
