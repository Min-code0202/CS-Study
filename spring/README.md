# Spring 면접 스터디

## 목적

Spring 관련 면접 질문을 정리하고
실제 면접에서 답변할 수 있는 수준까지 학습하는 것을 목표로 합니다.

## 폴더 구조

`spring/` 아래에 공통 템플릿과 챕터별 폴더가 있습니다. 각 챕터는 여러 주제 폴더로 구성되며, 주제 폴더 안에서 스터디원별 문서를 관리합니다.

```text
spring/
├── README.md
├── _template.md
├── 01-spring-core/
├── 02-spring-mvc/
├── 03-aop/
├── 04-transaction/
├── 05-jpa/
├── 06-mybatis/
├── 07-rest-api/
├── 08-spring-security/
├── 09-database-in-spring/
└── 10-practical-spring/
```

## 챕터 목록

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

## 진행 방식

- 각 챕터별로 학습합니다.
- 각 주제 폴더에서 스터디를 진행합니다.
- 스터디원은 루트의 [`_template.md`](./_template.md)를 복사하여 자신의 이름으로 파일을 생성합니다.
- 한 사람당 하나의 Markdown 파일만 사용합니다.
- 질문은 자신의 파일 안에 계속 추가합니다.
- 다른 사람의 파일은 수정하지 않습니다.

예시

```text
01-di-ioc/
├── README.md
├── jaemin.md
├── subin.md
└── junseok.md
```

## 협업 규칙

- 자신의 파일만 수정합니다.
- 다른 사람의 파일은 수정하지 않습니다.
- 하나의 PR에서는 하나의 주제만 작업합니다.
- 커밋은 가능한 작게 유지합니다.

브랜치 예시

```text
study/spring-core
study/jpa
study/security
```

커밋 예시

```text
docs: add spring core interview questions

docs: update persistence context answers

docs: organize transaction notes
```
