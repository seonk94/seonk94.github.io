---
title:  "Typescript Utility Types - Partial"
search: true
categories: 
  - Typescript
  - Partial
last_modified_at: 2020-06-25T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Typescript
article_tag2: Partial
article_section: Typescript Partial
meta_keywords: Typescript,typescript,Partial,partial,Utility-Types,Utility
---

TypeScript는 공통 유형 변환을 용이하게 하기 위해 몇 가지 유틸리티 유형을 제공한다. 이러한 유틸리티는 전세계적으로 이용할 수 있다.

## Partial<T>

T의 모든 속성을 옵션으로 설정한 유형을 생성한다. 이 유틸리티는 주어진 유형의 모든 하위 집합을 나타내는 유형을 반환한다.

Example
```ts

interface Todo {
    title: string;
    description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
    return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
    title: '집에 가기',
    description: '가는 길에 과일 사기',
};

const todo2 = updateTodo(todo1, {
    description: '물도 사기'
});

```

| [Typescript 참고 문서](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialt)