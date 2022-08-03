# Handling Event

### Handling Event 의 뜻

- event : 사건
- handling evnet : 사건을 처리하는 것

## DOM과 React에서 Event의 차이점

### DOM의 Event

```jsx
<button onclick = "activate()">
	Activate
</button>
```

- button이 onclick되면 activate함수를 호출한다

### React의 Event

```jsx
<button onClick = {activate}>
	Activate
</button>
```

- 카멜 표기법을 사용한다
- React에서는 함수 그대로 전달한다

>> 표기법과 함수를 전달하는 방식이 다르다

### Event Handler (Event Listener)

- 어떤 사건이 발생하면, 사건을 처리하는 역할

### 예제 - class component

```jsx
class Toggle extends React.Component {
	constructor(props) {
		super(props);

		this.state = { isToggleOn: true };
		
		this.handleClick = this.handleClick.bind(this);
		//bind를 사용한 부분
	}

	handleClick() {
		this.setState(prevState => ({
			isToggleOn: !prevState.isToggleOn
		}));
	}
	//handleClick을 정의하는 부분	
원
	render() {
		return (
			<button onClick={this.handleClick}>
				{this.state.isToggleOn ? "켜짐" : "꺼짐"}
			</button>
		);
	}
}
```

- isToggleOn이라는 Boolean 변수를 통해 현재 상태를 True로 지정해줌
- handleClick이 Event Handler
- this는 해당하는 컴포넌트 클래스를 가리키는데 handleClick함수에서는 this가 무엇도 가리키지 않는다. 이를 해결하기 위해 bind를 사용한다.

### Bind

- bind를 사용해 this.handleClick에 대입해준다
- bind안하면 global scope에서 호출됨 —> undefined됨

```jsx
render() {
		return (
			<button onClick={() => this.handleClick()}>
        {/* 원래는 <button onClick={this.handleClick}>이였다 */}
				{this.state.isToggleOn ? "켜짐" : "꺼짐"}
			</button>
		);
	}
```

- render안에서 Arrow function으로도 bind사용이 가능하다

### 예제 - function component

```jsx
function Toggle(props) {
	const [isToggleOn, setIsToggleOn] = useState(true);

	function handleClick() {
		setIsToggleOn((isToggleOn) => !isToggleOn);
	}

	return (
		<button onClick={handleClick}
			{isToggleOn ? "켜짐" : "꺼짐"}
		</button>
	);
}
```

- 함수 안에 함수를 넣으면 bind 된다
- event handler는 this에 넣지않고 바로 onclick에 넣으면 된다

### Event Handler에 Arguments 전달하기

- Arguments : Event Handler(함수)에 전달(주장)할 데이터
- Parameter : 매개변수, Arguments와 비슷
    - 특정 사용자의 id를 매개변수로 전달해서 정해진 작업을 수행하는 등의 역할을 가짐

### 예제

```jsx
function MyButton(props) {
  const handleDelete = (id, event) => {
    console.log(id, event.target);
  };

  return (
    <button onClick={(event) => this.handleDelete(1, event)}>
      삭제하기
    </button>
  );
}
```

- 첫번째 매개변수로는 id, 두번째 매개변수로는 event를 받는다.
- 화살표 함수를 사용하면 bind를 사용하지 않아도 된다.

# Conditional Rendering

- 어떠한 조건에 따라서 렌더링이 달라지는 것
    
    =조건부 렌더링(Conditional Rendering)
    

### 예제

```jsx
function Greeting(props) {
	const isLoggedIn = props.isLoggedIn;

	if (isLoggedIn) {
		return <UserGreeting />
	}
	return <GuestGreeting />;
}
```

- props로 들어오는 isLoggin에 값에 따라서 true면 UserGreeting함수를 돌려주는 Greeting함수

### truthy와 falsy

- JS에서 true와 false로 처리되는 것들이다

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9ea39c70-d190-4afb-9ac2-f08c9af9ebde/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220729T014239Z&X-Amz-Expires=86400&X-Amz-Signature=7a449cebc416cd9613fa04ad00419f1272f94b07e6acd00cc5b6bae7c7c28ad5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### Element Variables

