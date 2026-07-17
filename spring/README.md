# Spring 면접 스터디

## 스터디 목적

Spring의 핵심 개념과 실무 면접 질문을 챕터 단위로 정리하고, 실제 면접에서 30초~1분 안에 설명할 수 있는 수준까지 학습합니다.

## 전체 챕터

- [Spring Core](./01-spring-core/README.md)
- [Spring MVC](./02-spring-mvc/README.md)
- [AOP](./03-aop/README.md)
- [Transaction](./04-transaction/README.md)
- [JPA](./05-jpa/README.md)
- [MyBatis](./06-mybatis/README.md)
- [REST API](./07-rest-api/README.md)
- [Spring Security](./08-spring-security/README.md)
- [Database in Spring](./09-database-in-spring/README.md)
- [Practical Spring](./10-practical-spring/README.md)

## 폴더 구조

각 챕터 아래에는 주제별 하위 폴더를 두지 않습니다. 스터디원은 챕터마다 자신의 Markdown 파일 하나를 만들고, 세부 주제를 Heading으로 구분해 같은 파일 안에 계속 작성합니다.

```text
spring/
├── README.md
├── _template.md
├── 01-spring-core/
│   ├── README.md
│   ├── jaemin.md
│   └── subin.md
├── 02-spring-mvc/
│   ├── README.md
│   └── jaemin.md
├── 03-aop/
├── 04-transaction/
├── 05-jpa/
├── 06-mybatis/
├── 07-rest-api/
├── 08-spring-security/
├── 09-database-in-spring/
└── 10-practical-spring/
```

예를 들어 Spring Core의 DI / IoC, Bean, Bean Lifecycle, Bean Scope, Component Scan, Spring Boot는 모두 `jaemin.md`와 같은 하나의 파일 안에서 Heading으로 관리합니다.

## 작성 방법

1. 루트의 [`_template.md`](./_template.md)를 복사합니다.
2. 학습할 챕터 폴더 안에 자신의 이름 또는 GitHub ID로 파일을 생성합니다.
3. 템플릿의 챕터명과 주제 Heading을 해당 챕터에 맞게 변경합니다.
4. 새로운 질문은 같은 파일 안에 계속 추가합니다.
5. 답변은 `<details>` 태그 안에 작성해 접고 펼칠 수 있도록 구성합니다.

## 협업 규칙

- 한 사람당 챕터별 Markdown 파일 하나만 사용합니다.
- 다른 사람의 파일은 수정하지 않습니다.
- 하나의 PR에는 하나의 챕터만 포함합니다.
- 질문은 파일 안에서 Heading으로 계속 추가합니다.
- 답변은 `<details>` 태그를 사용하여 접고 펼칠 수 있도록 작성합니다.
