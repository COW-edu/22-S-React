# 7/27일 학습 내용

# useMemo

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

## useMemo 예시

```jsx
import React, { useEffect, useState} from "react';

function App(){
const [number , setNumber] = useState(0);
const [isKorea,setIsKorea] = useState(true);

const location = isKorea ? "한국" : "외국" ;
/*문자열이기에 state로 인해 리렌더링 되어도 문자열 값은 같아서 바뀌었다고 판단을 안하고 
useEffect를 state 바뀜으로 인해서는 실행을 안한다*/

/* const location = {
country : isKorea ? '한국' : '외국';
}*/

/*하지만 이처럼 객체로 되어있을땐 상황이 달라진다 state 변경으로 인해 
리렌더링이 되면 location이 다시 생성되는데 아까는 문자열이여서 같은 값으로 판단했지만
지금은 객체여서 우리가 겉보기에 같아보여도 주소값이 다르므로 useEffect는 다르다고 판단
그래서 객체 상태면 state 변화로 인한 리렌더링으어도 location도 변경되었다고 생각해서
useEffect가 계속 호출되는거*/
 

/*const location = useMemo(()=> {
return{
country : isKorea ? '한국' : '외국';
}
 },[isKorea]); */

/* 이 상황을 해결할거는 useMemo를 사용하는것 */

useEffect(()=>{
console.log('useEffect 호출');
},[location]);

return(
<div>하루 몇끼 먹어요?<div>
<input
 type ="number" 
value ={number} 
onChange={(e) => setNumber(e.target.value)}
/>

<div>어느 나라에 있어요?<div>
<p> 나라 : {location} </p>
<button onClick={()=> setIsKorea(!isKorea)}> </button>
/>
);

}
```

![Untitled](7%2027%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%20%E1%84%82%E1%85%A2%E1%84%8B%E1%85%AD%E1%86%BC%208367485b691e4be1adea478af4a51f4c/Untitled.png)

참고자료

[https://www.daleseo.com/react-hooks-use-memo/](https://www.daleseo.com/react-hooks-use-memo/)

---

# useCallback

## useCallback이 생긴 이유

렌더링을 하면 컴포넌트 함수를 호출한다 컴포넌트 함수도 함수이기에 모든 내부 변수를 초기화한다.

예를 들어 calculate 함수를 담는 변수가 있으면 이는 초기화되어 새로 만들어진 함수 객체를 다시 할당 받게 된다. 

( 물론 초기화가 된다고 그게 실행되는 건 아니고 새로 생성되는거다 실행은 코드에서 호출할때만)

만약 state 변화로 리렌더링이 일어나고 초기화된 함수가 useEffect의 의존성 배열에 있다면 (함수를 담는 객체니까) 새로 생성되어 바뀌었다고 인식해 useEffect를 실행하게 된다.

뿐만 아니라 굳이 다시 생성할 필요없는 함수를 다시 생성하는 것은 메모리에 안좋다.

이를 막기 위해서 생긴게 useCallback이다

## useCallback이란?

**인자로 전달한 콜백함수 그 자체를 memoization 해주는 것**

useCallback은 컴포넌트가 리렌더링 되어도 함수가 다시 초기화되는 걸 막는다

useMemo랑 다르게 리턴하는 값을 메모이제이션하는게 아니다. 함수 자체를 메모이제이션하는 것이다.

useCallback을 사용하면 useEffect가 단순렌더링으로 함수가 바뀌었다고 판단하지 않는다

---

# Hook의 규칙

- hook은 무조건 최상위 레벨에서만 호출해야한다 ( 반복문이나 조건문에서는 호출하면 안돤다 )

      → why? react가 hook이 호출되는 순서에 의존하기 때문에 react는 지역적인 state를 각 hook에 연동시킬 수 있는데 hook의 호출순서가 달라지면 올바른 state 관리가 안된다

- 리액트 함수 컴포넌트에서만 hook을 호출해야한다 순서가 같아야한다

---

# custom hook

## custom hook이란 ?

반복되는 hook 활용 메소드들(use State , useEffect)들을 개발자가 하나로 만들어줌으로써 더 간결하고 보기 좋은 코드를 만드는 것 ( 로직을 묶어주는 것)

-로직의 반복을 최소화하고 재사용성을 높이기 위해서 사용한다

## custom hook 특징

1. **이름이 무조건 use로 시작해야한다**
2. 여러개의 컴포넌트에서 하나의 custom hook을 사용해도 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어있다 →영향 x
3. 각 custom hook 호출에 대해서 분리된 state를 얻게된다

```jsx
//useCounter.jsx
import React , {useState} from 'react';

function useCounter(initialValue){
    const [count , setCount] = useState(initialValue);

    const increaseCount = () => setCount((count)=>count+1);
    const decreaseCount =() => setCount((count)=>Math.max(count-1,0));

    return [count , increaseCount , decreaseCount];
}

export default useCounter;

//Accomodate.jsx
import React, {useState , useEffect} from "react";
import useCounter from"./useCounter";

const MAX_CAPACITY=10;

function Accommodate(props){
const [isFull, setIsFull] =useState(false);
const [count,increaseCount,decreaseCount] = useCounter(0);

useEffect(()=>{
    console.log("==========");
    console.log("useEffect is called");
    console.log(`isFull ${isFull}`);
});

useEffect(()=>{
    setIsFull(count>=MAX_CAPACITY);
    console.log(`Current count value:${count}`);
} ,[count]);
return(
    <div style={{height:"400px",width:"400px", margin:140}}>
        <p><h1>{`총 ${count}명을 수용했습니다`}</h1></p>
        <button onClick={increaseCount} disabled={isFull}>
            입장
        </button>
        <button onClick={decreaseCount}>
            퇴장
        </button>
        {isFull && <p style ={{color:"red"}}> 정원이 가득 찼습니다</p>}
    </div>
);
}

export default Accommodate;
```

## Event란?

이벤트는 특정 사건을 의미한다 ex)사용자가 버튼을 클릭한 사건

