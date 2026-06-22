# Hooks

Hooks는 **함수 컴포넌트**에서 State, 생명주기, 기타 React 기능을 쓰게 해주는 API다.  
이름이 `use`로 시작한다 (`useState`, `useEffect` 등).

---

## Hooks 규칙

1. **최상위에서만** 호출 — 반복문, 조건문, 중첩 함수 안에서 호출 금지
2. **React 함수 컴포넌트** 또는 **커스텀 Hook** 안에서만 호출

```jsx
// ❌ 조건부 Hook
if (show) {
  const [x, setX] = useState(0);
}

// ✅
const [x, setX] = useState(0);
if (!show) return null;
```

React가 Hook 호출 **순서**로 상태를 매칭하기 때문에 순서가 항상 같아야 한다.

---

## useState

컴포넌트 내부 State를 선언한다.

```jsx
const [value, setValue] = useState(initialValue);
```

- [03-Props와-State.md](./03-Props와-State.md) 참고
- 함수형 업데이트: `setValue(prev => prev + 1)`

---

## useEffect

**렌더 이후** 부수 효과(side effect)를 실행한다.

```jsx
import { useEffect, useState } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then((res) => res.json())
      .then(setUser);
  }, [userId]); // 의존성 배열

  if (!user) return <p>로딩...</p>;
  return <h1>{user.name}</h1>;
}
```

### 의존성 배열

| 패턴 | 동작 |
| --- | --- |
| `useEffect(fn)` | **매 렌더 후** 실행 |
| `useEffect(fn, [])` | **마운트 시 1회** (빈 배열) |
| `useEffect(fn, [a, b])` | `a` 또는 `b`가 바뀔 때 실행 |

### cleanup (정리 함수)

```jsx
useEffect(() => {
  const timer = setInterval(() => {}, 1000);

  return () => {
    clearInterval(timer); // 언마운트 또는 다음 effect 전에 실행
  };
}, []);
```

타이머, 구독, 이벤트 리스너 등 **정리가 필요한 작업**에 사용한다.

### useEffect로 하지 말아야 할 것

- 렌더 결과를 계산하는 로직 → `useMemo` 또는 렌더 중 계산
- 이벤트 핸들러 로직 → `onClick` 등에 직접 작성
- Props/State로부터 파생 값 → 렌더 중 계산

---

## useRef

**리렌더 없이** 값을 유지하거나 DOM 노드에 접근한다.

```jsx
import { useRef, useEffect } from 'react';

function TextInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current?.focus();
  }, []);

  return <input ref={inputRef} />;
}
```

| 용도 | 예시 |
| --- | --- |
| DOM 참조 | `inputRef.current.focus()` |
| 이전 값 저장 | `const prev = useRef()` |
| 타이머 ID 저장 | `const timerId = useRef()` |

`ref.current`를 바꿔도 **리렌더가 발생하지 않는다**.

---

## useMemo

**비용이 큰 계산** 결과를 메모이제이션한다.

```jsx
const sortedList = useMemo(
  () => items.sort((a, b) => a.date - b.date),
  [items]
);
```

의존성이 바뀔 때만 다시 계산한다.

---

## useCallback

**함수 참조**를 메모이제이션한다.

```jsx
const handleDelete = useCallback((id) => {
  setItems((prev) => prev.filter((item) => item.id !== id));
}, []);
```

`React.memo`로 감싼 자식에게 함수 Props를 넘길 때 유용하다.

```jsx
const MemoList = React.memo(ItemList);

// MemoList에 handleDelete 전달 시 참조 안정화
<MemoList onDelete={handleDelete} />
```

---

## useContext

Context 값을 구독한다. Props drilling을 줄일 때 쓴다.

```jsx
const ThemeContext = createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <div className={theme}>...</div>;
}
```

- 자주 바뀌는 값을 Context에 넣으면 **구독 컴포넌트가 많이 리렌더**될 수 있다
- 전역 테마, 로그인 사용자 등 **넓게 공유되는 값**에 적합

---

## 커스텀 Hook

공통 로직을 함수로 추출한다. 이름은 반드시 `use`로 시작.

```jsx
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// 사용
const [theme, setTheme] = useLocalStorage('theme', 'light');
```

커스텀 Hook은 **상태 로직의 재사용**이 목적이다.

---

## 자주 쓰는 Hooks 요약

| Hook | 용도 |
| --- | --- |
| `useState` | State 선언 |
| `useEffect` | 부수 효과 (fetch, 구독, DOM) |
| `useRef` | DOM 참조, 변경해도 리렌더 없는 값 |
| `useMemo` | 계산 결과 캐싱 |
| `useCallback` | 함수 참조 캐싱 |
| `useContext` | Context 값 읽기 |
| `useReducer` | 복잡한 State를 reducer로 관리 |

### useReducer 간단 예시

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 });

<button onClick={() => dispatch({ type: 'increment' })}>+</button>
```

여러 하위 필드를 한꺼번에 다루거나, State 전이 로직이 복잡할 때 `useState` 대신 고려한다.

---

## 클래스 생명주기와 Hooks 대응

| 클래스 | Hooks |
| --- | --- |
| `componentDidMount` | `useEffect(fn, [])` |
| `componentDidUpdate` | `useEffect(fn, [deps])` |
| `componentWillUnmount` | `useEffect`의 cleanup |
| `this.state` | `useState` / `useReducer` |

---

## 정리

| 개념 | 핵심 |
| --- | --- |
| Hooks | 함수 컴포넌트에서 State·effect 사용 |
| 규칙 | 최상위, 컴포넌트/커스텀 Hook 내부만 |
| useEffect | 렌더 후 실행, 의존성 배열로 제어 |
| 커스텀 Hook | 로직 재사용 단위 |

---

## 다음 단계

→ [07-컴포넌트-설계-패턴.md](./07-컴포넌트-설계-패턴.md)에서 실무에서 자주 쓰는 설계 패턴을 다룬다.
