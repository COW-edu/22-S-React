컴포넌트는 두가지로 나뉘어진다.

Function Component

- state 사용 불가
- Lifecycle에 따른 기능 구현 불가.
- hook 으로 이러한 한계 극복

Class Component

- 생성자에서 state 정의
- setState() 함수를 통해 state 업데이트
- Lifecycle methods 제공

# hook

리액트 state와 생명주기 기능에 갈고리를 걸어 원하는 시점에 정해진 함수를 실행되도록 만들때 그 함수.

이름 앞에 use 를 붙여 훅인 것을 알려줘야함

### useState() - state를 사용하기 위한 hook

```jsx
const [변수명, set함수명] = useState(초기값);
```

useState() 를 호출할 때에는 파라미터로 선언할 state의 초기값 들어감.

리턴값으로 배열 반환,

```jsx
import React, { useState } from "react";

function Counter(props) {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>총 {count}번 클릭했습니다.</p>
      <button onClick={() => setCount(count + 1)}>클릭</button>
    </div>
  );
}
```

클래스 컴포넌트는 setState 로 모든 state 조정 가능

useState는 변수 각각에 대해 set 함수가 따로 존재.

### useEffect() - side effect를 수행하기 위한 hook.

리액트에서 side effect = 효과, 영향

서버에서 데이터 받아옴, 수동으로 돔 변경 등의 작업 → 다른 컴포넌트에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없음.

이러한 작업을 사이드에서 함.

생명주기 함수와 동일한 기능을 제공하는 함수.

```jsx
useEffect(이펙트 함수, 의존성 배열);
```

의존성 배열: 이 함수가 의존하고 있는 배열. 배열 안의 값이 변경되면 이펙트 함수 실행

이펙트 함수는 처음 렌더링된 이후와 업데이트로 인한 재렌더링에 실행

### useMemo

Memoized value를 리턴하는 hook

Memoization - 최적화를 위해 사용하는 개념

비용이 높은 함수의 호출결과를 저장, 같은 입력값으로 함수를 호출하면 전에 저장해놨던 결과를 출력.

```jsx
const memoizedValue = useMemo(
	() => {
		//연산량이 높은 작업을 수행하여 결과를 반환
		return computeExpnsiveValue(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

의존성 배열의 변수가 변했을 때에만 함수를 호출, 그렇지 않으면 기존함수의 결과 그대로 반환

빠른 렌더링 가능.

렌더링이 일어나는동안 실행. 따라서 사이드 이펙트같은 렌더링이 일어나는 동안 실행되면 안되는 기능들은 넣으면 안됨.

의존성 배열을 넣지 않을 경우, 매 렌더링마다 함수가 실행됨.

빈 배열일 경우, 컴포넌트 마운트 시에만 호출됨. 변경 X

### useCallback

useMemo() hook 과 유사, 값이 아닌 함수를 반환

렌더링될 때마다 매번 함수를 새로 정의하는 것이 아니라, 의존성 배열이 바뀐 경우에만 새로 정의

```jsx
const memoizedCallback = useCallback(
	() => {
		doSomthing(의존성 변수1, 의존성 변수 2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

의존성 배열에 있는 변수가 변하면 memoization 된 callback 함수.

```jsx
import React, {useState, useCallback} from "react";

function Counter(props) {
	const [count, setCount] = useState(0);
	//컴포넌트 마운트시에만 함수 정의
	const handleClick = useCallback(event) => {
		//클릭이벤트 처리
	}, []);

	return (
		<div>
			<p>총 {count}번 클릭했습니다.</p>
			<button onClick={() => setCount(count + 1)}>
				클릭
			</button>
		</div>
	)
}
```

### useRef()

Referance : 특정 컴포넌트에 접근할 수 있는 객체

이 레퍼런스 객체를 반환.

refObject.current → 현재 참조하고 있는 Element

```jsx
const refContainer = useRef(초기값);
```

파라미터로 초기값을 넣으면 해당 초기값으로 초기화된 레퍼런스 반환.

컴포넌트 생명주기 전체에 유지.

변경 가능한 current 속성을 가진 하나의 상자.

```jsx
function TextInputWithFocusButton(props) {
	const inputElem = useRef(null);

	const onButtonClick = () => {
		//'current' 는 마운트된 Input element를 가리킴
		inputElem.current.focus();
	};

	return(
		<>
			<input ref={inputElem} type="text" />
			<button onClick={onButtonClick)>
				Focus the input
			</button>
		</>
	);
}
```

```jsx
<div ref={myRef} />
```

노드가 변경될 때마다 myRef의 current 속성에 현재 해당되는 DOM 노드 저장.

다양한 변수 저장 장점.

매번 렌더링 될 때마다 같은 레퍼런스 객체 반환

내부의 데이터가 변경되었을 때 별도로 알리지 않는다.

Callback ref

다른 노드에 연결될 때마다 호출.

DOM 노드의 변화를 알기 위한 방법.

```jsx
function MesureExample(props) {
  const [height, setHeight] = useState(0);

  const measuredRef = useCallback((node) => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height);
    }
  }, []);

  return (
    <>
      <h1 ref={measuredRef}> 안녕, 리액트</h1>
      <h2> 위 헤더의 높이는 {Math.round(height)}px 입니다.</h2>
    </>
  );
}
```

자식 컴포넌트가 변경되었을 때 알림을 받을 수 있고, 이를 통해 다른 정보 업데이트 가능.

## Hook의 규칙

### Hook은 무조건 최상위 레벨에서만 호출해야 한다.

리액트 함수 컴포넌트의 최상위 레벨을 의미. 반복문이나 조건문, 중첩된 함수 안에서 hook 호출 하면 안됨.

hook은 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 한다.

state 올바르게 사용해야됨.

잘못된 Hook 사용법

```jsx
function MyComponent(props) {
	const [name, setName] = useState('Inje');

	if (name !== '') {
		useEffect(() => {
		...
		});
	}

	...
}
```

중간의 조건문의 결과에 따라 호출되는 hook 이 달라진다.

### 리액트 함수 컴포넌트에서만 hook을 호출해야 한다.

일반적인 JS 함수에서는 안됨.

state 와 관련된 모든 로직은 소스코드를 통해 명확하게 확인이 가능해야함.

eslint-plugin-react-hooks.

리액트 컴포넌트가 hook 규칙을 따르는지 확인해줌.