이벤트르 처리하는 것은 이벤트를 헨들링하는 것으로 사건을 처리하는 역할을 한다 (Event Listener라고도 함)

## Event 사용시 주의사항

1. 이름은 카멜 표기법으로
2. 이벤트에는 함수 형태의 값을 전달

       HTML에서는 큰 따옴표 안에 실행 코드를 넣었지만 리액트에서는 함수 형태의 객체를 전달하기

       에 중괄호를 사용해서 함수를 넣는다 {   }

1. DOM 요소에만 이벤트를 설정할 수 있다 우리가 직접 만든 컴포넌트에는 이벤트를 자체적으로 설정할 수 없다

다양한 이벤트 보기

[https://ko.reactjs.org/docs/events.html](https://ko.reactjs.org/docs/events.html)

## 조건부 렌더링이란 ?

어떠한 조건에 따라 렌더링이 달라지는 것

특정 조건에 따라 다른 결과물을 렌더링 하는 것

## element를 변수처럼 사용 가능하다

## 컴포넌트 안에서 쓰는 if/else 문

```jsx
function Component(){
if(true){
return <div> 참이다 </p>;
  }
else{
return null;
  }
}

//하지만 이런 경우에는 else 생략도 가능하다

function Component(){
if(true){
return <div> 참이다 </p>;
  }
return null;
}
/*왜냐면 자바스크립트 function 안에선 return이라는
키워드를 만나면 return  밑에 있는 코드는 더 이상 실행하지 
않기 때문이다 */
```

## JSX 안에서 쓰는 삼항 연산자

삼항 연산자는 중첩 사용도 가능하고 jsx내에서 사용할 수 있다 **하지만 if / else문은 return ()안에 있는 JSX안에서 실행이 불가하다**

<div> if{ 어쩌구 } {저쩌구} </div> 가 안됨 !

```jsx
function Component() {
  return (
    <div>
      {
        1 === 1
        ? <p>참이면 보여줄 HTML</p>
        : null
      }
    </div>
  )
}

//삼항 연산자 중첩 사용도 가능

function Component() {
  return (
    <div>
      {
        1 === 1
        ? <p>참이면 보여줄 HTML</p>
        : ( 2 === 2 
            ? <p>안녕</p> 
            : <p>반갑</p> 
          )
      }
    </div>
  )
}
```

## &&연산자

컴포넌트 렌더링을 막기 위해서 null을 리턴하면 렌더링이 되지 않는다 ( 이는 생명주기 함수에 영향을 미치지 않는다) 그래서 특정 컴포넌트가 조건에 만족하지 못해서 렌더링을 하고 싶지 않으면  null을 입력할 수 있다

하지만 중괄호 안에 &&연산자를 이용해서 더 쉽게 해결할 수 있는 것 같다

```jsx
function Component() {
  return (
    <div>
      {
        1 === 1
        ? <p>참이면 보여줄 HTML</p>
        : null
      }
    </div>
  )
} 

// 위 코드랑 아래 코드랑 동일한 역할
function Component() {
  return (
    <div>
      { 1 === 1 && <p>참이면 보여줄 HTML</p> }
    </div>
  )
}
```

## List란 ?

변수나 객체들을 하나의 변수로 묶어 놓은 것

배열과 키를 사용하면 반복적인 것들을 쉽게렌더링 할 수 있다

반복되는 다수의 엘리먼트가 렌더링이 된다.

## 여러개의 컴포넌트 렌더링 하기

```jsx
const numbers = [ 1,2,3,4,5];
const listItems = numbers.map((number)=>
<li>{number}</li>

ReactDOM.render(
<ul>{listItems}</ul>,
document.getElementById('root');
);
```

위 코드처럼 numbers 의 배열을 반복 실행하여 각 항목에 대해 <li> 엘리먼트를 반환하고 엘리먼트 배열의 결과를 listItems에 저장한다 listItems 배열을 <ul> 엘리머트 안에 포함하고 DOM에 렌더링하면 간단하게 배열을 렌더링 할 수 있다

## map 함수

map함수는 배열의 첫번째 아이템 부터 순서대로 작업을 처리해주는 함수이다  

**map함수에 있는 엘리먼트는 키가 필요하다**

```jsx
//for문

const numbers =[1,3,5]
for(let i=0; i<numbers.length;i++){
console.log(numbers[i]);
}

//map함수
const numbers=[1,3,5];
const listItems = numbers.map((number,idx)=>{
console.log(number);
});

//두 코드는 동일한 결과를 가진다
```

## Key란?

리스트 각 아이템은 고유한 키를 갖고 있어야한다.

고유해서 변경 , 추가 , 제거를 구분하기 위해서 키를 사용한다

배열 내부의 엘리먼트에 지정해야 한다

## key가 필요한 이유

실제 배열이 변경되는 상황이 있을 때 동작하는 방법을 알아야한다

ex) a,b,c,d,에서 b와 c 사이 z가 삽입하게 된다면 b와 c 사이 z를 삽입하는 것이 아니라 c→z , d→c , d 추가로 새롭게 렌더링된다. 

