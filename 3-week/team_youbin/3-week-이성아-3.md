- React의 장점과 단점
    - 장점
        - Virtual DOM = 빠른 업데이트 & 렌더링 속도
        - Component-Based = 재사용성(Reusablity)
            
             > 개발 기간 단축, 유지 보수 용이
            
        - 든든한 지원군
        - 활발한 지식공유 & 커뮤니티
    - 단점
        - 방대한 학습량
        - 지속적인 변화
        - 높은 상태관리 복잡도


- JSX
    - JSX란?
        - A syntax extension to Java Script = Java Script + XML/HTML
    - JSX 코드
        
        ```jsx
        1 const element = <h1>Hello, world!</h1>;
        ```
        
    - JSX의 역할
        
        ```jsx
        React.createElement(
        	type,
        	[props],
        	[...children]
        )
        ```
        
    - JSX를 사용한 코드
        
        ```jsx
        class Hello extends React.Component {
        	render() {
        		return <div>Hello {this.props.toWhat}</div>;
        	}
        }
        
        ReactDOM.render(
        	<Hello toWhat="World" />,
        	document.getElementById('root')
        );
        ```
        
    - JSX를 사용하지 않은 코드
        
        ```jsx
        class Hello extends React.Component {
        	render() {
        		return React.createElement('div', null, 'Hello ${this.props.toWhat}');
        	}
        }
        
        ReactDOM.render(
        	React.createElement(Hello, { toWhat : 'World' }, null),
        	document.getElementById('root')
        );
        ```
        
    - JSX의 장점
        - 코드가 간결해진다.
        - 가독성이 향상된다.
        - Injection Attacks 방어
    - JSX 사용법
        
        > …XML/HTML {Java Script Code } …XML/HTML
        > 
        - Tag의 속성(Attribute)에 값을 넣는 방법