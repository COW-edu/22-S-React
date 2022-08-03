- **State(상태)**
    
    : React Component의 변경 가능한 데이터이며 개발자가 정의한다.
    
    - Rendering이나 데이터 흐름에 사용 되는 값만 State에 포함시켜야한다.
        
        ```jsx
        class LikeButton extends React.Component {
        	constructor(props) {
        		super(props);
        
        		this.state = {
        			liked: false
        			};
        		}
        
        		...
        }
        ```
        
    - React State는 Java Script 객체이며 직접 수정 할 수 없다.
        
        ```jsx
        ///state를 직접 수정 (잘못된 사용법)
        this.state = {
        	name: 'Inje'
        };
        
        /// setState 함수를 통한 수정 (정상적인 사용법)
        this.setState({
        	name: 'Inje'
        });
        ```
        
- **Lifecycle(생명주기)**
    - Component가 생성되는 시점과 사라지는 시점이 정해져 있다.
    - Mounting(출생) - Updating(인생) - Unmounting(사망)
    1. Mounting(출생)
        
        : 컴포넌트가 생성되는 시점
        
        Component의 constructor, 즉 생성자가 실행
        
        생성자에서는 Component의 State를 정의
        
        Component가 Rendering되며 
        
        이후 componentDidMount 함수 호출
        
    2. Updating(인생)
        
        : Component의 props가 변경되거나
        
        setState 함수 호출에 의해 State가 변경되거나
        
        forceUpdate라는 강제 업데이트 함수 호출로 인해 Component가 다시 Rendering
        
        이후 componentDidUpdate 함수 호출
        
    3. Unmounting(사망)
        
        상위 컴포넌트에서 현재 컴포넌트를 더 이상 화면에 표시하지 않을 때
        
        직전에 componentWillUnmount 함수 호출
        
    
     > Component가 계속 존재하는 것이 아니라, 시간의 흐름에 따라 생성되고 업데이트 되다가 사라진다.

     - Hooks & useState, useEffect, useMemo, useCallback, useRef
    
    ![스크린샷 2022-07-28 오후 3.54.48.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/446b736c-2a4e-468e-9fa1-513ed65c209a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-07-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.54.48.png)
    
    - Hooks
        
        : React의 State와 Lifecycle 기능에 갈고리를 걸어 원하는 시점에 정해진 함수를 실행되도록 만든 것이며 이때 실행되는 함수를 Hook이라고 부른다.
        
    - 대표적인 Hooks
        - useState()
            
            : State를 사용하기 위함 Hook
            
            ```jsx
            const [변수명, set함수명] = useState(초기값);
            ```
            
            ```jsx
            import React, { useState } from "react";
            
            function Counter(props) {
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
            
            ```jsx
            import React, { useState } from "react";
            
            function Counter(props) {
            	const [count, setCount] = useState(0);
            
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
            
        - useEffect()
            
            : Side effect를 수행하기 위한 Hook
            
            React에서의 Side effect는 효과, 영향을 의미하며
            
            > 다른 Component에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없기 때문에 Rendering이 끝난 이후에 실행되어야한다.
            > 
            > 
            > 따라서 React의 함수 Component에서 Side effect를 실행할 수 있게 해주는 Hook
            > 
            
            ```jsx
            useEffect(이펙트 함수, 의존성 배열);
            ```
            
            ```jsx
            useEffect(이펙트 함수, []);
            ```
            
            ```jsx
            useEffect(이펙트 함수);
            ```
            
            ```jsx
            import React, { useState } from "react";
            
            function Counter(props) {
            	const [count, setCount] = useState(0);
            
            	// componentDidMount, componentDidUpdate와 비슷하게 작동
            	useEffect() =>. {
            		// 브라우저 API를 사용해 document의 title를 업데이트
            		document.title = 'You clicked ${count} times';
            	});
            
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
            
            ```jsx
            function UserStatusWithCounter(props) {
            	const [count, setCount] = useState(0);
            	useEffect(() => {
            		document.title = `총 $(count)번 클릭했습니다.`;
            	});
            
            	const [isOnline, setIsOnline] = useState(null);
            	useEffect(() => {
            		ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
            		return () => {
            			ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
            		};
            });
            
            function handleStatusChange(status) {
            	setIsOnline(status.isOnline);
            }
            
            // ...
            ```
            
            ```jsx
            useEffect(() => {
            	// 컴포넌트가 Unmount된 이후,
            	// 의존성 배열에 있는 변수들 중 하나라도 값이 변경되었을 때 실행
            	// 의존성 배열에 빈 배열([])을 넣으면 마운트와 Unmount 시에 단 한 번씩만 실행
            	// 의존성 배열 생략 시 Component 업데이트 시마다 실행
            	...
            
            	return () => {
            		// 컴포넌트가 마운트 해제되기 전에 실행
            		...
            	}
            }, [의존성 변수1, 의존성 변수2, ...]);
            ```
            
        - useMemo()
            
            : Memoized value를 Return하는 Hook
            
            ```jsx
            const memoizedValue = useMemo(
            	() => {
            		// 연산량이 높은 작업을 수행하여 결과를 반환
            		return computeExpensiveValue(의존성 변수1, 의존성 변수2);
            	},
            	[의존성 변수1, 의존성 변수2]
            );
            ```
            
            ```jsx
            const memoizedValue = useMemo(
            	() => computeExpensiveValue(a,b)
            );
            ```
            
            ```jsx
            const memoizedValue = useMemo(
            	() => {
            		return computeExpensiveValue(a,b);
            	},
            	[]
            );
            ```
            
        - useCallback()
            
            : useMemo() Hook과 유사하지만 값이 아닌 함수를 반환
            
            ```jsx
            const memoizedCallback = useCallback(
            	() => {
            		doSomething(의존성 변수1, 의존성 변수2);
            	},
            	[의존성 변수1, 의존성 변수2]
            );
            ```
            
            ### 동일한 역할을 하는 두 줄의 코드
            
            ```jsx
            useCallback(함수, 의존성 배열);
            
            useMemo(() => 함수, 의존성 배열);
            ```
            
            ```jsx
            import { useState } from "react";
            
            function ParentComponent(props) {
            	const [count, setCount] = useState(0);
            
            	// 재Rendering될 때마다 매번 함수가 재정의
            	const handleClick = (event) => {
            		// 클릭 이벤트 처리
            	};
            
            	return (
            		<div>
            			<button
            				onClick={() => {
            					setCount(count + 1);
            				}}
            			>
            				{count}
            			</button>
            	
            			<ChildComponent handleClick={handleClick} />
            		<div>
            	);
            }
            
            ```
            
            ```jsx
            import { useState, useCallback } from "react";
            
            function ParentComponent(props) {
            	const [count, setCount] = useState(0);
            
            	// 재Rendering될 때만 매번 함수가 재정의
            	const handleClick = useCallback((event) => {
            		// 클릭 이벤트 처리
            	}; []);
            
            	return (
            		<div>
            			<button
            				onClick={() => {
            					setCount(count + 1);
            				}}
            			>
            				{count}
            			</button>
            	
            			<ChildComponent handleClick={handleClick} />
            		<div>
            	);
            }
            
            ```
            
        - useRef()
            
            : Reference(특정 컴포넌트에 접근할 수 있는 객체)를 사용하기 위한 Hook
            
            > refObject.current : 현재 참조하고 있는 Element
            > 
            
            ```jsx
            const refContainer = useRef(초깃값);
            ```
            
            ```jsx
            function TextInputWithFocusButton(props) {
            	const inputElem = useRef(null);
            
            	const onButtonClick = () => {
            		// `current`는 Mount된 input element를 가리킨다.
            		inputElem.current.focus()
            	};
            
            	return (
            		<>
            			<input ref={inputElem} type="text" />
            			<button onClick={onButtonClick}>
            				Focus the input
            			</button>
            		</>
            	);
            }
            ```
            
           > useRef() Hook은 내부의 데이터가 변경되었을 때 별도로 알리지 않는다.
          > 

          ```jsx
          function MeasureExample(props) {
            const [height, setHeight] = useState(0);

            const measuredRef = useCallback(node => {
              if (node !== null) {
                setHeight(node.getBoundingClientReact().height);
              }
            }, []);

            return (
              <>
                <h1 ref={measureRef}>안녕, 리액트</h1>
                <h2>위 헤더의 높이는 {Math.round(height)}px 입니다.</h2>
              </>
            );
          }
          ```

          - Hook의 규칙과 Custom Hook 만들기
              - Hook의 규칙
                  1. Hook은 무조건 최상위 레벨에서만 호출해야한다.
                      
                      Hook은 Component가 Rendering될 때마다 매 번 같은 순서로 호출되어야 한다.
                      
                  2. React 함수 Component에서만 Hook을 호출해야 한다.
                      
                      > eslint-plugin-react-hooks 참고
                      > 
              - Custom Hook 만들기
                  - Custom Hook은 이름이 use로 시작하고 내부에서 다른 Hook을 호출하는
                      
                      하나의 자바스크립트 함수
                      
                  - 여러 개의 Component에서 하나의 을 사용할 때,
                      
                      Component 내부에 있는 모든 state와 effects는 분리되어 있다.
                      
                  - 각 Custom Hook 호출에 대해 분리된 state를 얻게 되고
                      
                      Custom Hook 호출 또한 완전히 독립적이다.