ex) a,b,z,c,d,에서 a를 제거하게 되면 a→b, b→z, z→c, c→d 그리고 d를 제거하는 것이다

**이처럼 리스트의 요소 변경은 리스트 요소 모두에 영향을 미친다**

key가 없는 경우 변경이 필요하지 않은 리스트의 요소까지 변경이 일어나서 비효율적이다

이런 부분을 개선하기 위해서 key를 사용하는 것이다

## key의 위치와 선택

key를 선택하는 가장 좋은 방법은 리스트의 다른 항목들 사이에서 해당 항목을 고유하게 식별할 수 있는 문자열을 사용하는 것이다

- 데이터의 id를 key로 사용하는 것이 가장 일반적이다
- 그 배열안에 있는 문자열 자체로 식별할 수도 있지만 그러면 배열 내 중복되는 문자열이 없어야한다
- 인덱스로 할 수도 있지만 항목의 순서가 바뀔 수 있는 경우 key에 index를 사용하는 것은 권장하지 않는다

key의 특징 : key는 자신의 소속 배열 , 형제 사이에서만 고유한 값이어야한다

key의 위치는 주변 배열의 context에서만 의미가 있다.

```jsx
function ListItem(props) {
  const value = props.value;
  return (
    // 틀렸습니다! 여기에는 key를 지정할 필요가 없습니다.
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 틀렸습니다! 여기에 key를 지정해야 합니다.
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

## Controlled components

리액트는 컴포넌트의 내부에서 state를 관리하지만 html은 element 내부에서 stae를 관리한다

사용자가 입력한 값에 접근하고 제어할 수 있도록 하는것이 controlled components이다 (리액트가 통제를 하는 것)

## 여러가지 종류의 폼

1. textarea태그

긴 텍스트를 받기 위한 태그이다

이 텍스트의 값을 react를 통해서 제어할 수 있다

1. select 태그

드롭다운 목록을 만드는 태그이다 select 태그의 value 어트리뷰트는 선택지라고 생각할 수 있다. 리액트 state안에서 한곳에서 이 value를 관리할 수 있기에 더 편리하다

1. multiple iinput 태그

여러개의 인풋이 있고 이것을 다 제어하기위해서는 여러개의 state를 선언하여 각각의 입력에 대해 사용한다

1. input null value

사용자가 자유롭게 입력하도록 한다