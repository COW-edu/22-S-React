[React]
- Components and Props
    - React component
    
    ![Props = 재료, React component = 틀, React element = 붕어빵](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d990960-fffa-42ee-8cc1-029fe0b06b70/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-07-24_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.12.44.png)
    
    Props = 재료, React component = 틀, React element = 붕어빵
    
    - React props
        
        : component에 전달할 다양한 정보를 담고 있는 Java Script 객체
        
        - 특징
        
        > Read-Only
        > 
        
        즉, 읽을 수만 있으여 값을 변경할 수 없다!
        
         > New props를 component에 전달하여 New element 생성
        
        - 사용법
            - Using JSX
            
            ```jsx
            function App(props) {
            	return (
            		<Profile
            			name="소플"
            			introduction="안녕하세요, 소플입니다."
            			viewCount={1500} /* Java Script code */
            		/>
            	);
            }
            ```
            
            ```jsx
            {
            	name: "소플",
            	introduction: "안녕하세요, 소플입니다."
            	viewCount: 1500
            }
            ```
            
            ```jsx
            function App(props) {
            	return (
            		<Layout
            			width={2560}
            			height={1440}
            			header={
            				<Header title="소플의 블로그입니다." />
            			}
            			footer={
            				<Footer />
            			}
            		/>
            	);
            } 
            ```
            
            - 참고용 : No Using JSX
            
            ```jsx
            React.createElement(
            	Profile,
            	{
            		name: "소플",
            		introduction: "안녕하세요, 소플입니다.",
            		viewCount: 1500
            	},
            	null
            );
            ```
            
    - React components
        
        > All React components must act like pure functions with respect to their props.
        > 
        
        즉, 모든 React components는 props를 직접 바꿀 수 없고, 같은 props에 대해서는 항상 같은 결과를 보여줄 것!
        
        ![스크린샷 2022-07-25 오후 3.55.22.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/942ae2b7-6334-410b-86d4-fb3f895dd9c9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.55.22.png)
        
        - 종류
            1. Function Component
                
                ```jsx
                function Welcome(props) {
                	return <h1>안녕, {props.name}</h1>;
                }
                ```
                
            2. Class Component
                
                ```jsx
                class Welcome extends React.Component {
                	render() {
                		return <h1>안녕, {this.props.name}</h1>;
                	}
                }
                ```
                
        - Component Naming
            
            > 항상 대문자로 시작해야한다!
            > 
            
            ```jsx
            const element = <div />;
            ```
            
            ```jsx
            const element = <Welcome name="인제" />;
            ```
            
        - Component Rendering
            
            ```jsx
            const element = <div />;
            ```
            
            ```jsx
            const element = <Welcome name="인제" />;
            ```
            
            ```jsx
            function Welcome(props) {
            	return <h1>안녕, {props.name}</h1>;
            }
            
            const element = <Welcome name="인제" />;
            ReactDOM.render(
            	element,
            	document.getElementById('root')
            );
            ```
            
        - Component Synthesis
            
            > React에서는 Component 안에 또 다른 Component를 사용할 수 있기 때문에 복잡한 화면을 여러 개의 Component로 나눠서 구현 가능!
            > 
            
            ```jsx
            function Welcome(props) {
            	return <h1>Hello, {props.name}</h1>;
            }
            
            function App(props) {
            	return (
            		<div>
            			<Welcome name="Mike" />
            			<Welcome name="Steve" />
            			<Welcome name="Jane" />
            		</div>
            	)
            }
            
            ReactDOM.render(
            	<App />,
            	document.getElementById('root')
            )
            ```
            
            ![스크린샷 2022-07-25 오후 4.13.40.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/599c333f-3208-40b0-86c4-16394316d5f4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.13.40.png)
            
        - Component Extract
        
        : 재사용성이 올라가면서 개발 속도도 향상된다.
        
        ```jsx
        function Comment(props) {
        	return (
        		<div className="comment">
        			<div className="user-info">
        				<Avatar user={props.author} />
        				<div className="user-info-name">
        					{props.author.name}
        				</div>
        			</div>
        			
        			<div className="comment-test">
        				{props.text}
        			</div>
        		
        			<div className="comment-date">
        				{formatDate(props.date)}
        			</div>
        		</div>
        	);
        }
        
        ```
        
        ```jsx
        props = {
        	author: {
        		name: "소플",
        		avatarUrl: "https://..."
        		},
        		text: "댓글입니다."
        		date: Date.now(),
        }
        ```
        
        ```jsx
        function Avatar(props) {
        	return (
        		<img className="avatar"
        			src={props.user.avatarUrl}
        			alt={props.user.name}
        		/>
        	);
        }
        ```
        
        ```jsx
        function UserInfo(props) {
        	return (
        		<div className="user-info">
        			<Avatar user={props.user} />
        			<div className="user-info-name">
        				{props.user.name}
        			</div>
        		</div>
        	);
        }
        ```
        
        ```jsx
        function Comment(props) {
        	return (
        		<div className="comment">
        			<UserInfo user={props.author} />
        			<div className="comment-text">
        				{props.text}
        			</div>
        			<div className="comment-date">
        				{formatDate(props.date)}
        			</div>
        	);
        }
        ```
        
        재사용 가능한 Component를 많이 갖고 있을수록 개발 속도가 빨라진다!
