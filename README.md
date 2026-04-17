# CS Study Repository

## 스터디 소개

이 저장소는 CS 핵심 과목을 챕터 단위로 학습하고 기록하기 위한 스터디용 레포지토리입니다.

대상 과목:

- network
- os
- java
- spring
- database

## 진행 방식

- 주 1회 진행
- 발표 + 질문 기반 학습
- 각 챕터별 학습 정리 및 질문 문서화

## 폴더 구조

```text
.
├─ network/
├─ os/
├─ java/
├─ spring/
├─ database/
├─ common/
└─ docs/
```

각 과목 폴더는 아래 구조를 따릅니다.

```text
{subject}/
├─ chapter01/
├─ chapter02/
├─ chapter03/
├─ chapter04/
└─ template/
```

각 `chapterXX` 폴더에는 다음 파일을 포함합니다.

- `README.md`: 해당 챕터 학습 내용 정리
- `questions.md`: 질문 및 답변 정리

## 참여 방법 (PR 기반)

1. 개인 브랜치 생성
2. 해당 챕터/과목 문서 작성
3. Pull Request 생성
4. 리뷰 후 머지

## 브랜치 전략

- `feat/이름/챕터/과목` 형식 사용
- 예시: `feat/minsu/chapter02/os`

## 커밋 컨벤션

- `feat`: 챕터별 학습 내용 추가
- `docs`: 문서 수정
- `fix`: 문서 오류 수정
- `chore`: 구조/설정 변경

예시:

- `feat: add chapter03 spring summary`
- `docs: update questions template`

