# JSX

## JSX란?

**JSX(JavaScript XML)** 는 JavaScript 안에서 **HTML처럼 보이는 문법**으로 UI를 작성하는 방식이다.

```jsx
const element = <h1 className="title">Hello</h1>;
```

브라우저는 JSX를 이해하지 못한다. 빌드 도구(Babel, Vite 등)가 **일반 JavaScript 함수 호출**로 변환한다.

```jsx
// JSX
<h1 className="title">Hello</h1>

// 변환 후 (개념적)
React.createElement('h1', { className: 'title' }, 'Hello');
```

---

## JSX 기본 문법

### 1. 하나의 루트 요소

```jsx
// ✅ Fragment 사용
return (
  <>
    <h1>제목</h1>
    <p>본문</p>
  </>
);
```

### 2. JavaScript 표현식 삽입 — `{ }`

```jsx
const name = '수민';
const count = 3;

return (
  <div>
    <p>{name}</p>
    <p>{count + 1}</p>
    <p>{count > 0 ? '있음' : '없음'}</p>
  </div>
);
```

### 3. ⭐ HTML과 다른 속성 이름

| HTML | JSX | 이유 |
| --- | --- | --- |
| `class` | `className` | `class`는 JS 예약어 |
| `for` | `htmlFor` | `for`는 JS 예약어 |
| `onclick` | `onClick` | camelCase 이벤트 |
| `tabindex` | `tabIndex` | camelCase |

### 4. 스타일은 객체로

```jsx
<div style={{ color: 'red', fontSize: '16px' }}>
  빨간 글씨
</div>
```

- `style` 값은 **객체**
- CSS 속성명은 camelCase (`font-size` → `fontSize`)

---

## 조건부 렌더링

### 삼항 연산자

```jsx
{isLoggedIn ? <Dashboard /> : <LoginForm />}
```

### && 연산자 (짧은 조건)

```jsx
{error && <p className="error">{error}</p>}
```

`error`가 truthy일 때만 `<p>`가 렌더링된다.

### early return 패턴

```jsx
function Profile({ user }) {
  if (!user) return <p>로딩 중...</p>;

  return <h1>{user.name}</h1>;
}
```

---

## 리스트 렌더링

`map()`으로 배열을 JSX로 변환한다.

```jsx
const todos = [
  { id: 1, text: 'React 공부' },
  { id: 2, text: '운동' },
];

function TodoList() {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}
```

### ⭐ key Props

- 리스트 항목마다 **고유한 `key`** 가 필요하다
- React가 어떤 항목이 추가/삭제/이동했는지 식별하는 데 사용
- 배열 **인덱스를 key로 쓰는 것은** 항목 순서가 바뀔 때 비효율·버그 가능 → **고유 id 권장**

```jsx
// ✅
<li key={todo.id}>

// ⚠️ 순서 변경·삭제가 잦으면 피하기
<li key={index}>
```

---

## JSX에서 자주 하는 실수

### 1. class 대신 className

```jsx
// ❌
<div class="box">

// ✅
<div className="box">
```

### 2. 닫는 태그 누락

```jsx
// ❌
<img src="photo.jpg">

// ✅
<img src="photo.jpg" />
```

### 3. 중괄호 안에 객체를 그대로 출력

```jsx
// ❌ 객체는 React 자식으로 렌더 불가
<p>{user}</p>

// ✅ 원하는 속성만
<p>{user.name}</p>
```

### 4. if/for 문을 JSX 안에 직접 사용

```jsx
// ❌ JSX 안에서는 표현식만 가능
return (
  <div>
    {if (show) <p>보임</p>}  // 문법 오류
  </div>
);

// ✅ 삼항, &&, 또는 밖에서 처리
const content = show ? <p>보임</p> : null;
return <div>{content}</div>;
```

---

## JSX와 컴포넌트

```jsx
// 자기 닫는 태그
<Button label="저장" />

// children 포함
<Card title="알림">
  <p>내용</p>
</Card>

// Props 전달
<UserCard name="수민" age={25} isActive={true} />
```

문자열이 아닌 값(숫자, 불리언, 객체, 함수)은 **반드시 `{ }`로 감싼다**.

---

## 정리

| 항목 | 내용 |
| --- | --- |
| JSX | JS 안의 UI 문법, 빌드 시 `createElement`로 변환 |
| `{ }` | JS 표현식 삽입 |
| `className` | HTML `class` 대체 |
| `key` | 리스트 항목 식별용 고유 값 |
| 조건부 렌더링 | 삼항, `&&`, early return |

---

## 다음 단계

→ [03-Props와-State.md](./03-Props와-State.md)에서 컴포넌트에 데이터를 넘기고 관리하는 방법을 다룬다.