- React Element를 변수처럼 다루는 방법

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/df5508ad-f58c-4246-a954-8163b5deb9a9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220729T014224Z&X-Amz-Expires=86400&X-Amz-Signature=4a1615517513e3b7951ddead10687bb0230b28a1535bea5a164355e7827f898e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/948097bf-0327-46cb-8f13-75beb2576eb3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220729T014208Z&X-Amz-Expires=86400&X-Amz-Signature=e39da2be3cca12bd694b80d37c399dffeac75122247c8f3382585736aed0cf2d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- button을 변수로 만들었다

### Inline Conditions

- 조건문을 코드 안(in line)에 집어넣는 것

### Inline If

- &&연산자로 if문을 대체한다
    - true조건문 && true조건문 일때만 true를 반환한다
    
    ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/649d7899-ce4a-49c1-937b-0c60b7beed6e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220729T014152Z&X-Amz-Expires=86400&X-Amz-Signature=f97169501e49bb51581790906844f9d23f1222e90df18b7601588c4c8c202b93&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
    
    - unreadMessages.length가 0보다 크면 true임으로 뒤의 것도 실행시킨다
    - 만약 false라면 뒤의 것은 읽지도 않고 바로 false처리한다
    
    ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c47a88a9-9188-438f-9646-50a05c50a964/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220729T014140Z&X-Amz-Expires=86400&X-Amz-Signature=d8d1c8794b0764ae73b36b2162c186436d295073e5c94b168c0e0d0ea7d3a93e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
    
    - 단 count의 값인 0은 그대로 들어가 출력이 된다

### Inline If-Else

- 조건문에 값에 따라서 다른 element를 보여줄 때 사용
- 삼항 연산자를 사용

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f148164b-3e55-48c7-b378-b63a3eebf628/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220729T014120Z&X-Amz-Expires=86400&X-Amz-Signature=d75b1abc27300d7de122bfba511877009b69a6da40e0a831435d5026022cf298&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- true일 경우 logout버튼을 출력한다

### Component 렌더링 막기

- null을 리턴하면 렌더링하지 않는다

# List and Keys

### List

- 같은 아이템을 순서대로 모아놓은 것

### Array

- List를 위해 사용하는 자료구조
- JS의 변수나 객체들을 하나의 변수로 묶어 놓은 것

### Key

- key는 각 객체나 아이템을 구분할 수 있는 고유한 값을 의미
- List에 있는 아이템들을 구분하기 위한 고유한 문자열
    - 같은 List에 있는 Elements 사이에서만 고유한 값이면 된다
- key 값으로는 id나 index(고유한 id가 없을 경우) 등이 사용된다
- ex) 민증번호, 학번 등

### 여러 개의 Component 렌더링 하기

- 배열과 key를 이용해 반복되는 다수의 element를 렌더링 할 수 있다

### map

- 한쪽에 있는 아이템과 다른 쪽의 아이템을 짝 지어줌

```jsx
const doubled = numbers.map((number) => number *2);
```

- number의 배열에 있는 각 숫자에 2를 곱한 값이 들어간 doubled라는 배열을 생성하는 코드
- 배열의 첫번째 아이템부터 어떠한 아이템을 연산한 뒤 배열을 만들어 돌려준다

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d27ca097-1950-4735-ab68-d0e3cd09874f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220729T014103Z&X-Amz-Expires=86400&X-Amz-Signature=3feed777f091627ff70aa292007ad67bce319de7e996f98836a78d70f1a0e373&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- number의 숫자를 하나씩 li태그 안에 넣어 listItems로 돌려준다

```jsx
...
 map((number, index) =>
<li key={index}>
	{number}
<li>
```

- map 함수 안에 있는 elements는 꼭 key가 필요하다
- 결과값

![배고파요](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0311cd5a-005d-4b6a-b8b2-c5a359f01747/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220729%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220729T014015Z&X-Amz-Expires=86400&X-Amz-Signature=d8bcf7aac2e845b17c89c1d9172f9e16a66e6e4ab0e458b28d579322dcbf4b23&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)