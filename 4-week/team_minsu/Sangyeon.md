## DOM vs React

```jsx
//DOM의 Event
<button onclick="activate()">
		Activate
</button>

//React의 Event
<button onClick={activate}>
		Activate
</button>
```

차이점

1. React는 onclink이 카멜 표기법으로 되어있다.
2. DOM에서는 이벤트 처리할 함수를 문자로 전달하지만 
    
    React에서는 함수 그대로 전달함.
    

### 다양한 표기법들

1. 카멜(Camel)케이스
    - 첫 번째 단어는 소문자, 그 다음 단어부터는 대문자를 사용
        
        ex) getMathScore
        
2. 파스칼(Pascal) 케이스
    - 카멜 표기법과 달리, 첫 번째 단어부터 대문자
        
        ex) CommonUtil
        
3. 스네이크(snake) 케이스
    - 단어 사이에 언더바를 넣음
        
        ex) total_number
        
    

## Event Handdler

사건이 발생하면 사건을 처리하는 역할

### 사용법

```jsx
function Toggle(props) {
	const [isToggleOn, setIsTogglenOn] = useState(true);

// 방법 1. 함수 안에 함수로 정의
	function handleClick() {
		setIsToggleOn((isToggleOn) => !(sToggleOn);
	}

	const handleClick = () => {
		setIsToggleOn((isToggleOn) => !(isToggleOn);
	}

	return(
		<button onClick={handleClick}
				{isToggleOn ? "켜짐" : "꺼짐"
		</button>
	);
}
```

### 삼항 연산자

```html
조건식 ? 반환값1 : 반환값2
```

조건식이 true면 반환값 1을 반환, 조건식이 false면 반환값 2을 반환