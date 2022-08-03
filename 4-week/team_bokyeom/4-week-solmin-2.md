# 2022.7.27 Cow 프론트엔드 공부

---

- [x]  인프런 React 강의 Section7(3~4)
- [x]  추가 조사(useMemo,useCallback)

---

Hook의 규칙

- 무조건 최상위 레벨에서만 호출해야 함
즉, 반복문이나 조건문 또는 중첩된 함수들 안에서 Hook 호출 X

⇒ Hook은 Component가 렌더링될 때마다 매번 같은 순서로 호출되어야 함

- React 함수 내에서만 Hook을 호출해야 함

eslint-plugin-react-hooks(규칙 두 가지를 강제)

Hook의 규칙을 따르도록 강제하는 plugin (코드분석도구)

Custom Hook 만들기

- 여러 Component에서 반복적으로 사용되는 로직을 Hook으로 만들어 재사용하기 위함

---

useCallback

- Component의 Optimization(최적화)를 위해 사용
- useMemo와 다르게 인자로 받은 Callback함수 자체를 Memoization해줌
- Function Component를 렌더링 할 경우 모든 내부 변수가 초기화됨 → useCallback의 인자로 함수를 넣어 Memoizaton할 경우 초기화 X
- 구조

첫 번째 인자는 Memoization해줄 Callback함수, 두 번째로는 의존성 배열을 받음

```jsx
useCallback(() => {
	return value;
}, [item])
//의존성 배열이 바뀔 경우 Callback함수도 새로운 객체를 생성
```

[https://www.youtube.com/watch?v=XfUF9qLa3mU](https://www.youtube.com/watch?v=XfUF9qLa3mU)

---

useMemo

- Component의 Optimization(최적화)를 위해 사용
- 처음 계산된 결과값을 메모리에 저장, 함수 호출될 경우 Memoize된 값을 재사용
- 구조

첫 번째 인자로는 Callback함수(Memoization해줄 값을 계산해서 return해주는 함수), 두 번째 인자로는 배열(의존성 배열)을 받음

```jsx
const value = useMemo(() => {
	return calculate();
}, [item]);
```

[https://www.youtube.com/watch?v=e-CnI8Q5RY4](https://www.youtube.com/watch?v=e-CnI8Q5RY4)

---

useEffect

- 인자로 Callback함수를 받음

<aside>
💡 Callback 함수란?
다른 함수의 인자로 전달된 함수

</aside>

- Callback함수 내부에 우리가 처리하고 싶은 코드를 작성하면 됨
- 구조

useEffect()의 인자로 하나만 받을 경우

```jsx
useEffect(()=>{
	//작업...
});
//렌더링 될때마다 실행
```

useEffect()의 첫 인자로 Callback함수를 받고 두번째 인자로 배열을 받을 경우

```jsx
useEffect(()=>{
	//작업...
},[value]);
//Dependency array
//화면에 첫 렌더링 될때 실행
//value 값이 바뀔 때 실행
//빈 배열일 경우 화면에 첫 렌더링 될때 실행
```

예시

```jsx
import React, {useState, useEffect} from 'react';

function App(){
	const [count, setCount] = useState(1);

	const handleCountUpdate = () => {
		setCount(count+1);
	};

	// 매번 렌더링 될때마다 실행됨
	useEffect(() => {
		//내용 ;
	})

	return (
		<div>
			<button onClick={handleCountUpdate}>Update</button>
			<span>count: {count}</span>
		</div>
	);
}

export default App;
```

1. Update버튼이 눌리면 handleCountUpdate함수 실행
2. handleCountUpdate함수는 setCount를 사용하여 count값을 증가시킴

cf. 화면이 첫 렌더링되거나 버튼을 눌러 재렌더링 될 경우useEffect()가 실행됨

[https://www.youtube.com/watch?v=kyodvzc5GHU](https://www.youtube.com/watch?v=kyodvzc5GHU)