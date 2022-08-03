# 4week-1

## Hooks

- class형 component는 state와 생명주기 함수를 활용하여 사용한다. 그러나 함수형 컴포넌트는 state와 생명주기 함수를 사용할 수 없다. → 함수형 컴포넌트는 단순한 구조의 UI 컴포넌트를 제작할 때 많이 사용된다고 한다.
- Hooks을 사용하면 함수형 컴포넌트도 state와 생명주기 함수와 동일한 기능을 구현할 수 있다.
- 주의사항
    - 무조건 최상위 레벨에서만 호출해야한다. (반복문, 조건문, 중첩문에서 호출하면 안됨. )
    - 리액트 함수 컴포넌트에서만 hook을 호출해야 한다.
- Custom hook 만들기
    - custom hook을 만드는 이유는, 여러 컴포넌트에서 반복해서 사용되는 로직을 hook으로 만들어서 재사용하기 위해서이다.
    - custom hook이란, 이름이 use로 시작하고 내부에서 다른 hook을 호출하는 하나의 자바스크립트 함수라고 한다.

## 자주 사용되는 hook의 종류

- useState() : state를 사용하기 위한 hook.
    
    ```jsx
    const [변수명, set함수명] = useState(초기값);
    // class의 setState()와는 달리, 변수 각각에 set함수가 따로 존재한다.  
    ```
    
- useEffect() : class 생명 주기 함수의 componentDidMount, componentDidUpdate, componentWillUnmount를 통합하여 제공한다고 생각하면 쉽다.
    
    ```jsx
    useEffect(이펙트 함수, 의존성 배열); 
    // mount와 unmount를 한 번만 실행하도록 하고싶다면 의존성 배열에 []를 입력한다. 
    // 의존성 배열을 생략할 경우 component가 업데이트 될 때마다 실행된다. 
    ```
    
- useMemo() : memoized value를 리턴하기 위해서 사용. rendering이 일어나는 동안 실행된다!
    
    ```jsx
    const memoizedValue = useMemo (
    	() => {
    		// 연산량이 높은 작업을 수행하여 결과를
    		return computeExpensiveValue (의존성 변수1, 의존성 변수2);	
    	},
    	[의존성 변수1, 의존성 변수2]  // 의존성 배열을 넣지 않을 경우 렌더링 할 때마다 함수가 실행됨
    															// 의존성 배열이 빈 배열이면 mount 될떄만 creat가 실행
    );
    ```
    
    - memoization : 최적화를 위해 사용함. 연산량이 많이 드는 함수의 호출 결과를 저장해 뒀다가 같은 입력 값으로 함수를 호출할 때, 새로 함수를 호출하지 않아도 이전 호출 결과를 반환 받음.
- useCallback() : useMemo()와 비슷하지만, Callback은 값이 아닌 함수를 반환한다. 불필요한 재렌더링을 방지할 수 있다.
    
    ```jsx
     const memoizedCallback = useCallback (
    	() => {
    		doSomething(의존성 변수1, 의존성 변수2);
    	},
    	[의존성 변수1, 의존성 변수2]
    );
    ```
    
- useRef() : Reference(특정 component에 접근할 수 있는 객체)를 사용하기 위한 hook이다. useRef()는 이 레퍼런스를 반환한다.
    - 변수명.current(현재 reference 하고 있는 element)를 활용하여 접근한다.
    - 내부의 데이터가 변경되었을 때 별도로 알려주지 않는다. current 속성이 바뀌어도 재런더링이  발생하지 않음.
    
    ```jsx
    const refContainer = useRef(초깃값);  // 언마운트 전까지 계속 유지된다. 
    ```
    

## 배열 Component

- 배열 component를 활용해서 다양한 자료형을 저장할 수 있다.
    
    ```jsx
    const number = [1,2,3,4];   
    const mix = [1,'sebin',function sebin() {}];   
    const componentList = [<MyComponent />, <b>Hi</b>];  //jsx와 xml도 저장이 가능하다
    ```
    
- map() 함수를 사용해서 배열로 저장된 데이터를 jsx로 변경할 수 있다.
    
    ```jsx
    const todoList = [
    	{taskName : "공부하기", finished : false},
    	{taskName : "방 치우기", finished: false},
    	{taskName : "밥먹기", finished : true},
    ];
    const todos = todoList.map(todo => <div>{todo.taskName}</div>);
    // 결과적으로 [<div>공부하기</div>,<div>방 치우기</div>,<div>밥먹기</div>]를 반환한다.
    ```