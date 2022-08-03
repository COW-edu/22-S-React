7/27일 학습내용 정리

# Handling Events

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

# Conditional Rendering

⇒ 조건부 렌더링 → 어떠한 조건에 따라 렌더링이 달라지는 것

### js의 truthy와 falsy

truthy : true는 아니나 true로 여겨지는 값

falsy : false는 아니지만 false로 여겨지는 값

### Element Variables

조건부렌더링을 사용중 렌더링 해야 할 컴포넌트를 변수처럼 사용해야될때 쓰임

### inline Conditions

조건문을 코드안에 집어넣는것

1.inline if

→ if문을 필요한곳에 직접 집어넣어 사용하는 방법

but 실제 if문 사용하는것이 아니라 &&논리연산자를 사용함.

인라인 이프는 &&연산자를 jxs코드안에서 중괄로를 사용하여 직접집어넣는 방법임

ex)

```jsx
function Mailbox(props) {
		const unreadMEssages =  props.unreadMessages;

		return (
				<div>
						<h1>안녕</h1>
						{unreadMessages.length > 0 &&
								<h2>
										현재 {unreadMessages.length}개의 읽지 않은 메세지가 있습니다.
								</h2>
						}
				</div>
		);
}
```

다음은 unreadMessages.length의 길이가 0보다 큰지 작은지의 결과값에 따라 뒤의 코드가 렌더링 유무가 달라진다. 

= 0보다 크면 렌더링된

= 0보다 작으면 렌더링 안됨

유의) &&연산자를 사용할때 조건문의 false Expression을 사용하면 뒤에나오는 Expression은 평가 되지않으나  false Exprestiond값이 그대로 리턴되므로 주의해야함.

2.inline if-Else

if-else문을 필요한곳에 직접넣어 사용하는 방법

inline if와 다르게 조건문에 값에따라 다른 엘리먼트를 보여줄때 사용

→ 삼항연산자인 ? 를 사용한다.

(삼항연산자)

조건 ? A:B

조건이 참이면 A, 거짓이면B 반환

ex)

```jsx
function UserStatus(props) {
		return (
				<div>
						이 사용자는 현재 <b>{props.isLoggedIn ? '로그인' : '로그인X'}</b> 상태입니다.
				</div>
		);
}
```

위 코드의 경우

props.isLoggedIn의 값이 true인 경우 로그인을 false인 경우 로그인X를 반환함

→ 삼항연산자에 문자열말고 엘리먼트를 넣어 사용할수도 있음

### Component 렌더링 막기

return값으로 null을 입력하면 렌더링 되지 않음

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

### Key

각 객체나 아이템을 구분할 수 있는 고유한 값

리액트에서는 아이템들을 구분하기위한 고유의 문자열

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