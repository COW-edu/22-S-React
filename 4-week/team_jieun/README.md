# Team 지은

Team 지은 mentor 도진
4주차 RBF
세빈,가현,현진,은택

# <Hooks>

앞서 컴포넌트에는 두가지 종류가 있었다.

함수 컴포넌트와 클래스 컴포넌트인데 클래스 컴포넌트와 같이 생성자에서 state를 정의하고 setState함수를 활용하여 state 수정 및 라이프싸이클 메소드를 사용할 수 있는것과 달리 

함수 컴포넌트에서는 state를 사용하지 못하고 라이프싸이클에 따른 기능 구현이 불가능하다.

그래서 이러한 점을 보완하기 위해 Hooks이라는 개념이 생겼다.

훅은 갈고리라는 뜻을 가진것에 걸맞게 state관련 함수 , 라이프싸이클 관련 함수 , 최적화함수 등을

갈고리 처럼 함수컴포넌트에 걸어 원하는 시점에 정해진 함수들이 실행되도록 한다.

→ 이때 실행되는 함수들이 hooks (훅을 사용할땐 앞에 use를 붙인다.)

(요약)

- 리액트 버전 16.8에서 새롭게 등장한 개념! 최근에 리액트로 개발할 때 대부분이 훅을 사용한다.
- 함수 컴포넌트 → state 사용 불가, Lifecycle에 따른 기능 구현 불가

 - 프로그래밍에서 hook : 원래 존재하는 특정 기능에 갈고리를 거는 것 처럼 끼어들어가 같이 수행되는 것을 의미한다.

- class형 component는 state와 생명주기 함수를 활용하여 사용한다. 그러나 함수형 컴포넌트는 state와 생명주기 함수를 사용할 수 없다. → 함수형 컴포넌트는 단순한 구조의 UI 컴포넌트를 제작할 때 많이 사용된다고 한다.
- Hooks을 사용하면 함수형 컴포넌트도 state와 생명주기 함수와 동일한 기능을 구현할 수 있다.

## 대표적인 hooks

- useState() : state를 사용하기 위한 hook.
    
    ```jsx
    const [변수명, set함수명] = useState(초기값);
    // class의 setState()와는 달리, 변수 각각에 set함수가 따로 존재한다.  
    ```
    

useState의 사용 법은 변수에 초기값을 설정해준 후 set 함수가 실행되었을 때 변수가 바뀐다고 생각

이때 set함수는 파라미터를 받아서 그 파라미터로 state값이 바뀌게되고 컴포넌트는 정상적으로 리렌더링된다

**ㅡㅡㅡㅡstate 변수를 선언하고 초기화해주면 react는 해당 state 변수를 리렌더링할 때 기억하고 가장 최근에 갱신된 값을 제공한다 ㅡㅡㅡㅡㅡ**

대신 리렌더링할때 내부 변수는 초기화된다

참고자료