[Java]
- 프로그래밍 패러다임 : 절차지향과 객체지향
    - 절차지향(Procedural Programming)
        
        문제를 해결하는 절차(명령)를 기술하는 방식의 프로그래밍
        
        - 폰 노이만 모델 컴퓨터 구조에 기반
        
        명령의 순차적인 실행
        
        메모리 위치를 의미하는 변수의 사용
        
        변수의 값을 바꾸는 배정문의 사용
        
        - 변수 x와 y를 사용하고, 배정문 ‘=’을 사용하며 위에서 아래로 순차적으로 실행한다.
        
        Ex) C, Pascal, Ada, Python 등
        
        ~ 장점 ~
        
        컴퓨터의 처리구조와 유사해 실행 속도가 빠르다.
        
        ~ 단점 ~
        
        유지 보수와 디버깅의 어려움이 있다.
        
        실행 순서가 정해져 있으므로 코드의 순서가 바뀌면 동일한 결과를 보장할 수 없다.
        
    - 객체지향(Object-oriented Programming)
        
        실제 세계를 모의 실험하기 위한 언어로 고안, 객체 개념을 기반으로 하는 프로그래밍
        
        - 프로그램 실행은 객체 사이의 상호작용에 의해 이루어진다.
        
        Ex) C++, Java, C#, Objective-C, Swift
        
        Python, Visual Basic 등
        
         # 객체(Object)
        
        속성과 관련된 행동(함수, 메소드)들의 모음으로 표현
        
         # 클래스(Class)
        
        클래스는 객체에 대한 타입 정의
        
        객체는 클래스의 한 실체(instance)
        
         > 계산과정
        
        객체들 사이의 상호작용(메소드 호출)
        
        - 객체지향의 3대 특성
            1. 캡슐화
                
                : 관련된 코드와 데이터가 묶여 정리된 것으로써 개발자가 만들었으며, 묶여있고 오류가 없어 사용이 편리하다. 데이터를 감추고 외부 세계와 상호작용하는 방법은 메소드를 통해서 가능한데, 라이브러리로 만들어 업그레이드하면 쉽게 바꿀 수 있다.
                
            2. 상속
                
                : 이미 작성된 클래스를 이어 받아서 새로운 클래스를 생성하는 기법으로 기존 코드를 재활용하는 것이다.
                
            3. 다형성
                
                : 하나의 이름(방법)으로 많은 상황에 대처하는 기법이다. 개념적으로 동일한 작업을 하는 함수들에 똑같은 이름을 부여할 수 있어 코드가 더 간단해지는 효과가 있다.
                
        
        ~ 장점 ~
        
        데이터에 대한 신뢰성과 코드의 재활용성이 높다.
        
        코딩이 절차지향보다 간편하다.
        
        디버깅과 업데이트가 쉽다.
        
        ~ 단점 ~
        
        처리속도가 절차지향보다 느리다.
        
        설계에 많은 시간이 소요된다.
        
    - 둘의 차이점
        
        > 객체지향의 반대는 절차지향이 아니고
        > 
        > 
        > 절차지향의 반대는 객체지향이 아니다!
        > 
        - 절차지향은 데이터 중심, 객체지향은 기능 중심

        Q-1. React component에서 Props, component, element의 각 요소들을 붕어빵에 비유한다면?

        Q-2. 절차지향은 000 중심, 객체지향은 00 중심이다. 00에 들어갈 알맞은 단어는?

