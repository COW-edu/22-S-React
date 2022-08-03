# 2022.7.26 Cow 프론트엔드 공부

---

- [x]  인프런 React 강의 Section7(1~2)

---

Hooks(갈고리)

Component

- Function Component
- Class Component


Class Component의 경우 state와 관련된 기능 뿐만 아니라 Component의 생명주기 함수까지 모두 명확하게 정해져 있어 가져다 쓰기만 하면 됨.

but

Function Component는 코드가 간결, 별도로 state 정의해서 사용하거나 Component의 생명주기에 맞춰 어떤 코드가 실행되게 할 수 없음

⇒ 이를 지원하기 위해 등장한 것이 Hooks(Class Component의 기능을 모두 동일하게 구현가능)

갈고리라는 뜻 → 원래 존재하는 어떤 기능에 마치 갈고리를 거는 것처럼 끼어 들어가 같이 수행됨

Hooks의 이름은 모두 use로 시작

---

useState()

- state를 사용하기 위한 Hook
- 사용법

```jsx
const [변수명, set함수명] = useState(초기값);
```

‘[ ]’ 사용이유? - 배열 구조 분해 할당

ex) 버튼을 누를 시 count가 증가

```jsx
import React, {useState} from "react";

function Counter(props) {
//count를 Counter함수의 변수로 선언
	var count = 0;

	return (
		<div>
			<p>총 {count}번 클릭했습니다.</p>
			<button onClick={() => count++}>
				클릭
			</button>
		</div>
	);
}
```

문제점

→ count를 함수의 변수로 선언하여 사용할 경우 버튼 클릭 시 count의 값이 증가하지만 재렌더링이 일어나지 않아 화면에 표시 X

```jsx
import React, {useState} from "react";

function Counter(props) {
	//useState()를 통해 count를 state로 관리하도록 만듦
	const [count, setCount] = useState(0);

	return (
		<div>
			<p>총 {count}번 클릭했습니다.</p>
			<button onClick={() => setCount(count+1)}>
				클릭
			</button>
		</div>
	);
}
```

useState()를 사용하여 count값을 state로 관리하도록 만듦

⇒Class Component에서 setState()함수를 이용해 state를 업데이트하고 화면에 재렌더링하는 과정과 비슷

---

useEffect()

- Side effect를 수행하기 위한 Hook
- Side effect란? 개발자가 의도치 않은 코드가 실행되면서 버그가 발생하는 것 (부정적)
but
- React에서 Side effect는 효과,영향이라는 의미, 다른 컴포넌트에 영향을 미칠 수 있으며 렌더링 중에는 작업이 완료될 수 없는 작업을 뜻함 ⇒ 렌더링이 끝난 후에 실행되어야 하는 작업
ex) 서버에서 데이터를 받아오는 것, 수동으로 DOM을 변경하는 작업
- 생명주기 함수와 동일한 기능을 수행할 수 있음
- 사용법

```jsx
useEffect(이펙트 함수, 의존성 배열);
//배열안에 있는 변수 중 하나라도 값이 변경되었을 때 이펙트 함수가 실행됨
useEffect(이펙트 함수);
//의존성 배열 생략 가능 -> 컴포넌트가 업데이트 될때마다 호출
```

---

useMemo()

- Memoized value를 리턴하는 Hook

<aside>
💡 Memoization
연산량이 많은 함수의 호출 결과를 저장해두었다가 같은 입력값으로 함수를 호출할 경우 저장해두었던 호출 결과를 바로 반환하는 것

</aside>

- Memoized value = Memoization된 결과값
- 사용법

```jsx
const memoizedValue = useMemo(
	() => {
		//연산량이 높은 작업을 수행하여 결과를 반환
		return computeExpensiveValue(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

의존성 배열에 들어있는 변수가 변했을 경우에만 새로 create함수를 호출하여 결과값을 반환

Component가 다시 렌더링될 때마다 연산량이 높은 작업을 반복하는 것을 피할 수 있음

⇒빠른 렌더링 속도

```jsx
//의존성 배열을 넣지 않을 경우, 렌더링마다 함수가 실행 됨
const memoizedValue = useMemo (
	() => computeExpensiveValue(a,b)
);
```

```jsx
//의존성 배열이 빈 배열일 경우, Component Mount시에만 호출
const memoizedValue = useMemo(
	() => {
		return computeExpensiveValue(a,b);
	},
	[]
);
```

---

useCallback()

- useMemo()와 유사하지만 값이 아닌 함수를 반환
- 의존성 배열이 바뀐 경우 함수를 새로 정의하여 리턴
- 사용법

```jsx
const memoizedCallback = useCallback(
	() => {
//변수로서 받는 함수를 Callback이라 부름
		doSomething(의존성 변수1, 의존성 변수1);
	},
	[의존성 변수1, 의존성 변수2]
);
//의존성 변수가 바뀔 시 memoization된 Callback함수를 반환
```

---

useRef()

- Reference를 사용하기 위한 Hook(Reference객체를 반환)

<aside>
💡 Reference
특정 Component에 접근할 수 있는 객체
refObject.current = 현재 참조하고 있는 Element

</aside>

- 사용법

```jsx
const refContainer = useRef(초깃값);
//변수로 초깃값을 넣으면 해당 초깃값으로 초기화된 reference객체를 반환
//반환된 reference객체는 Component Mount해제 전까지 유지
```

```jsx
<div ref={myRef} />
// Node가 변경될 때마다 myRef current속성에 현재 해당되는 DOM Node를 저장
```

- 내부의 데이터가 변경되었을 때 별도로 알리지 않는다
- current속성을 변경한다고 해서 재렌더링이 발생하는 것은 X

Callback ref

React는 ref가 다른 Node에 연결될 때마다 Callback을 호출