# State and Lifecycle

## state

- React Component의 상태(변경 가능한 데이터)를 말한다
- state는 개발자가 직접 정의하여 사용한다
- JS의 객체이다

### state 유의사항

- 렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 한다
    - state가 변경될 경우 component가 재렌더링 되기 때문

### 사용되지  않은 state 해결법

- component instance로 정의하면 된다
    - component instance
        - class로 선언된 component들만 가지는 instance
        - component class 내부에서 this 키워드를 통해 참조하는 대상에 해당
        - 지역 상태를 저장하고 life cycle event들에 대한 반응을 제어할 때 매우 유용

### 상태의 종류

- 지역상태
    - 특정 컴포넌트 안에서만 관리되는 상태
    - 다른 컴포넌트들과 데이터를 공유하지 않는다
    - 보통 Form 데이터들이 지역상태에 속한다.
- 컴포넌트 간 상태
    - 여러가지 컴포넌트에서 관리되는 상태
    - 다수의 컴포넌트에서 쓰이고, 또 영향을 미치는 상태
    - Prop Drilling 방식을 필요로 한다.
- 전역 상태
    - 프로젝트 전체에 영향을 끼치는 상태
    - Prop Drilling방식을 활용하여 부모에서 자식으로 데이터를 전달한다

### Prop Drilling

- props를 오직 상위 컴포넌트에서 하위 컴포넌트로 전달하는 용도로만 쓰이는 컴포넌트를 거치며 컴포넌트에서 다른 컴포넌트로 데이터를 전달하는 과정
- [상위 컴포넌트 > 중간 컴포넌트 > 중간 컴포넌트 > ... > 타겟 컴포넌트]

### constructor

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b2e235c1-c16b-4105-a9e8-87f479b5527f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220726%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220726T124318Z&X-Amz-Expires=86400&X-Amz-Signature=e2fe94eda6b47f033b02978cfee60418307b8077ce2e278ceb8a95e25c2fa58b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 모든 class component는 constructor(생성자)라는 함수를 가지고 있다
- this.state가 현재 component의 상태를 정의하는 부분이다
- class component의 경우 state를 생성자에서 정의한다

### super

**Constructor 내부에서 this.props로 접근하기 위해서 super(props)를 하는것**

### setState

- component rendering과 관련이 있기에 직접 수정하면 의도대로 작동을 안할수도 있다

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a5e4a59e-d9a1-4353-a078-6f3e2397678b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220726%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220726T124339Z&X-Amz-Expires=86400&X-Amz-Signature=c7e68c3c6b977d5164e0d1450810775ccb6e020a225135bbe3a6badaa8160b9e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- setState함수를 사용하여 state를 수정할 수 있다

## Life cycle(생명주기)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3715d731-5976-477c-b972-2b86b0ae7832/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220726%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220726T124355Z&X-Amz-Expires=86400&X-Amz-Signature=8d6a689e1cedff0ac279ce6b5720988b56bb16d50ca25fd1c6f3d4ae4a1a8e62&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### Mount(출생)

- constructor로 생성자를 실행. state를 정의
- componentDidMount
    - 컴포넌트가 웹브라우저에 나타난 후 호출
    - 화면에 모두 그려진 후에 실행되어야 하는 이벤트 처리, 초기화, 비동기 작업 처리 등 가장 많이 활용

### Update

- 컴포넌트가 update해가는 과정
- update의 요인 4가지
    - 부모 컴포넌트에서 넘겨주는 props의 값이 바뀔 때
    - setState에 의해 state가 바뀔 때
    - 부모 컴포넌트의 리렌더링으로 자식 컴포넌트도 리렌더링이 될 때
    - this.forceUpdate 함수로 인한 렌더링 트리거가 일어날때

### Unmount

- 상위 컴포넌트에서 현재 컴포넌트를 화면에 표시하지 않게 될 때 unmount 됨
- component는 시간의 흐름에 따라 mount-update-unmount되어 결국 사라진다

## State & Life cycle 실습

- Notification
    - 역할
        1. 스타일 지정
        2. Notification class의 빈 state 만들기
    - Life cycle
        - 컴포넌트가 mount후, update후, unmount전 출력될 콘솔을 함수를 통해 작성한다.
            - NotificationList에서 추가한 id를 함께 출력하도록 콘솔에 id를 추가한다.(빽콤 사용)
