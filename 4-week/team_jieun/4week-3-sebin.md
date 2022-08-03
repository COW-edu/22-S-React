# 4week-3

## Lifting State up

- **Shared State**란 ? 하나의 데이터를 여러개의 component에서 사용해야 할 때가 있다. 이 때 공통된 데이터를 가장 가까운 부모 component state에서 저장해두고, 하위 coponent에서는 이 state를 공유해서 사용하는 경우이다.

## Composition vs Inheritance

- composition이란 ? 여러개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것이다. 많이 사용하는 방식이기 때문에 잘 알고 있는 것이 중요하다.
    - 아래 사진을 보면 A컴포넌트와 B컴포넌트를 composition하여 하나의 페이지 컴포넌트를 만든 것을 볼 수 있다.
    
    ![화면 캡처 2022-07-28 235820.png](4week-3%20e9db38b13c4e4435bf11068dd9b5b4a7/%25ED%2599%2594%25EB%25A9%25B4_%25EC%25BA%25A1%25EC%25B2%2598_2022-07-28_235820.png)
    
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

## Context

- 데이터를 props로 전달하는 기존의 방식 대신, 컴포넌트 트리를 통해 곧바로 컴포넌트로 데이터를 전달하는 것이다.
    
    
    - 기존 방식
    
    ![화면 캡처 2022-07-29 004136.png](4week-3%20e9db38b13c4e4435bf11068dd9b5b4a7/%25ED%2599%2594%25EB%25A9%25B4_%25EC%25BA%25A1%25EC%25B2%2598_2022-07-29_004136.png)
    
    - context를 활용한 방식
    
    ![화면 캡처 2022-07-29 004200.png](4week-3%20e9db38b13c4e4435bf11068dd9b5b4a7/%25ED%2599%2594%25EB%25A9%25B4_%25EC%25BA%25A1%25EC%25B2%2598_2022-07-29_004200.png)
    
    - 기존 방식을 사용할 경우 props를 하위 컴포넌트로 계속 전달해야 한다는 단점이 존재한다. 하지만 context를 사용하면 데이터가 필요한 부분에 바로 전달이 가능해진다. 코드도 깔끔해지고 데이터를 한곳에서 관리하기 때문에 디버깅 하기에도 유리하다고 한다.
- 로그인 여부, 로그인 정보, 현재 언어, ui테마 등 **여러개의 component들이 접근해야 하는 데이터가 존재할 때 사용된다** .
    
    ```jsx
    //아래는 데이터를 기존 방식으로 전달하고 있다.
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
    
    //아래는 context를 활용하여 props에 접근한다.
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
    //중간에 위치한 컴포넌트는 하위 컴포넌트로 데이터를 전달할 필요가 없어졌다.
    
    function ThemedButton(props) {
    	return ( 
    		<ThemeContext.Consumer>   
    			{value => <Button theme={value}/>}
    		</ThemeContext.Consumer>
    	);    
    }      
    //가장 가까운 상위 Provider를 찾아서 해당되는 값을 사용한다.
    //provider이 없을 경우 기본값을 사용한다. (여기서는 light이 기본값)
    ```
    
- 무조건 context를 사용하는 것은 옳지 않다. component와 context가 연동되면 재사용성이 떨어지기 때문이다.
- Context API
    - **const 변수명 = React.createContext(’기본값’)**를 사용하여 context 객체를 생성한다.
    - 렌더링이 일어날 때 상위 레벨의 **Provider**로 부터 context 값을 받아오게 된다. (매칭되는 상위 컴포넌트가 없다면 기본값을 리턴한다. )
    - **변수명.Consumer**는 context의 data를 받아오는 코드이다.