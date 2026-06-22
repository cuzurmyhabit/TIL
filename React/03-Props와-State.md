# Props와 State

컴포넌트에 데이터를 넣는 방법은 크게 두 가지다.

- **Props**: 부모가 자식에게 **내려주는** 값 (읽기 전용)
- **State**: 컴포넌트 **내부에서 스스로 관리**하는 값 (변경 가능)

---

## Props

### 정의

Props(properties)는 **부모 컴포넌트 → 자식 컴포넌트**로 전달되는 인자다.

```jsx
function UserBadge({ name, role }) {
  return (
    <span>
      {name} ({role})
    </span>
  );
}

// 부모
<UserBadge name="수민" role="admin" />
```

### ⭐ Props는 불변(immutable)

자식 컴포넌트는 **Props를 수정하면 안 된다**.

```jsx
// ❌ Props 직접 변경 금지
function Bad({ count }) {
  count = count + 1; // 안 됨
}

// ✅ 변경이 필요하면 부모의 State를 바꾸고, 새 Props를 내려받음
```

데이터 흐름: **단방향 (부모 → 자식)**

```text
부모 State 변경 → 새 Props → 자식 리렌더
```

### 기본값과 구조 분해

```jsx
function Button({ label = '확인', variant = 'primary' }) {
  return <button className={variant}>{label}</button>;
}
```

### children도 Props

```jsx
function Panel({ title, children }) {
  return (
    <section>
      <h2>{title}</h2>
      {children}
    </section>
  );
}
```

---

## State

### 정의

State는 **시간에 따라 바뀌는 컴포넌트 내부 데이터**다. 값이 바뀌면 React가 해당 컴포넌트를 **다시 렌더링**한다.

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

| 반환값 | 역할 |
| --- | --- |
| `count` | 현재 상태 값 |
| `setCount` | 상태를 업데이트하는 함수 |
| `useState(0)` | 초기값 `0` |

### ⭐ setState는 비동기·배칭된다

`setCount`를 호출해도 **바로 다음 줄에서** 갱신된 값을 읽을 수 없다.

```jsx
setCount(count + 1);
console.log(count); // 아직 이전 값

// 이전 값 기준 업데이트 — 함수형 업데이트
setCount((prev) => prev + 1);
```

연속 클릭 등에서 최신 값을 기준으로 더할 때는 **함수형 업데이트**를 쓴다.

```jsx
// ❌ 같은 이벤트에서 3번 올려도 1만 증가할 수 있음
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);

// ✅
setCount((prev) => prev + 1);
setCount((prev) => prev + 1);
setCount((prev) => prev + 1);
```

---

## Props vs State 비교

| | Props | State |
| --- | --- | --- |
| **소유** | 부모 | 해당 컴포넌트 |
| **변경** | 부모만 변경 | `setState`로 변경 |
| **방향** | 부모 → 자식 | 컴포넌트 내부 |
| **용도** | 설정, 표시용 데이터 | 사용자 입력, 토글, 로딩 등 |

### 언제 State로 올릴까? (State 끌어올리기)

여러 자식이 **같은 값**을 공유하거나, 부모가 값을 제어해야 할 때 State를 **공통 부모**로 올린다.

```jsx
function Parent() {
  const [text, setText] = useState('');

  return (
    <>
      <Input value={text} onChange={setText} />
      <Preview text={text} />
    </>
  );
}
```

```text
        Parent (text State)
       /              \
   Input            Preview
 (value, onChange)   (text Props)
```

---

## 제어 컴포넌트 vs 비제어 컴포넌트

### 제어 컴포넌트 (Controlled)

입력값을 **React State**가 소유한다.

```jsx
function SearchBox() {
  const [query, setQuery] = useState('');

  return (
    <input
      value={query}
      onChange={(e) => setQuery(e.target.value)}
    />
  );
}
```

- React가 입력값의 **단일 진실 공급원(Single Source of Truth)**
- 대부분의 폼에서 권장

### 비제어 컴포넌트 (Uncontrolled)

DOM이 값을 직접 들고, `ref`로 읽는다.

```jsx
function SearchBox() {
  const inputRef = useRef(null);

  const handleSubmit = () => {
    console.log(inputRef.current.value);
  };

  return <input ref={inputRef} defaultValue="" />;
}
```

- 간단한 폼, 파일 input 등에 가끔 사용

---

## 객체·배열 State 다루기

State를 **직접 변경(mutate)하지 말고** 새 객체/배열을 만들어 교체한다.

```jsx
const [user, setUser] = useState({ name: '수민', age: 20 });

// ❌
user.age = 21;
setUser(user);

// ✅
setUser({ ...user, age: 21 });

// 배열 추가
setTodos([...todos, newTodo]);

// 배열 삭제
setTodos(todos.filter((t) => t.id !== id));
```

React는 **참조(reference)가 바뀌었는지**로 변경을 감지하는 경우가 많다.

---

## 정리

| 개념 | 핵심 |
| --- | --- |
| Props | 부모 → 자식, 읽기 전용 |
| State | 내부에서 관리, 변경 시 리렌더 |
| 단방향 데이터 흐름 | 데이터는 위에서 아래로 |
| State 끌어올리기 | 공유 State는 공통 부모에 |
| 불변 업데이트 | 객체·배열은 복사 후 수정 |

---

## 다음 단계

→ [04-렌더링-흐름.md](./04-렌더링-흐름.md)에서 State가 바뀔 때 React가 화면을 갱신하는 과정을 다룬다.
