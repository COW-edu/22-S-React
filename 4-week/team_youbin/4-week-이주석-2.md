### Hooks

📌 Hook의 규칙
    1. 최상위에서만 Hook을 호출
        • React 함수(컴포넌트)의 최상위에서만 Hook을 호출 할 것.
        • 반복문, 조건문, 중첩된 함수등에서 호출 X

    2. React 함수에서만 Hook을 호출
        • Custom Hook에서는 호출 가능
        • 일반적인 Javascript 함수에서는 호출 X

    3. Hook을 만들때 앞에 use 붙히기
        • 그래야만 한눈에 보아도 Hook 규칙이 적용되는지를 파악할 수 있기 때문

    4. React는 Hook 호출되는 순서에 의존
        • 한 컴포넌트에서 여러개의 hook이 사용되는 경우
        • hook은 위에서부터 아래로 순서에 맞게 동작한다.

📌 Hook의 장점
    기본적으로 클래스형 컴포넌트보다 쉽고 직관적으로 같은 기능을 만들 수 있다.

    1. 함수형 컴포넌트로 코드 통일 가능
        • 이전에는 state유무로 있으면 클래스형 / 없으면 함수형으로 분리해서 작업했다고 함

    2. useEffect로 클래스형 Lifecycle에 흩어져 있는 로직 묶음
        • hook은 Lifecycle과 달리 여러번 선언가능 해 코드가 무엇을 하는지에 따라 hook별로 분기가 가능하다.

    3. Custom Hook을 이용해 손쉽게 로직 재사용 가능함
        • 클래스형 컴포넌트에서 로직을 재사용하기 위해 썼던 HOC나 render-props 같은 패턴이 가져오는 컴포넌트 트리의 불필요한 중첩을 없애줌

📌 Hook의 종류
    기본 Hooks
    🌱 useState (동적 상태 관리)
```js
    const [state, setState] = useState(initialState);

    // params : state의 기본값, 초기값.
    // return : [상태 값, 상태 설정 함수] 형태의 배열
```
    • setState(newState);
        상태 설정 함수 setState 함수는 state를 갱신할 때 사용한다.
        새 state값을 받아 컴포넌트 리렌더링을 큐에 등록하고, 다음 리렌더링 시 useState에서
        반환 받은 state 상태 값은 갱신된 최신 state 값을 가진다.

    • 하나의 useState는 하나의 state만 관리할 수 있다. 컴포넌트에서 관리할 상태가 여러 개라면 useState를 여러 번 사용할 수 있다.

    🌱 useEffect (side effect 수행 -mount/unmount/update)
```js
    useEffect(function, [...]);

    // params : 1.어떤 effect를 발생하는 함수  2.(조건부 effect) effect가 종속되어 있는 값의 배열
    // return(정리 effect) : 정리(clean-up) 함수
```
    • 기본 동작은 모든 렌더링을 완료한 후 effect를 발생하는 것으로, 의존성 중 하나가 변경된다면 effect는 항상 재생성된다.
    • 특정 값 업데이트 시에만 실행되는 조건부 effect에서는, 두번째 인자로 종속값 배열을 준다.
      마운트될 때만 실행하고 싶은 경우, 두번째 인자로 빈 배열을 넣어준다.
    • 컴포넌트 언마운트 전 혹은 업데이트 전 정리(clean-up)가 필요한 effect에서는, return에 정리(clean-up) 함수를 반환한다.

    🌱 useContext (컴포넌트를 중첩하지 않고도 전역 값 쉽게 정리)

    추가 Hooks
        🌱 useReducer (복잡한 컴포넌트들의 state를 관리 -분리)
        🌱 useCallback (특정 함수 재사용)
        🌱 useMemo (연산한 값 재사용)
        🌱 useRef (DOM선택, 컴포넌트 안에서 조회/수정할 수 있는 변수 관리)

### Handling Events
    🌱 클래스형 컴포넌트
        React에서는 DOM엘리먼트가 생성된 후 리스너를 추가하기 위해 addEventListener를 호출할 필요가 없다.
        대신, 엘리먼트가 처음 렌더링될때 리스너를 제공하면 된다.

    🌱 함수형 컴포넌트
        일단 함수형 컴포넌트의 상태값은 useState훅으로 관리되기 때문에 컴포넌트의 this로부터 자유롭다.
        또한 함수형 컴포넌트 자체와 함수형 컴포넌트 안에서 선언한 함수들 모두 전역 객체를 this로 가지기 때문에 애초에 this가 다 같다.
        그래서 이벤트 핸들러에 콜백 함수를 넘기는 상황에도 딱히 신경 쓸 필요가 없다.
            • const 키워드 + 함수 형태로 선언해야한다
            • 요소에 적용하기 위해 this가 따로 필요하지 않다

📌 HTML에서와의 차이점
    1) React 이벤트 이름은 소문자 대신 camelCase를 사용
    2) JSX에 문자열 대신 함수를 전달

    html에서는 아래와 같이 이벤트를 넣었다면,
```js
<button onClick="activateButton()">클릭하세요</button>
```
    React에서는 이벤트이름을 onClick, onSubmit 등과 같이 camelCase로 설정한다는 차이점이 있습니다. event handler는 JSX 표기인 { }를 사용하여 연결합니다.
```js
<button onClick={activateButton}>클릭하세요</button>
```

