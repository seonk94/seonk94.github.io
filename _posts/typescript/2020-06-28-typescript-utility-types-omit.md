---
title:  "Typescript Utility Types - Omit, Exclude, Extract"
search: true
categories: 
  - Typescript
tag:
  - Omit
  - Exclude
  - Extract
last_modified_at: 2020-06-28T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Typescript
article_tag2: Omit, Exclude, Extract
article_section: Typescript Omit, Exclude, Extract
meta_keywords: Typescript,Omit,Exclude,Utility-Types,Utility,Extract
---

TypeScript는 유틸리티 유형중 Omit, Exclude, Extract 알아보기.

## Omit<T, K>

T에서 모든 프로퍼티를 선택한 다음 K를 제거한 타입을 구성합니다.

([Pick](https://seonk94.github.io/typescript/typescript-utility-types-partial/#pick) 과 유사한 기능)

```ts
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Omit<Todo, 'description'>;

// type TodoPreview = Pick<Todo, 'title' | 'completed'>;

const todo : TodoPreview = {
    title: '집에 가기',
    completed: false,
};
```

## Exclude<T, U>

T에서 U에 할당할 수 있는 모든 속성을 제외한 타입을 구성합니다.

```ts
type T0 = Exclude<"A" | "B" | "C", "A">; // "B" | "C";
type T1 = Exclude<"A" | "B" | "C", "A" | "B">; // "C"
type T2 = Exclude<string | number | (() => void), Function>; // string | number
```

|  Pick + Exclude = Omit

```ts
type Omit<T, U extends keyOf T> = Pick<T, Exclude<keyof T, U>>
```

## Extract<T, U>

T에서 U에 할당할 수 있는 모든 속성을 추출하여 타입을 구성합니다.

```ts
type T0 = Extract<"A" | "B" | "C" | "D", "A" | "F">; // "A"
type T1 = Extract<string | number | (() => void), Function>; // () => void
```

---

| [Typescript 참고 문서](https://www.typescriptlang.org/docs/handbook/utility-types.html#omit)