- NotificationList
    - 역할
        1. Notification component를 목록 형태로 보여주기 위한 component
        2. reservedNotifications에 출력할 텍스트 작성
        3. constructor에서 빈 배열 만들기(초기화)
        4. componentDidMount에서는 1초마다 출력할 텍스트를 가져와 빈 배열에 넣고 업데이트 한다(업데이트 하기 위해 setState를 사용함)
    - Life cycle
        - reservedNotifications에 id를 추가하고 render에서 return받을 부분 안에 key와 id를 추가해서 console에서 life cycle의 로그를 구별할 수 있게 설정한다
            - key는 react element를 구분하기 위한 고유한 값인데 map함수를 사용할 때 필수적으로 들어가줘야 한다
                - map함수는 react에서 반복문에 사용됨
        - unmount까지 확인하기 위해선 componentDidMount에서 알림이 끝나면 setState로 constructor에서 만들었던 배열명을 다시 빈 배열로 만들어주도록 한다

# Hooks

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b60e3aa3-991b-4fc1-abf6-0bad8eb4e120/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220726%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220726T124417Z&X-Amz-Expires=86400&X-Amz-Signature=139addbb5973a69fe06520bc6aca0e31654151d93ee752bb712f850f917e0b5c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- Hooks를 통해 Class Component에서만 가능했던 기능을 모두 사용할 수 있게 됨
- 원래 존재한 어떤 기능에 끼어 들어가 같이 수행되게 함

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9944bfb9-4034-416d-aac8-c09d7ca33e38/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220726%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220726T124431Z&X-Amz-Expires=86400&X-Amz-Signature=4e7ff939a19f02fa444e4a38287a4e0cb4748456c80f135cfcc90c8e88ef9db6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 함수 컴포넌트안에 Hooks을 걸어 원할 때 관련 함수를 사용할 수 있게 함
- 각 함수들은 모두 use로 시작함 (Hooks을 나타냄)

## useState

- state를 사용하기 위한 Hook
- 함수 컴포넌트에서는 state를 사용하지 않기에 useState를 통해 사용함

### useState사용법

```jsx
const [변수명, set함수명] = useState(초기값);
```

### useState예시

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b95399e8-c277-4fd0-9325-6031f0aaf740/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220726%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220726T124450Z&X-Amz-Expires=86400&X-Amz-Signature=03ca26badb957c21f4722706d8a947abb3ca6a303546084a3436993d8e6d9c4c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 변수명 = count      set함수명 = setCount
- 초기값은 0<<
- count 값이 변경되면 컴포넌트값이 변경되며 재랜더링 되기에 화면에 변경값이 나오게 된다
- useState에는 변수 각각에 대해 set함수가 따로 존재한다

## useEffect

- side effect를 수행하기 위한 Hook
- effect에 관한 작업들은 다른 컴포넌트에 영향을 미칠 수 있고 렌더링  중에는 작업이 완료될 수 없기에 side로 실행시키겠다는 의미에서 side effect를 사용하고 이를 실행할 수 있게 해주는 Hook이 useEffect이다
- mount에 사용되는 함수들을 하나로 통합해서 기능을 제공함
- return으로 받는 것이 life cycle의 unmount와 역할이 같다 (return 안에 있는 것이 unmount되기 전에 실행 됨)

### side effect

- react에서의 side effect는 부작용이 아닌 효과, 영향으로 쓰임
- effect : 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 작업 등을 의미

### useEffect() 사용법

```jsx
useEffect(이펙트 함수, 의존성 배열);
```

의존성 배열 :  이 effect가 의존하고 있는 배열, 배열 안에 있는 값이 변경되면 effect 함수가 실행 됨

### Effect함수가 실행되는 경우

- 처음 컴포넌트가 렌더링 된 이후
- 업데이트 인한 재렌더링 이후

### Effect함수 실행 조정하기

```jsx
useEffect(이펙트 함수, []);
```

- Effect function을 mount와 unmount시에 단 한 번씩만 실행되게 하는 방법이다
- 의존성 배열에 빈 배열을 넣음으로서 실행을 조절할 수 있다
    - 해당 effect가 props나 state에 있는 어떤 것도 의존하지 않기에 여러번 실행되지 않음

```jsx
useEffect(이펙트 함수);
```

- 의존성 배열을 생략하면 컴포넌트가 업데이트될 때마다 호출됨