📌 Event Binding
    Event에서 가장 중요한 부분은 binding이다. binding이 중요한 이유는 binding을 따로 지정을 해주지 않으면 해당 method에서 호출하는 this가 해당 class 안에서의 event 값이 아닌 최상위인 windows에서 값을 가져올 수 있기 때문이다.

    Event Binding 방법
        🌱 Constructor에서 정의를 해주는 방법
        🌱 render function 안에서 정의를 해주는 방법
        🌱 ES6 화살표 함수를 사용하는 방법
        🌱 autobind-decorator 라이브러리를 사용하는 방법

    Event Binding를 하지 않아도 되는 경우
        🌱 this를 이용하여 해당 class 참조를 하지 않아도 되는 경우
        🌱 React.creatClass()를 사용하는 경우
        🌱 화살표 함수를 사용하는 경우
        🌱 같은 method를 여러 번 호출할 경우 생성자에서 한 번만 binding을 해서 중복 binding 행동을 줄이는 경우

### Quiz
    Q1. Hook의 장점 4가지는?
    Q2. 이벤트 핸들링과 HTML의 차이점 2가지는?

### List & Key
    목록을 View에서 표현할 때 주로 List를 사용합니다.

📌 자바스크립트 map()
```js
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled)  //[2, 4, 6, 8, 10]
```
    map과 forEach는 거의 동일한데 다른 점은 같은 길이의 배열이 나오냐 안나오냐의 차이가 있다.
    위 예제를 예로 들면 doubled변수에 map함수는 동일한 길이의 결과값을 return해주지만 forEach를 사용했다면 doubled는 undefined이다.
    그런 이유로 map과 forEach의 용도가 다릅니다.

📌 여러 컴포넌트 렌더링
    JSX에서는 중괄호를 사용하여 Element를 구축할 수 있다.
```js
const names =  ['kendrick', 'christoper', 'theo', 'dave'];
const listItem = names.map((name) =>
    <li>{name}</li>
);
```
    위에서 만든 listItem을 render에 넘겨준다.
    li는 ul아래 있어야 한다.

📌 기본 리스트 컴퍼넌트
    예제로 사용하는 Name 리스트를 컴퍼넌트로 묶어주는 작업을 해보자.
```js
function NameList(props){
    const names = props.names;
    const listItem = names.map((name) =>
        <li>{name}</li>
    );
    return (
        <ul>{listItem}</ul>
    );
};
 
const names = ['kendrick', 'christopher', 'theo', 'dave'];
ReactDOM.render(
    <NameList names={names} />,
    document.getElementById('root')
);
```
    컴포넌트화 시켜서 렌더링 시키면 warning이 뜬다.

    react.js:3639 Warning: Each child in an array or iterator should have a unique "key" prop. Check the render method of `NameList`. See https://fb.me/react-warning-keys for more information.
        in li (created by NameList)
        in NameList
    
    Each child에 unique key가 필요하다.
    List에는 각 항목을 구분해주는 key값이 필요하다.

```js
function NameList(props){
    const names = props.names;
    const listItem = names.map((name) =>
        //키값 추가 
        <li key={name.toString()}>{name}</li>
    );
    return (
        <ul>{listItem}</ul>
    );
};
 
const names = ['kendrick', 'christopher', 'theo', 'dave'];
ReactDOM.render(
    <NameList names={names} />,
    document.getElementById('root')
);
```
    li에 key attribute를 추가해 준다.
    key는 현재는 name값을 넣어줬지만, 일반적으로 항목을 고유하게 식별 할 수 있는 값으로 지정한다.

📌 유의사항
    1. 키는 배열의 컨텍스트에 의미가 있다.
    2. 키는 형제중 유일해야한다.

### Forms
    🌱 form 안에 여러가지 요소가 들어있을 땐 name 속성을 부여해 각각을 구분할 수 있도록 한다. 이는 event.target.name으로 조회 가능하다.
    🌱 value 속성을 통해 값을 부여한 경우, 타이핑을 해도 업데이트 되지 않으므로 onChange를 통해 업데이트 함수를 지정한다.
    🌱 onchange 이벤트는 input, select, textarea 요소에서 발생한다. input 이벤트와 달리 요소 값 각각의 모든 변경마다 반드시 발생하지는 않다.
    🌱 input에서는 onChange가 텍스트값이 바뀔 때마다 발생한다.
    🌱 onChange와 onSubmit에서의 event.preventDefault는 페이지가 리로딩되는 액션을 못하게 한다.
    🌱 form은 controlled components 중 하나로, (순수 HTML input에서 사용자가 자유롭게 입력한 데이터를 submit 같은 특정 이벤트가 발생했을 때 입력받은 
        데이터에 접근하는 방식과는 다르게) 리액트 form에서는 컴포넌트 안 state 데이터가 사용자가 화면에 보고 있는 데이터와 동일하도록 만드는 것을 권장한다.
    🌱 form은 입력할 때 마다 특정 부분이 리렌더링 되어도 리액트 자체적으로 효율적이게 DOM을 없데이트 해주기 때문에 성능은 크게 걱정하지 않아도 된다.

### Lifting State Up
    🌱 부모컴포넌트(Main)의 함수가 자식컴포넌트(Search)로 전달되어져야 한다.
    🌱 자식컴포넌트에서는 onSearch를 전달받아야 한다.(props 명시 해줘야 함)
    🌱 자식컴포넌트에서의 onSearch 함수는 handleSearchClick 함수가 호출될 때 실행된다.