[https://react.vlpt.us/basic/07-useState.html](https://react.vlpt.us/basic/07-useState.html) -나중에 볼 자료

[https://velog.io/@velopert/react-hooks](https://velog.io/@velopert/react-hooks)

[https://velog.io/@kwonh/ReactHook-useState-와-useEffect-로-상탯값과-생명주기-사용하기](https://velog.io/@kwonh/ReactHook-useState-%EC%99%80-useEffect-%EB%A1%9C-%EC%83%81%ED%83%AF%EA%B0%92%EA%B3%BC-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

- useEffect() : class 생명 주기 함수의 componentDidMount, componentDidUpdate, componentWillUnmount를 통합하여 제공한다고 생각하면 쉽다.
    
    ```jsx
    useEffect(이펙트 함수, 의존성 배열); 
    // mount와 unmount를 한 번만 실행하도록 하고싶다면 의존성 배열에 []를 입력한다. 
    // 의존성 배열을 생략할 경우 component가 업데이트 될 때마다 실행된다. 
    ```
    

1. useEffect (   (  )⇒{           }  );

  배열을 안 넣었던 건 처음 렌더링 될때와 업데이트로 인해 화면에 재렌더링 될때 실행한다.

1. useEffect (  () ⇒ {        }  ,[  ]);

 저 배열 안에 값을 넣으면 마운트 되었을떄와 의존성 배열 안에 있는 변수들 중 하나라도 값이           변경되었을 때 실행 만약 의존성 배열에 빈 배열을 넣으면 마운트와 언마운트시에 단 한번씩만 실행이 된다

.useEffect clean up 함수 (뒷정리하기)

컴포넌트가 필요없어서 해지될 때 사용하는 함수이다

사용법은 useEffect 함수안에 return () ⇒ {   이 안에 원하는 정리 작업들을 적으면 된다  } 

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
    
    ## useMemo 함수가 왜 필요할까?
    
    함수형 컴포넌트가 렌더링 된다는 것은 함수가 호출되면  모든 내부 변수들이 초기화되는 것을 의미한다. 상태가 변경되서 렌더링되거나 부모컴포넌트의 상태 변경으로 덩달아 렌더링 되는 경우도 있다. 이렇게 react에서 컴포넌트의 렌더링은 수시로 일어날 수 있다.
    
    ```jsx
    function Mycomponent({x,y)){
    state~~~~~~
    const z=compute(x,y);
    return <div>{z}</div>
    }
    ```
    
    현재 이 코드는 prop로 넘어온 x와 y를 compute 함수에 인자로 넘겨서 z값을 구한후 출력을 한다 만약에 compute 함수가 내부적으로 매우 복잡한 연산을 수행하기에 결과값을 리턴하는데 몇초 이상 오래걸린다면 어떻게 될까? 컴포넌트의 재렌더링이 필요할 때마다 이 함수가 호출되므로 사용자는 지속적으로 UI에서 지연이 발생되는 경험을 하게 된다. → 불필요하고 무의미한 호출로 인한 지연 ?!
    
    ## Memoization
    
    위와 같은 상황에서 유용한게 memoization이다. memoization이란 기존에 수행한 연산의 결과값을 어딘가에 저장해두고 동일한 입력이 들어오면 재활용하는 프로그래밍 기법을 말한다. 
    
    동일한 값을 리턴하는 함수를 반복저으로 호출해야한다면 맨 처음 계산할 떄 그 값을 메모리에 저장해두고 다음번에도 이런 상황이오면 다시 계산하는게아니라 그 메모리에서 가져와서 사용하는것
    
    → useMemo는 성능을 최적화할 수 있는 함수이다
    
    → but 무분별하게 사용하면 오히려 성능을 악화시킨다
    
    ## useMemo 구조
    
    useMemo는 두개의 인자를 받는데 하나는 콜백함수이고 하나는 의존성 배열이다
    
    배열안에 요소의 값이 업데이트 될때만 콜백함수를 다시 호출해서 메모이제이션된 값을 업데이트 해서 다시 memoization안에 넣어주는 것이다
    
     빈 배열을 넘겨주면 맨 처음 컴포넌트가 마운트 되었을 때만 값을 계산해서 memoization에 사용한다고 본다
    
    참고자료
    
    [https://www.daleseo.com/react-hooks-use-memo/](https://www.daleseo.com/react-hooks-use-memo/)
    
- useMemo() Hook과 유사하지만 **값이 아닌 함수를 반환한다**. → 컴포넌트가 렌더링 될 때마다 매번 함수를 정의하는 것이 아니라 의존성 변수값이 바뀐 경우에만 함수를 새로 정의해 return 해준다.
- 컴포넌트가 마운트 될 때만 함수가 정의된다.
    
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
    
    useRef 사용 용도
    
    첫번째로 변수관리 용도가 있는데 이를 위해서는 state와 ref의 차이점을 알아야한다
    
    **state와 ref의 차이점**
    
    state는 컴포넌트 내부에서 변화하는 값을 관리해주기 위해 사용되는데 즉 state의 변화는 업데이트로 인지되서 렌더링이 된다. element는 렌더링이 될때 업데이트 해주는 게 아니라 실제 새로운 element를 생성해서 렌더링해주는 것이기에 컴포넌트 내부 변수들을 초기화 시켜준다
    
    하지만 ref는 값이 변화가 일어나도 렌더링이 일어나지않는다 즉 ref 값의 변화로는 렌더링이 일어나지않기에 ref값만 변화했다면 화면에ref 값이 변한것이 업데이트 되지 않는다. 또한 ref는 state 변화로 인해 업데이트 (렌더링)되었다해서 ref 값은 초기화 되지 않고 유지된다
    
    **ref와 변수의 차이점**
    
    변수와 ref모두 값이 달라진다해도 화면에 업데이트가 일어나지않는다 . 둘다 값은 변하고 있지만 렌더링 되지 않기에 화면에 표시되지 않는다 . 하지만 둘의 차이점은 업데이트로 인해 렌더링이 되었을 때 컴포넌트로 엘리먼트를 다시 생성해야해서 컴포넌트 내부 변수들이 초기화될때 변수는 초기화되지만 ref는 값이 그대로 유지된다
    
    ex) 화면에 렌더링 될때마다 렌더링이 몇번 되었는 지 알고싶다 
    
    그럼 보통 useEffect 함수를 사용하려 할 것이다 
    
    ```jsx
    import React {useState,useRef,useEffect) from "rect";
    const App = () =>{
    const [count , setCount]= useState(1);
    const renderCount = useRef(1);
    //const [render , setRender ] =useState(1);
    
    useEffect(()=>{
    renderCount.current= renderCount.current+1;
    console.log('렌더링 수:' , renderCount.current);
    
    //setRender(render+1);
    });
    }
    
    return(
    <p>count :{count}</p>
    <button onClick={()=>setCount(count+1)}>올려</button>
    );
    
    export default App;
    ```
    
    위 코드는 state가 변할 때 마다 렌더링 되고 그 수를 ref 통해 출력을 하도록 되어있다
    
    보통 주석에 처리해놓은 것처럼 render라는 state를 만들고 ref가 아닌 state로 렌더링 된 수 값을 관리해주려고 할 수 있다 . 하지만 그렇게 코드를 만들면 state render값이 변경되었단 사실만으로 또 렌더링이 되어 useEffect함수가 실행된다 즉 무한루프에 빠지게 된다
    
    변수관리 용도는 위 코드에서 잘 나타낸다
    
    두번째로는 DOM 요소에 접근할 때이다
    
     
    
    리액트를 사용하는 프로젝트에서도 가끔씩 DOM을 직접 선택해야하는 상황이 발생할 때도 있다 예를 들어 스크롤바 위치를 가져오거나 포커스를 설정하는 상황이 있다
    
    이때 react에서는 ref를 사용한다
    
    useRef()를 사용하여 ref 객체를 만들고 이 객체를 우리가 선택하고 싶은 DOM에 ref값으로 설정해주어야 한다. 그러면 ref 객체의 .current 값은 우리가 원하는 DOM을 가르킨다.
    
    이 ref의 object를 우리가 접근하고자하는 요소 태그에 ref속성으로 넣어주기만 하면 해당 요소에 접근이 가능한 것이다!
    
    마치 바닐라 js의 queryBySelector 역할을 한다
    
    ```jsx
    import React, {useEffect,useRef} from "react";
    
    const App=() =>{
    const inputRef = useRef();
    
    useEffect(()=>{
    inputRef.current.focus();
    //지금 현재 inputRef.current가 input태그를
    //가르킨다고 생각 
    },[]);
    
    return (
    <div>
    <input ref={inputRef} type="text" placeholder="username"/>
    </div>
    );
    
    };
    ```
    
    이 코드는 화면이 처음 렌더링 되었을 때 바로 포커스가 보이게하는 코드이다 로그인 버튼을 누르지 않아도 자동으로 focus 되도록
    
    ## Hook의 규칙
    
    - Hook은 무조건 **최상위 레벨**에서만 호출해야 한다.
        - 최상위 레벨: 리액트 함수 컴포넌트의 최상위 레벨을 의미 → 반복문, 조건문, 중첩된 함수 안에서 Hook을 호출하면 안된다.
        - Hook은 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 한다. ~> 다수의 useEffect, useState hook에서 컴포넌트의 state를 올바르게 관리할 수 있게 된다.
        - 잘못된 Hook 사용법
        
        ```jsx
        function MyComponent(props){
        	const [name, setName] = useState('hyunjin');
        
        	if (name !== ''){
        		useEffect(() => {
        			...
        		});
        	}
        	...
        }
        ```
        
        → 조건문의 결과에 따라 호출되는 Hook이 달라지기 때문
        
    
    - **리액트 함수 컴포넌트에서만** Hook을 호출해야 한다.
    
    ### Custom Hook 만들기
    
    - 여러 컴포넌트에서 반복적으로 사용되는 로직을 hook으로 만들어 재사용하기 위함!
    - 이름이 use로 시작하고 내부에서 다른 hook을 호출하는 하나의 **자바스크립트 함수**
    - 특별한 규칙이 없음 → 단순한 함수와 같은데 이름 앞에 use를 붙임으로써 단순한 함수가 아닌, 리액트의 hook이라는 것을 나타내 준다.
    
    <aside>
    💡 Custom Hook의 이름은 꼭 use로 시작해야 한다!
    
    </aside>
    
    - 여러 개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있다.
        - 어떻게 state를 분리할까?
            
            리액트 컴포넌트는 각각의 Custom Hook 호출에 대해서 분리된 state를 얻게 되기 때문에
            
    - 각 Custom Hook의 호출은 완전히 독립적이다.
    
    # <Handling Event>
    
    ## Events
    
    = 사건
    
    ex) 유저가 버튼을 클릭했을때 → 버튼클릭 이벤트라 부르며 하나의 이벤트라 한다.
    
    이러한 이벤트를 처리하려할때 이벤트를 handling한다라고 부름
    
    리액트에서 이벤트 처리방법을 알기전
    
    Dom의 이벤트 처리방식을 알아보자
    
    ```jsx
    <button onclick = "activate()"> Activate</button>
    ```
    
    위는 버튼클릭시 activate()라는 함수를 호출하는 예제 코드이다.
    
    돔에서는 클릭이벤트를 처리할 함수를 돔 클릭을 통해서 전달한다.
    
    리액트 에서는?
    
    ```jsx
    <button onClick = {activate}> Activate </button>
    ```
    
    위 코드는 처음코드와 동일한 기능을 하는 리액트 코드이다.
    
    (차이점)
    
    1.처음코드와 비교하여 onClick에 c가 대문자인것 → 카멜표기법
    
    2.돔에서는 이벤트를 처리할 함수를 문자열로 전달 but 리액트에서는 함수 그대로 전달.
    
    ### 
    
    ## Event Handler
    
    위와 같이 이벤트가 발생했을때 해당 이벤트를 처리하는 함수를 이벤트 핸들러라고 부른다.
    
    (이벤트 리스너라고 부르기도 한다.)
    
    ### Arguments 전달하기
    
    → Arguments = 함수(이벤트 핸들러)에 전달할 데이터
    
    비슷하게 매개변수도 많이 사용함
    
    parameter = 매개변수
    
    ```jsx
    <button onClick = {(activate) => this.deleteItem(id, event)}> 삭제하기 </button>
    <button onClick = {this.deleteItem.bind(this, id)}> 삭제하기 </button>
    ```
    
    위 코드는 차례대로 에로우펑션을 바인드를 사용한 동일한 기능을 하는 코드이다.
    
    이벤트라는 매개변수는 리액트의 이벤트 객체를 의미함
    
    두번째 코드의 경우 event매개변수가 id이후에 두번째 매개변수로 전달됨.
    
    함수컴포넌트에서 이벤트핸들러에게 매개변수 전달할때는
    
    ```jsx
    function MyButton(props) {
    		const handleDelete =  (id, event) => {
    				console.log(id, event.target);
    		};
    
    		return (
    				<button onClick={(event) => handleDelete(1, event)}>
    						삭제하기
    				</button>
    		);
    }
    ```
    
    다음과 같이 사용한다.
    
    ## Conditional Rendering
    
    - Conditional rendering이란, 어떠한 조건에 따라서 렌더링이 달라지는 것을 의미한다. (조건문이라고 생각하면 쉽다.) 결과에 (True 혹은 False) 따라서 렌더링을 다르게 하는 것이다.
    - 자바 스크립트에서는 truthy와 falsey가 존재한다. 개발자를 혼동 시키지만, 잘 활용하면 유용하게 사용할 수 있다고 한다. (나도 혼동중)
        - truthy는 true는 아니지만 true로 여겨지는 값이다. (ex. true, {} , [], “0” (string), “false”(not empty))
        - falsy는 false는 아니지만 false로 여겨지는 값이다. (ex. false, 0, 0n, ‘’ , null, undefined, NaN)
    - 조건부 렌더리을 사용하다보면, 렌더링해야 하는 컴포넌트를 객체처럼 다루고 싶을 때가 존재하는데  그럴 때는 **Element Variable**를 활용한다. 리액트의 element를 변수처럼 다루는 것이다.
        
        ```jsx
        //로그인 , 로그아웃 컴포넌트이다.
        function LoginButton(props) {
        	return (
        		<button onClick = {props.onClick}>
        			로그인
        		</button>
        	);
        }
        function LogoutButton(props) {
        	return (
        		<button onClick = {props.onClick}>
        			로그아웃
        		</button>
        	);
        }  
        
        // 아래 코드는 로그인, 로그아웃 컴포넌트를 변수처럼 사용한 예시이다. 
        
        function LoginControl(props) {
        	const [isLoggedIn,setIsLoggedIn]= useState(false);
        	
        	const handleLoginClick = () => {
        		setIsLoggedIn(true);
        	}
        	const handleLogoutClick = () => {
        		setIsLoggedIn(false);
        	}
        
        	let button;
        	if(isLoggedIn) {
        		button = <**LogoutButton** onClick = {handleLogoutClick}/>;
        	} else {
        		button = <LoginButton onClick = {handleLoginClick}/>;
        	}
        
        	return (
        		<div>
        			<Greeting isLoggedIn={isLoggedIn}/>
        			{button}
        		</div>
        	)
        }
        ```
        
    - Component를 렌더링 하고 싶지 않을 때는 .. ? → null을 리턴 값으로 받으면 된다.
    
    ## Inline Conditions
    
    - : in  + line 이라는 말에 맞게 코드를 라인 안에 작성하는 것을 의미한다. 조건문을 코드 안에 집어 넣는 것인데 Inline if, Inline if-else가 존재한다.
    - **Inline if**는 if문을 필요한 곳에 작성하는 형태인데, 실제 if문을 사용하는 것이 아니라 비슷한 효과를 주기 위해 $$ 연산자를 사용한다.
        - 조건문 && expression → 앞에 조건문이 true일 경우에는 뒤에 expression이 실행된다. 반면에 조건문이 false 경우에는 전체 결과가 false가 됨으로 뒤에 표현식을 볼 필요가 없다. 하지만 ! 결과값을 리턴 받는다 !!
        
        ```jsx
        function Mailbox(props) {
        	const unreadMessages = props.unreadMessages;
        
        	return (
        		<div>
        			<h1>hi</h1>
        			{unreadMessages.length > 0 &&    
        				<h2>
        					현재 {unreadMessages.length}개의 읽지 않은 message가 존재합니다.
        				</h2>
        			}
        		</div>
        	);
        }   //unreadMessages의 길이가 1 이상이면 뒤에 표현식이 렌더링되고 
        		// unreadMessages 0이라면 표현식이 렌더링이 되지 않는다.  
            // 결과적으로 현재 0개의 읽지 않는 message가 존재한다고 알려줄 것이다.
        ```
        
    - **Inline if-else**는 조건문의 값에 따라서 다른 element를 보여줄 때 사용한다.’ ? ‘ 연산자를 사용하여 나타낸다.
        - 삼항 연산자를 사용한다. condition ? true : false 형태이다.
        
        ```jsx
        function UserStatus(props) {
        	return (
        		<div>
        			이 사용자는 현재 <b>{props.isLoggedIn ? :'로그인' :'로그인 하지 않은'}</b>상태임.
        		</div>
        	)
        }
        
        ```
        
        # List and Keys
        
        ## List
        
        =목록
        
        리스트를 위해 사용하는 자료구조 = Arraty
        
        ### Array
        
        js의 변수나 객체들을 하나의 변수로 묶어 놓은 것
        
        ```jsx
        const numbers = [1, 2, 3, 4, 5];
        ```
        
        다음은 js의 배열이다.
        
        리액트에서는 이 배열을 사용해서 리스트형태로 엘리먼트를 렌더링 할 수 있음
        
        ## List의 Key
        
        - key는 고유한 형태가 들어가야 한다. (ex. 주민등록번호, id, 이메일 등등)
        - key로 값을 사용할 경우
            
            ```jsx
            const numbers =[1,2,3,4,5]
            const listItems = numbers.map((number) => 
            	<li key ={number.toString()}>  // numbers 배열에 중복된 값이 있으면 오류가 생긴다.
            		{number}                     // key 값은 고유해야 하기 때문이다.
            	</li>
            );
            ```
            
        - key로 id를 사용하는 경우
            
            ```jsx
            const todoItmes = todo.map((todo)=>
            	<li key={todo.id}>         //id의 의미 자체가 고유한 값을 나타내기 때문에 매우 적합하다.
            		{todo.text}
            	</li>
            );
            ```
            
        - **map() 함수 안에 있는 Element는 꼭 key가 필요하다 !**
        
        ## 여러 개의 Component 렌더링 하기
        
        웹에서 같은 컴포넌트를 여러개 나열해서 나타내야할때 
        
        컴포넌트를 여러번 사용하는것은 비효율적이다. → 같은 코드의 반복
        
        이러한 상황에 js의 함수인 map()함수를 사용한다.
        
        ### map()
        
        배열에 들어있는 각 변수에 처리를 한 후 리턴하는것
        
        ```jsx
        const double = numbers.map((number) => number * 2);
        ```
        
        위 함수는 numbers 배열에 들어있는 각각의 숫자에 2를 곱한 값이 들어간 double이라는 배열을 생성하는 코드
        
        map함수는 배열에 첫번째 요소부터 끝까지 어떤 연산처리후 최종결과를 배열로 만들어서 리턴한다.
        
        리액트에서 map함수
        
        ```jsx
        const number = [1, 2, 3, 4, 5];
        const listItems = numbers.map((number) =>
        		<li>{number}</li>
        );
        
        ReactDom.render(
        		<ul>{listItems}</ul>
        		document.getElementById('root')
        );
        ```
        
        위 코드는 number의 배열값을 li태그로 감싸 반환하여 listItems배열을 만들고있다. → 총 5개의 엘리먼트가 생김
        
        그 후 화면에 렌더링 하기위해 리액트돔에 렌더함수를 사용함 → li태그로 감싸진 listItems배열을 ul태그로 다시감싸 반환함
        
        - 1
        - 2
        - 3
        - 4
        - 5
        
        최종 다음과 같은 형태로 출력됨
        
        ## Form
        
        - 사용자로부터 입력을 받기 위해 사용하는 양식이다. (ex. 회원가입을 하거나, 로그인을 할 때 사용하는 입력창을 생각하면 쉽다.)
        - HTML의 폼보다 리액트를 활용하여 form을 작성하면 사용자가 입력한 값에 접근하고 제어하는 것이 편리하다.
        
        리액트, html 각각의 form은 차이가 있다?
        
        리액트는 컴포넌트 내부에서 state를 통해 데이터리관리
        
        반면
        
        html 폼은 엘리먼트 내부에 각각의 state가 존재
        
        html폼
        
        ```jsx
        <form>
        		<label>
        				이름:
        				<input type="text" name="name" />
        		</label>
        		<button type="submit">제출</button>
        </form>
        ```
        
        이름을 입력받고 제출하는 간단한 폼
        
        → js코드를 통해 사용자가 입력한 값에 접근하기에 불편한 구조
        
        접근 제어를 쉽게 하기위해
        
        controlled Components를 알아야함
        
        - HTML은 각각의 form들이 자체적으로 state를 관리한다는 불편함이 있다. 자바스크립트를 통해 각각의 form 값들에 접근하기가 쉽지 않다.
        - 리액트는 controlled component를 활용하여 모든 데이터를 state에서 관리한다.
        - Controlled Component : 이름 그대로 값이 누군가(리액트)에게 통제를 받는 Input Form Element이다. 그렇기 때문에 사용자의 입력을 직접적으로 제어가 가능하다.
            
            ```jsx
            function NameForm(props) {
            	const [value,setValue] = useState('');
            
            	const handleChange = (event)=> {    
            	setValue(event.target.value);    //제어 컴포넌트를 통해 직접적으로 제어한다. 
            	}                                //setValue를 사용하여 값을 업데이트 하는 것이다.
            ...
            	return (
            		<form>
            		<label>
            		이름 : 
            			<input type="text" value={value} onChange={handleChange}> 
            		</label>   //콜백함수형태로 이벤트를 처리한다. 
            		</form>
            	)
            }
            ```
            
        - Form 의 종류
            - textarea : 여러 줄에 걸쳐 긴 텍스트를 입력받기 위해 사용하는 html 태그이다. value 속성을 사용한다.
            
            ```jsx
            //위의 예시에서 input만 textarea로 바꾼 형태이다. 
            <textarea value={value} onChange={handleChange}> 
            ```
            
            - selsect : drop-down 목록을 보여주기 위해 사용하는 html 태그이다.
            
            ```jsx
            <select>
            	<option value ="apple">사과</option>
            	<option selected value="grape">포도</option>  // 포도가 선택된 상태로 출력된다.
            </select>
            
            //다른 예시이다.
            const [value, setValue] = setState('grape'); //초기값으로 grape를 넣어준다.
            ...
            <select value={value}>
            	<option value ="apple">사과</option>
            	<option value="grape">포도</option>
            </select>
            ```
            
            - file input : 서버로 파일을 업로드 하거나 파일을 다룰 때 사용한다. (Uncontrolled Component이다. 읽기 전용이기 때문에 값이 리액트의 통제를 받지 않는다. )
            
            ```jsx
            <input type="file"/>;     
            ```
            
        - 만약 하나의 component에서 여러개의 form을 다루고 싶다면 ? → **여러개의 state를 선언하여 각각의 입력에 대해 사용하면 된다.**
        - value props을 정해진 값으로 넣으면 코드를 수정하지 않는한 입력값을 바꿀 수 없다. value props은 넣되, **자유롭게 값을 변경하고 싶다면 값에 undefined 혹은 null을 넣어주면 된다.**
            
            ```jsx
            ReactDOM.render(<input value="hi" />, rootNode); 
            // input의 값이 hi로 정해져있다. 
            setTimeout(function() {
            	ReactDOM.render(<input value={null}/>,rootNode);
            },1000);  // 1 초뒤에 value가 null로 바뀌면서 입력이 자유로워진 형태가 된다. 
            
            ```
            
            # 여러 종류의 폼
            
            ## Textarea 태그
            
            = 여러줄에 걸쳐 긴 텍스트를 입력받기위한 HTML태그
            
            ```jsx
            <textarea>
            		안녕하세요 응용22 최은택입니다.
            </textarea>
            ```
            
            Html에서는 다음과 같이 텍스트를 태그로 감싸는 형태로 사용하나
            
            리액트에서는 textarea라는 태그에 value라는 어트리뷰트를 사용하여 텍스트를 표시함
            
            → controlled Components방식 →값을 컴포넌트의 state를 사용해서 다룰수 있음
            
            ## Select태그
            
            = Drop-down 목록을 보여주기 위한 HTML태그
            
            html에서는 다음과같이
            
            ```jsx
            <select>
            		<option value="apple">사과</option>
            		<option value="banana">바나나</option>
            		<option selected value="grape">포도</option>
            		<option value="melon">멜론</option>
            </select>
            ```
            
            옵션태그를 셀렉트태그가 감싸는형태로 사용
            
            현재 선택된 옵션은 selected 라는 어트리뷰트를 갖고있음
            
            리액트에서는 옵션태그에 selected 속성을 사용하지않음
            
            대신 selected의 value라는 어트리뷰트를 사용해서 값을 표시
            
            ```jsx
            <select multiple={true} value{['a' , 'a+']}>
            ```
            
            다음과 같이 true를 쓰고 배열을 넣어 여러개 옵션을 선택할 수 있다.
            
            # File input태그
            
            =디바이스의 저장 장치로부터 하나또는 여러 개의 파일을 선택할 수 있게 해주는 HTML태그이다.
            
            사용방법
            
            ```jsx
            <input type="file" />
            ```
            
            input타입을 파일로
            
            file input태그는 값이 읽기전용 → 리액트에서는 Uncontrolled Component가 됨
            
            하나의 컴포넌트에서 여러개의 입력을 받으려면?
            
            → 여러개의 state선언후 각각의 입력에 대해 사용하면 됨
            
            # Lifting State Up
            
            ## shared state
            
            =공유된 state
            
            자식 컴포넌트들이 가장 가까운 공통된 부모컴포넌트의 state를 공유하여 사용하는것
            
            = state에 있는 데이터를 여러개의 하위 컴포넌트에서 공통적으로 사용하는 경우
            
            중복된 값을 사용할 필요가없다 → 응용가능
            
            ## Composition vs Inheritance
            
            - composition이란 ? 여러개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것이다. 많이 사용하는 방식이기 때문에 잘 알고 있는 것이 중요하다.
                - 아래 사진을 보면 A컴포넌트와 B컴포넌트를 composition하여 하나의 페이지 컴포넌트를 만든 것을 볼 수 있다.
                
                ![화면 캡처 2022-07-28 235820.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0914632b-86ac-4f96-9284-da35eeff44e2/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-07-28_235820.png)
                
            - containment : 하위 컴포넌트를 포함하는 형태의 합성 방법이다. sidebar나 dialog 같은 box형태의 컴포넌트는 자신의 하위 컴포넌트를 미리 알 수없다. → 이 때 containment를 사용한다.
                - props에 존재하는 children 속성을 사용하면 된다.  이 속성은 개발자가 값을 직접 집어넣는 것이 아닌 리액트에서 기본적으로 제공해주는 기능이다. 하위 컴포넌트를 props.children으로 모아서 제공해준다.
                
                ```jsx
                function FancyBoreder(props) {
                	return (
                		<div className={'FancyBorder FancyBorder-' + props.color}>
                			{props.children}  
                	</div>
                	);
                }                         //children prop을 사용하여 컴포넌트를 하나 생성했다. 
                
                function WelcomDialog(props) {
                	return(
                		<FancyBoreder color="blue">    //결과적으로 파란색으로 감싸져서 출력된다.
                			<h1> 어서 오세요             // FancyBoreder안에 존재하는 <h1>태그는 
                				...                       //FancyBoreder의 childern props로 전달된다.
                			</h1>        **
                		</FancyBoreder>
                	
                	);
                }
                ```
                
                - 여러 개의 children 집합이 필요한 경우에는 ? 별도로 props를 정의하여 각각 원하는 component를 넣어주면된다.
            - Specialization : 넓게 사용되는 개념을 구별이 가능하게 구체화 하는 것이다. ( + 객체지향 언어에서는 상속을 사용하여 specialization을 구현하지만, 리액트에서는 composition을 사용하여 specialization을 구현한다. )
                
                ```jsx
                //Dialog 컴포넌트를 활용하여 welcomdialog 컴포넌트를 Specialization 한 것이다.
                function Dialog(props) { 
                	return (
                		<FancyBorder color = "blue">
                			<h1 className="Dialog-title">   // '-' 이 표현의 의미는 Dialog 뒤에 title이 
                				{props.title}                 // 붙는다는 것을 나타내주는 것 같다.
                			</h1>                           // <h1>,<p> 태그는 FancyBoreder의 childern props로 전달된다.
                			<p className="Dialog-message">
                				{props.message}
                			</p>
                		</FancyBorder>
                	);
                }
                
                function WelcomDialog(props) {
                	return (
                		<Dialog 
                			title="어서 오세요"    //title의 제목에 따라 용이하게 사용이 가능하다. 
                			message="세빈이의 사이트에 방문하신 것을 환영합니다 !"
                		/>
                	);
                }
                ```
                
                - containment와 Specialization를 같이 사용하고 싶다면 .. ? → containment를 사용하기 위해 children 속성을 사용하고 + Specialization를 사용하기 위해 직접 정의한 props를 사용하면 된다!
            - Inheritance란 ?  다른 컴포넌트로부터 상속을 받아 새로운 컴포넌트를 만드는 방법 (추천할 만한 사례를 찾지 못했다고 한다. 결론적으로 권장하지 않는 것 같다. )
            
            ## 결론
            
            <aside>
            💡 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 만들고, 만든 컴포넌트들을 조합해서 새로운 컴포넌트를 만들자!
            
            </aside>
            
            ## Context
            
            - 데이터를 props로 전달하는 기존의 방식 대신, 컴포넌트 트리를 통해 곧바로 컴포넌트로 데이터를 전달하는 것이다.
                
                
                - 기존 방식
                
                ![화면 캡처 2022-07-29 004136.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/57ea4052-541a-4ab1-8cd6-550392820857/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-07-29_004136.png)
                
                - context를 활용한 방식
                
                ![화면 캡처 2022-07-29 004200.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0cfe55e6-42e6-49a2-90b0-78bf999bb89a/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-07-29_004200.png)

                - 기존 방식을 사용할 경우 props를 하위 컴포넌트로 계속 전달해야 한다는 단점이 존재한다. 하지만 context를 사용하면 데이터가 필요한 부분에 바로 전달이 가능해진다. 코드도 깔끔해지고 데이터를 한곳에서 관리하기 때문에 디버깅 하기에도 유리하다고 한다.
            - 로그인 여부, 로그인 정보, 현재 언어, ui테마 등 **여러개의 component들이 접근해야 하는 데이터가 존재할 때 사용된다** .
                
                ```jsx
                // 아래는 데이터를 기존 방식으로 전달하고 있다.
                // 비효율적으로 데이터를 가져온다는 것을 알 수 있다.
                function App(props) {
                	return <Toolbar theme="dark"/>; 
                }
                 // Toolbar 컴포넌트에 theme 데이터를 넘겨주고 있다.
                function Toolbar(props){
                	return (
                		<div>
                			<ThemedButton theme={props.theme} /> 
                		</div>                  
                	);
                }
                // ThemeButton 컴포넌트에 theme을 또 다시 넘겨주고 있다
                function ThemedButton(props) {
                	return <Button theme={props.theme} />;  
                }
                
                // 아래는 context를 활용하여 props에 접근한다.
                const ThemeContext = React.creatContext('light');   // ThemeContext 생성
                
                function App(props) {
                	return(
                		<ThemeContext.Provider value='dark'>  
                			<Toolbar />
                		</ThemeContext.Provider>
                	);  
                }    
                // Provider을 사용하여 하위 컴포넌트들에게 현재 데이터를 전달한다.
                // 트리의 깊이에 상관 없이 모든 하위 컴포넌트가 데이터를 읽을 수 있다. 
                
                function Toolbar(props){
                	return (
                		<div>
                			<ThemedButton/> 
                		</div>               
                	);
                }
                // 중간에 위치한 컴포넌트는 하위 컴포넌트로 데이터를 전달할 필요가 없어졌다.
                
                function ThemedButton(props) {
                	return ( 
                		<ThemeContext.Consumer>   
                			{value => <Button theme={value}/>}
                		</ThemeContext.Consumer>
                	);    
                }      
                // 가장 가까운 상위 Provider를 찾아서 해당되는 값을 사용한다.
                // provider이 없을 경우 기본값을 사용한다. (여기서는 light이 기본값)
                ```
                
                Context를 사용할때 주의할 점
                
                Context다른 레벨의 많은 컴포넌트들이 특정 데이터를 필요로 하는경우에 사용
                
                그러나 무분별한 context사용은 지양해야함
                
                → 컴포넌트와 context가 연동되면 재사용성이 떨어지기때문
                
                →컴포넌트가 많지않다면 기존의 컴포넌트 컴포지션방법이 효율적이다.
                
                Context는 어떻게 사용해야할까?
                
                ### Comtext API
                
                Context를 사용하려면 당연히 Context를 생성해야한다.
                
                → Context를 생성하기위해 React.createContext()함수를 사용한다.
                
                Context생성
                
                ```jsx
                const MyContext = React.createContext(기본값);
                ```
                
                이후 Context객체가 생성됨
                
                →리액트에서 렌더링이 일어날때 Context객체를 구독하는 하위컴포넌트가 나오면 현재 Context값을 가장 가까이에있는 상위레벨의 PROVIDE로 부터 받오게됨
                
                →상위레벨의 PROVIDE가 없으면 기본값이 적용
                
                이제 하위컴포넌트들이 해당 Context데이터를 받을 수 있도록 설정해야함
                
                → Comtext.Provider 사용
                
                =데이터를 제공해주는 컴포넌트
                
                Provider 사용
                
                ```jsx
                <MyContext.Provider = value = {/* some value*/}>
                ```
                
                Provider 컴포넌트에는 value라는 prop이 존재, 이것은 Provider 컴포넌트 하위에 있는 컴포넌트들에게 전달됨.
                
                하위컴포넌트들이 이 값을 사용할때 이들이 데이터를 소비한다는 뜻으로 컨슈밍 컴포넌트라고 부른다.
                
                컨슈밍 컴포넌트는 context값의 변화를 지켜보다 값이 변경되면 재렌더링 됨
                
                (하나의 Provider 컴포넌트는 여러개의 컨슈밍 컴포넌트와 연결될 수 있으며
                
                여러개의 Provider 컴포넌트는 중첩되어 사용될 수 있음.)
                
                ### Provider value에서 주의해야 할 사항
                
                context는 재렌더링 여부를 확인할때 레퍼런스 정보를 사용한다.
                
                provider의 부모 컴포넌트가 재렌더링되었을 경우 컨슈머 컴포넌트가 재렌더링 될 수 있다.
                
                이를 방지하기위해
                
                →value를 직접 넣지않고 컴포넌트의state로 옮기고 해당 state의 값을 넣어준다.
                
                ### Class.contextType
                
                =provide 하위에 있는 클래스컴포넌트에서 context의 데이터에 접근하기위해 사용한다.
                
                컨슈머 컴포넌트는 context의 데이터를 구독하는 방식
                
                함수컴포넌트에서는 Context.Consumer를 사용하면됨
                
                ### Context.Consumer
                
                사용방법
                
                ```jsx
                <MyContext.Consumer>
                		{value => /*컨텍스트의 값에 따라서 컴포넌트들을 렌더링 */}
                </MyContext.Consumer>
                ```
                
                컴포넌트의 자식으로 함수가 올 수 있다.
                
                MyContext.Consumer로 감싸주면 자식으로 들어간 함수가 현재 컨텍스트의 value를 받아서 리액트 노드로 리턴하게됨 
                
                함수로 전달되는 value는 provide의 value prop과 동일
                
                상위컴포넌트의 provide가 없다면 vlaue파라미터는 React.createContext(기본값);의 기본값과 동일.
                
                ### function as a child
                
                컴포넌트의 자식으로 함수를 사용하는 방법
                
                ```jsx
                //children이라는 props를 직접 선언하는 방식
                <Profile children = { name => <p>이름: {name}</p>) />
                
                //propfile컴포넌트로 감싸서 children으로 만드는 방식
                <Profile>{name => <p>이름: {name}</p>}</Profile>
                ```
                
                리액트에서는 기본적으로 하위컴포넌트들을 칠드런이라는 prop으로 전달해주는데 칠드런으로 컴포넌트대신 함수를 사용하여 위 코드와 같이 사용가능
                
                ### Context.displayName
                
                컴포넌트 객체는 Context.displayName이라는 문자열속성을 가진다.
                
                또한 크롬의 리액트 개발자 도구에선 Context의 provider나 consumer를 표시할때 이 디스플레이 name을 함께 표시한다
                
                ### 여러개의 Context를 동시에 사용하려면 어떻게 해야할까?
                
                 Comtext.Provider를 중첩하여 사용하면됨
                
                함수컴포넌트에서 context사용하는 방법을 아는것이 더 중요함 (클래스컴포넌트를 잘 사용하지않음)
                
                ### useContext()
                
                함수컴포넌트에서 context를 사용하기 위해 useContext()훅을 사용하는것
                
                useContext() 훅은 함수컴포넌트에서 context를 쉽게 사용할 수 있게 해줌
                
                다음과 같이 사용
                
                ```jsx
                function MyComponent(orios) {
                		const value - useContext(MyContext);
                
                		return (
                		    ...
                		)
                }
                ```
                
                컨텍스트 객체를 인자로 받고 현재 컨텍스트값을 리턴함
                
                # Styling
                
                ### 레이아웃과 관련된 속성
                
                ### display
                
                1.display: none;
                
                -엘리먼트를 화면에서 숨기기위해 사용
                
                -script태그의 display속성 기본값은 display: none;
                
                2.display: block;
                
                -블록 단위로 엘리멘트를 배치
                
                -p, div, h1~h6태그의 display속성 기본값이 display: block;
                
                3.display: inline;
                
                -엘리멘트를 라인 안에 넣는 것
                
                -span태그의 display 속성 기본값이 display: inline;
                
                4.display: flex;
                
                -엘리먼트를 블록 레벨의 flex container로 표시
                
                -container이기 때문에 내부에 다른 엘리먼트들을 포함
                
                ### visibility
                
                1.visibility: visible;
                
                -엘리먼트를 화면에 보이게 하는 것
                
                1.visibility: hidden;
                
                -화면에서 안보이게 감추는 것
                
                -엘리먼트를 안 보이게만 하는 것이고 화면에서의 영역은 그대로 차지
                
                ### position
                
                1.static
                
                -기본값으로 엘리먼트를 원래의 순서대로 위치시킴
                
                2.fixed
                
                -엘리먼트를 브라우저 윈도우에 상대적으로 위치시킴
                
                3.relative
                
                -엘리먼트를 보통의 위치에 상대적으로 위치시킴
                
                4.absolute
                
                -엘리먼트를 절대 위치에 위치시킴
                
                ### flex-direction
                
                1.row
                
                -기본값이며 아이템을 행(row)을 따라 가로 순서대로 왼쪽부터 배치
                
                2.column
                
                -아이템을 열(column)을 따라 세로 순서대로 위쪽부터 배치
                
                3.row-reverse
                
                -아이템을 행(row)의 역(reverse) 방향으로 오른쪽부터 배치
                
                4.column-reverse
                
                -아이템을 열(rcolumnw)의 역(reverse) 방향으로 아래쪽부터 배치
                
                ### align-items
                
                이때 정렬시 크로스 엑시스를 기준으로 한다.
                
                1.stretch
                
                -기본값으로써 아이템을 늘려서(stretch) 컨테이너에 가득 채움.
                
                2.flex-start
                
                -크로스 엑시스의 시작 지점으로 아이템을 정렬
                
                3.center
                
                -크로스 엑시스의 중앙으로 아이템을 정렬
                
                4.flex-end
                
                -크로스 엑시스의 끝 지점으로 아이템을 정렬
                
                5.baseline
                
                -아이템을 baseline 기준으로 정렬
                
                ### justify-content
                
                어떻게 나란히 맞출것인가를 → 메인엑시스를 기준으로함
                
                1.flex-start
                
                -메인엑시스의 시작지점으로 아이템을 맞춤
                
                2.center
                
                -메인엑시스의 중앙으로 아이템을 맞춤
                
                3.flex-end
                
                -메인엑시스의 끝 지점으로 아이템을 맞춤
                
                4.spece-between
                
                -메인엑시스를 기준으로 첫 아이템은 시작 지점에 맞추고
                
                마지막 아이템은 끝 지점에 맞추며 중간에 있는
                
                아이템들 사이(between)의 간격(spece)이 일정하게 되도록 맞춤.
                
                5.spece-around
                
                -메인엑시스를 기준으로 각 아이템의 주변(around) 간격(spece)을 동일하게 맞춤
                
                ## 폰트와 관련된 속성
                
                -font-family
                
                -font-size
                
                -font-weight
                
                -font-style
                
                등등이 있다.
                
                ### font-family
                
                어떤 글꼴을 사용할것인지 정하는 속성
                
                일반적인 글꼴분류(generic font family)
                
                -serif
                
                각 글자의 모서리에 작은 테두리를 갖고있는 형태의 글꼴
                
                -sans-serif
                
                모서리에 테두리가 업싱 깔끔한 선을 가진 글꼴
                
                컴퓨터 모니터에서는 Serif보다 가독성이 좋음
                
                -monospace
                
                모든 글자가 같은 가로 길이를 가지는 글꼴. 코딩을 할 때 주로 사용
                
                -cursive
                
                사람이 쓴 손글씨 모양의 글꼴
                
                -fantasy
                
                장식이 들어간 형태의 글꼴
                
                ### font-size
                
                글꼴의 크기와 관련
                
                px -> 가장 기본적으로 사용되는 단위.
                 이것은 픽셀단위라는 고정값에 따라 정해지기 때문에
                 화면의 크기나 확대와 같이 사용자가 임의로 정의할 수 없는 고정된 값을 말한다.
                 이 단위는 가장 기본이 되는 단위이기 때문에 앞으로 설명할 다른 단위들의 기준이 된다.
                
                em -> em은 부모 요소를 기준으로 자식 요소의 크기를 정하는 것을 말한다.
                
                rem -> rem은 root em의 약자이다.
                  rem 시스템은 모바일 환경에서 접근성을 위해 글자 크기를 키우거나 그리드 시스템에 사용하기에 유용함.
                 
                vw(viewport width)
                 뷰포트의 세로 너비의 백분율을 의미 함.
                 뷰포트라 함은 브라우저, 즉 페이지 전체를 의미.
                 1vw라 함은 가로 너비의 백분의 1만큼의 너비.
                
                ### font-weight
                
                글꼴의 두께와 관련
                
                값으로는
                
                nomal
                
                bold
                
                등을 사용하며
                
                100~900까지 백단위의 숫자의 값을 사용할 수 있음
                
                ### font-style
                
                -nomal
                
                일반적인 글자의 형태를 의미
                
                -italic
                
                글자가 기울어진 형태로 나타남
                
                별도로 기울어진 형태의 글자들을 직접 디자인해서 만든 것
                
                -oblique
                
                글자가 비스듬한 형태로 나타남
                
                ## 기타 많이 쓰이는 속성들
                
                ### background-color
                
                배경색 지정을 위한 속성
                
                색상의 값을 넣어 사용
                
                ex)16진수 ff0000 , 투명도는 뒤에 추가로 두자리수 , rgb , 색상이름 등등
                
                ## Styled-components(중요)
                
                css문법을 그대로 사용하면서 결과물을 스타일링된 컴포넌트 형태로 만들어주는
                
                오픈소스 라이브러리이다.
                
                ### 기본사용법
                
                tagged template literal를 사용하여 구성요소의 스타일을 지정함
                
                template literal → js에서 제공하는 문법중 하나
                
                이것을 알기전 literal의 개념에 대하여 알아야 함
                
                literal → 소스코드의 고정된 값을 의미
                
                다시돌아와서
                
                template literal은 literal를 template 형태로 사용하는 js의 문법
                
                → untagged template literal과 tagged template literal로 나뉨
                
                tagged template literal를 사용하면 문자열과 expression을 펑션의 파라미터로 넣어 호출한결과를 받을수 있다.
                
                ## Styled-components 사용예시
                
                ```jsx
                import React from "react";
                import styled from "styled-components";
                
                const Wrapper = styled.div`
                		padding: 1em;
                		background: grey;
                ;
                ```
                
                .Styled-components는 tagged template literal를 사용하여 css속성이 적용된 리액트 컴포넌트를 
                
                만들어 준다.
                
                Styled-components를 사용하는 기본적인 방법은 백티스로 둘러싸인 물자열 부분에 css속성을 넣고
                
                태그함수 위치에는 위와 같이 styled.html태그형태로 사용함
                
                →해당 엘리먼트 태그에 css속성들이 적용된 형태에 리액트 컴포넌트가 만들어짐
                
                ### Styled-components의 props 사용하기
                
                조건이나 동적으로 변하는 값을 사용해서 스타일링 하기위해 props를 사용한다
                
                →리액트 컴포넌트의 props와 같은 개념으로 생각해도 됨