### useEffect 예제

```jsx
useEffect(() => {
	//의존성 배열에 따라 실행되는 것들
	...
	return () => {
	// 컴포넌트가 마운트 해제되기 전에 실행되는 것들
	...
	}
}, [의존성 변수1, 의존성 변수2, ...]);
```

## useMemo

- Memoized value를 리턴하는 Hook

### Memoized value

- Memoization
    - 최적화를 위해서 사용
    - 연산량이 많이 드는 호출 결과를 저장해두었다가 같은 입력값으로 함수를 호출하면 새로 함수를 호출하지 않고 이전에 저장해둔 함수 결과를 바로 반환
- Memoization된 결과 값을 Memoized value라고 한다

### useMemo 사용법

```jsx
const memoizedValue = useMemo(
	() => {
		// 연산량이 높은 작업을 수행하여 결과를 반환
		return computeExpensiveValue(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

- 의존성 배열에 있는 값이 변했을 때만 결과값을 반환하고 그렇지 않다면 기존 값을 그대로 반환
- useMemo로 전달되는 함수는 렌더링이 이루어지는 동안 실행된다 (sideEffect에서 이루어 지는 것들이 useMemo에서 실행되면 안되는 이유)
- 의존성 배열을 넣지 않으면 매 렌더링마다 함수가 실행됨으로 useMemo의 의미가 없어진다
- 의존성 배열이 빈 배열일 경우 mount될 때만 호출이 된다

## useCallback

- useMemo와 유사하지만 값이 아닌 함수를 반환한다

### useCallback 사용법

```jsx
const memoizedCallback = useCallback(
	() => {
		doSomething(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

- useMemo와 마찬가지로 함수와 의존성 배열을 parameter로 받음 (parameter로 받는 함수를 callback이라고 함)
- 의존성 변수가 하나라도 변하면 callback함수를 반환함

## useRef

- Reference를 사용하기 위한 Hook
- 내부의 데이터가 변경되어도 별도로 알리지 않음
    - Callback ref로 알 수 있다

### Reference

- 특정 컴포넌트에 접근할 수 있는 객체
- Reference에서 사용하는 current라는 속성은 현재 reference하고 있는 Element이다.

### useRef 사용법

```jsx
const refContainer = useRef(초깃값);
```

- 초깃값을 넣으면 해당 초깃값으로 초기화된 reference객체를 반환
    - ex) 초깃값이 null이라면 current값이 null인 객체가 반환됨
- mount 해제 전까지 유지됨
- 변경 가능한 current 속성을 가진 상자

## Hook의 규칙

- Hook은 최상위 레벨에서만 호출해야 한다
    - 반복문이나 조건문, 중첩된 함수내에서 호출하면 안됨
    - 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 됨(useState와 useEffect를 올바르게 호출해서 state를 올바르게 관리하기 위해서)


# Hooks

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b60e3aa3-991b-4fc1-abf6-0bad8eb4e120/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220728T103314Z&X-Amz-Expires=86400&X-Amz-Signature=430f644c7ab2c5258c0b5cfd8b30bc4fe41b67c0c35e1963b60c1348a9c530c5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- Hooks를 통해 Class Component에서만 가능했던 기능을 함수 컴포넌트에서도 사용할 수 있게 됨
- 함수 컴포넌트는 렌더링 될 때마다 내부의 것들을 다시 계산해야 하기에 Hooks가 필요하다
- 원래 존재한 어떤 기능에 끼어 들어가 같이 수행되게 함

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9944bfb9-4034-416d-aac8-c09d7ca33e38/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220728T103327Z&X-Amz-Expires=86400&X-Amz-Signature=f0f9a3953c5c3a2ac78cdcd75a99f1724593d3d18ed65fb9e106e13296aecf0e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 함수 컴포넌트안에 Hooks을 걸어 원할 때 관련 함수를 사용할 수 있게 함
- 각 함수들은 모두 use로 시작함 (Hooks을 나타냄)

## useState

- state를 사용하기 위한 Hook
- 상태 관리를 한다
- 변수를  새로고침 없이 갱신할 수 있다
    
    ```jsx
    let num = 0;
    <button onClick = {() => {num ++}}>버튼</button>
    ```
    
    - 위와 같은 경우엔 웹에서는 숫자가 바뀌는걸 볼 수 없지만 useState를 사용하면 새로고침 없이 렌더링을 해줘서 웹에도 적용이 된다
- 함수 컴포넌트에서는 state를 사용하지 않기에 useState를 통해 사용함

### useState사용법

```jsx
const [변수명, set함수명] = useState(초기값);
```

### useState예시

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b95399e8-c277-4fd0-9325-6031f0aaf740/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220728%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220728T103351Z&X-Amz-Expires=86400&X-Amz-Signature=3817397a4451c35b50721048433282b39f8ae360f790820369c222c10d31c125&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 변수명 = count      set함수명 = setCount
- 초기값은 0
- count 값이 변경되면 컴포넌트값이 변경되며 재 랜더링 되기에 화면에 변경값이 나오게 된다
- useState에는 변수 각각에 대해 set함수가 따로 존재한다

## useEffect

- side effect를 수행하기 위한 Hook
- 렌더링 직후 작업을 설정한다
    - 렌더링  될 때마다 특정작업을 수행하도록 설정할 수 있는 Hook
- effect에 관한 작업들은 다른 컴포넌트에 영향을 미칠 수 있고 렌더링  중에는 작업이 완료될 수 없기에 side로 실행시키겠다는 의미에서 side effect를 사용하고 이를 실행할 수 있게 해주는 Hook이 useEffect이다
- mount에 사용되는 함수들을 하나로 통합해서 기능을 제공함
    - componentDidMount와 componentDidUpdate를 합친 형태와 유사
- return으로 받는 것이 life cycle의 unmount와 역할이 같다 (return 안에 있는 것이 unmount되기 전에 실행 됨)
- 외부 API를 요청하거나, 반복작업, 작업예약 등에 쓰인다

### side effect

- react에서의 side effect는 부작용이 아닌 효과, 영향으로 쓰임
- effect : 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 작업 등을 의미

### useEffect() 사용법

```jsx
useEffect(이펙트 함수, 의존성 배열);
```

의존성 배열 :  이 effect가 의존하고 있는 배열, 배열 안에 있는 값이 변경되면 effect 함수가 실행 됨

### Effect함수가 실행되는 경우

- 처음 컴포넌트가 렌더링 된 이후
- 업데이트 인한 재렌더링 이후

### Effect함수 실행 조정하기

```jsx
useEffect(이펙트 함수, []);
```

- Effect function을 mount와 unmount시에 단 한 번씩만 실행되게 하는 방법이다
- 의존성 배열에 빈 배열을 넣음으로서 실행을 조절할 수 있다
    - 해당 effect가 props나 state에 있는 어떤 것도 의존하지 않기에 여러번 실행되지 않음

```jsx
useEffect(이펙트 함수);
```

- 의존성 배열을 생략하면 컴포넌트가 업데이트될 때마다 호출됨

### useEffect 예제

```jsx
useEffect(() => {
	//의존성 배열에 따라 실행되는 것들 ex)console.log(input1)
	...
	return () => {
	// 컴포넌트가 마운트 해제되기 전에 실행되는 것들
	// ex) console.log('뒷정리'); 를 넣었다면 input1이 언마운트 되기전에 '뒷정리'라는 글자를 콘솔에 표시함
	...
	}
}, [의존성 변수1, 의존성 변수2, ...]) //ex) 의존성 변수1에 input1을 넣었다면 input1에 값이 입력되어 업데이트될 때 특정 작업을 수행함;
```

## useMemo

- Memoized value를 리턴하는 Hook
- 특정 값들이 다시 실행되면서 초기화시키고 싶지 않을 때, 해당 값이 변하기 전까지 저장해놓을 수 있다.
- useRef는 일반적인 값을, useMemo는 복잡한 함수의 리턴값을 기억

### Memoized value

- Memoization
    - 최적화를 위해서 사용
    - 연산량이 많이 드는 호출 결과를 저장해두었다가 같은 입력값으로 함수를 호출하면 새로 함수를 호출하지 않고 이전에 저장해둔 함수 결과를 바로 반환
- Memoization된 결과 값을 Memoized value라고 한다

### useMemo 사용법

```jsx
const memoizedValue = useMemo(
	() => {
		// 연산량이 높은 작업을 수행하여 결과를 반환
		return computeExpensiveValue(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

- 의존성 배열에 있는 값이 변했을 때만 결과값을 반환하고 그렇지 않다면 기존 값을 그대로 반환
- useMemo로 전달되는 함수는 렌더링이 이루어지는 동안 실행된다 (sideEffect에서 이루어 지는 것들이 useMemo에서 실행되면 안되는 이유)
- 의존성 배열을 넣지 않으면 매 렌더링마다 함수가 실행됨으로 useMemo의 의미가 없어진다
- 의존성 배열이 빈 배열일 경우 mount될 때만 호출이 된다

## useCallback

- useMemo와 유사하지만 값이 아닌 함수를 반환한다
- 함수 그 자체를 기억
- useCallback에서 state값은 최초의 상태가 계속 고정되기 때문에, useEffect처럼 값을 갱신하는 조건을 지정해주어야 한다.

### useCallback 사용법

```jsx
const memoizedCallback = useCallback(
	() => {
		doSomething(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

- useMemo와 마찬가지로 함수와 의존성 배열을 parameter로 받음 (parameter로 받는 함수를 callback이라고 함)
- 의존성 변수가 하나라도 변하면 callback함수를 반환함

## useRef

- Reference를 사용하기 위한 Hook
- HTML에서 id를 사용해 DOM에 이름을 다는 것처럼 useRef를 이용해 DOM에 이름을 달 수 있다
- 내부의 데이터가 변경되어도 별도로 알리지 않음
    - Callback ref로 알 수 있다
- mount 해제 전까지 유지됨
    - ref를 사용하는 경우는 임의로 값을 변경하지 않는다면 값이 계속 유지
- 변경 가능한 current 속성을 가진 상자
    - 값이 변경되어도 컴포넌트의 리렌더링이 없어 빠른 참조가 가능하다.

### Reference

- 특정 컴포넌트에 접근할 수 있는 객체
- Reference에서 사용하는 current라는 속성은 현재 reference하고 있는 Element이다.

### useRef를 사용하는 경우

- React가 직접 Dom을 선택해야 하는 경우 사용
    - 특정 엘리먼트의 크기나 색상을 가져오는 경우
    - 스크롤의 위치를 가지고 와야 하는 경우
    - 대상에 대한 포커스를 설정하는 경우

### useRef를 사용하는 이유

- 컴포넌트 안에 있는 DOM에 id를 지정한 후 해당 컴포넌트를 여러번 재사용하게 됐을 경우 id는 유일해지지 않아 문제가 생길 수 있다. —> ref를 사용

### useRef 사용법

```jsx
const refContainer = useRef(초깃값);
```

- 초깃값을 넣으면 해당 초깃값으로 초기화된 reference객체를 반환
    - ex) 초깃값이 null이라면 current값이 null인 객체가 반환됨
    - useRef()를 이용해 Ref 객체 refContainer를 만듦
- 리액트에 있는 모든 컴포넌트는 reference element를 가지고 있어서 어떤 컴포넌트에 ref={변수명}을 넣어준다면 리액트에서 해당 컴포넌트를 참조하게 된다.
    - 예시 - 초기화 버튼을 누르면 input에 포커스를 주는 경우
        
        ```jsx
        import React, { useState, useRef } from "react";
        
        function UseRefStudy() {
          const [testValue, setTestValue] = useState("");
          const testInput = useRef(); //Ref객체를 만듦
        
          const onChange = (e) => {
            setTestValue(e.target.value);
          };
        
          const onReset = () => {
            setTestValue("");
            testInput.current.focus(); //Ref 객체의 current값이 원하는 DOM을 가리키게 됨
        															 //focus()를 통해 input에 focus가 잡히게 됨
          };
        
          return (
            <div>
              <input
                placeholder="test"
                onChange={onChange}
                value={testValue}
                ref={testInput} //DOM에 객체를 ref값으로 설정해줌
              />
              <button onClick={onReset}>초기화</button>
            </div>
          );
        }
        
        export default UseRefStudy;
        ```
        

## Hook의 규칙

- Hook은 최상위 레벨에서만 호출해야 한다
    - 반복문이나 조건문, 중첩된 함수 내에서 호출하면 안됨
    - 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 됨(useState와 useEffect를 올바르게 호출해서 state를 관리하기 위해서)
- React 함수 컴포넌트에서만 Hook을 호출해야 한다.

## Custom Hook

- 여러 컴포넌트에서 반복해서 사용되는 로직을 hook으로 만들기 위해 사용