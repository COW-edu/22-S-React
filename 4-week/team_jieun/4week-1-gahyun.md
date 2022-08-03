# 7/25일 학습 내용

## State란?

props와 동일하게 렌더링 결과물에 영향을 주는 데이터를 갖고 있는 **객체**지만 함수내에서 선언된 변수처럼 컴포넌트 안에서 관리하며 값의 변경이 가능하다

## State의 특징

state는 객체에서 직접적으로 변경이 불가하고 setState 함수를 통해서만 변경이 가능하다

setState 함수 , React는 왜 불변 객체 ?

setState란 React의 불변성을 지키며 state의 값을 변경하기 위한 리액트 함수이다. 리액트에서는 모든 상태 값들을 불변성을 유지하며 관리하게 된다. 왜 그렇게 하냐 ? **바로 virtualdom과의 차이를 알아내기 위함이다.** 만약 객체가 직접적으로 값이 변경하게 된다면 **변경된 객체를 찾기 위해서 실제 돔과 가상 돔 전체 트리를 비교하며 변경사항을 감지해야한다**. 

하지만 불변 객체를 사용하면 **변화 감지가 쉬워진다**. 단순히 참조하고 있는 불변 객체가 이전 객체와 다른지를 얕은 평가만으로 확인할 수 있기 때문이다. 또한 객체가 변경된걸 쉽게 판단할 수 있어서 이걸로 컴포넌트가 다시렌더링할지를 결정한다. 

이러한 특성을 바탕으로 setState 메서드가 호출되면 상탯값을 변경하고 해당 컴포넌트를 다시 렌더링하게 된다.

## State와 props의 차이

- props는 해당 데이터를 **수정할 수 없는 값**이고 반면에 state는 **내부에서 가지고 있는 값으로써 변경할** 수 있는 값이다
- props는 (함수에서 매개변수처럼) **부모 컴포넌트가 자식 컴포넌트에 전달**되는 반면 state는 (함수 내에 선언된 변수처럼) **컴포넌트 안에서 관리된다**는 차이가 있다. (사용하는 쪽과 구현하는 쪽으로 생각)

state 예시

```jsx
//todo.js
import React from 'react';
import Title from './Title';

class Todo extends React.Component {
  state = { count: 0 };
  onClick = () => {
    const { count } = this.state;
    this.setState({ count: count + 1 });
  };
  render() {
    const { count } = this.state;
    return (
      <div>
        <Title title={`현재 카운트: ${count}`} />
        <button onClick={this.onClick}>증가</button>
      </div>
    );
  }
}

export default Todo;

//title.js
import React from 'react';

function Title(props) {
  return <p>{props.title}</p>;
}

export default Title;
```

1. import를 통해 React 객체도 불러오고 Title js에서 export했던 Title 컴포넌트도 가져온다
2. title.js로 향하면 import를 통해 React 객체를 가져오고 Title이란 함수 컴포넌트를 살펴본다 props라는 컴포넌트에 전달할 다양할 정보를 담고있는 객체를 매개변수로 받는다 .
3. 이 함수 컴포넌트는 p태그로 된 props의 title을 반환한다
4. 이 title js는 Title이라는 컴포넌트를 export한다 
5. todo js에서는 ToDo라는 클래스 컴포넌트가 있다 
6. 이때 state에서 count 속성(변수)는 0을 나타낸다
7. Onclick이라는 변수는 함수를 담고있는데 count 변수는 이 상태를 담고있고 this,setState함수를 통해 count에 1을 더해준다. 즉 이 함수가 실행될때마다 count는 1이 올라간다고 생각할 수 있다 
8. 클래스 컴포넌트 내에는 render함수가 있어야한다. 이 함수를 통해 렌더링을 하는데 일단 render함수가 실행될때마다 count에 this.state를 넣어주고 div 태그 안에 현재 카운트를 나타내는 title 컴포넌트와 버튼이 클릭할때마다 onclick함수가 실행디는 element를 넣어줘서 반환한다.

---

❓여기서 질문

자바스크립트 실행 코드는 중괄호를 사용한다는 것은 알고있었다 하지만 여기서 {count} = this.state는 무엇일까?  

→ 이것은 객체 분해할당으로 const count = this.state.count도 가능하다

클래스 컴포넌트에는 render 함수가 존재하면 root.render이 필요없나? 왜 render 함수가 꼭 존재해야하나

---

## 생명주기 Lifecycle

- 모든 컴포넌트는 여러 종류의 “lifecycle methods”를 가지며, 이 메서드를 오버라이딩하여 특정 시점에 코드가 실행되도록 설정할 수 있다.

![https://velog.velcdn.com/images%2Fdelilah%2Fpost%2F0287824c-2597-4adf-8663-6ff4c318c265%2Freact.png](https://velog.velcdn.com/images%2Fdelilah%2Fpost%2F0287824c-2597-4adf-8663-6ff4c318c265%2Freact.png)

---

- 클래스 컴포넌트는 항상 props로 기본 constructor를 호출해야 한다
    - 마운트 : DOM 에 렌더링 될 때마다
    - 삭제 : DOM 이 삭제될 때마다
- 클래스 컴포넌트에서 "특정 메서드"를 사용해서 컴포넌트가 (un)마운트 될 때, 일부 코드를 작동시킬 수 있다.
- 이때 사용되는 "특정 메서드" 를 생명주기 메서드라고 부른다.

**constructor**해당 컴포넌트가 **" 마운트되기 전"**에 호출된다.

**render"데이터가 변경되어 새 화면을 그려야 할 때 자동으로 호출"**된다.render() 함수가 반환하는 JSX를 화면에 그린다.

**componentDidMount()"render() 함수가 JSX를 화면에 그린 이후"**에 호출되는 함수이다. (= 컴포넌트 출력물이 DOM에 렌더링 된 이후)컴포넌트가 화면에 모두 표현된 이후 해야 하는 작업들을 여기서 진행하게 된다.

**componentDidUpdate()"컴포넌트가 실제 화면에 출력된 이후"** 호출되는 함수이다.이 함수는 부모 컴포넌트로부터 전달된 이전 props 와, 이전 state을 인자로 전달받는다.DOM의 정보를 변경할 때 사용한다.

**componentWillUnmount()"컴포넌트가 소멸되기 직전"**에 호출되는 함수이다.

1. Mount

컴포넌트가 페이지에 처음 렌더링 될때

1. Update

부모가 re-렌더링될때 state가 바뀔때 context가 바뀔때

1. Unmount 컴포넌특 페이지에서 사라질때

참고자료

[https://velog.io/@delilah/React-5-Component-컴포넌트의-생명주기](https://velog.io/@delilah/React-5-Component-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%9D%98-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0)[https://velog.io/@ingong/React-Component-와-Props-State-에-대해서-알아보자](https://velog.io/@ingong/React-Component-%EC%99%80-Props-State-%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)

[https://velog.io/@hidaehyunlee/React-State-란](https://velog.io/@hidaehyunlee/React-State-%EB%9E%80)[https://velog.io/@seong-dodo/React-클래스형-컴포넌트-vs-함수형-컴포넌트](https://velog.io/@seong-dodo/React-%ED%81%B4%EB%9E%98%EC%8A%A4%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-vs-%ED%95%A8%EC%88%98%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)

## Hooks 이란?

함수형 컴포넌트는 클래스 컴포넌트와 다르게 state와 생명주기 함수를 사용할 수 없다. 그래서 클래스 컴포넌트를 사람들이 더 많이 사용하다가 state와 생명주기 함수 역할을 해주는 hooks이 생긴 이후 함수 컴포넌트를 많이 사용하게 되었다.

---

# useState

## useState란?

useState는 가장 기본적인 hook으로서 이를 이용하여 state를 사용할 수 있다. 컴포넌트안에서 상태를 관리해야 하는 일이 생길 때 사용한다 ( 함수 내에서 값이 바뀔 필요가 있을 때)

<aside>
💡 const [ 변수명 , set 함수명 ] = useState(초기값);

</aside>

useState의 사용 법은 변수에 초기값을 설정해준 후 set 함수가 실행되었을 때 변수가 바뀐다고 생각

이때 set함수는 파라미터를 받아서 그 파라미터로 state값이 바뀌게되고 컴포넌트는 정상적으로 리렌더링된다

```jsx
import React, { useState } from 'react';

const Counter = () => {
  const [value, setValue] = useState(0);
  return (
    <div>
      <p>
        현재 카운터 값은 <b>{value}</b> 입니다.
      </p>
      <button onClick={() => setValue(value + 1)}>+1</button>
      <button onClick={() => setValue(value - 1)}>-1</button>
    </div>
  );
};
```

1. 먼저 usestate를 사용할 댄 import 구문을 통하여 불러오고 사용
2. counter라는 함수형 컴포넌트가 있고 이 컴포넌트 안에 state가 있다
3. value라는 state는 초기값이 0이고 setValue가 실행될때마다 value값이 변한다
4. 이 함수형 컴포넌트는 div태그를 return해준다
5. div 태그 안에 자식 태그들을 보자면 먼저 value값을 담은 p태그가 출력되고 button태그는 버튼이 눌릴때마다 setValue함수가 실행되기로 정의되었다
6. setValue 함수가 실행될 때 value +1 또는 value -1 값을 파라미터로 받고 그 파라미터가 state값으로 바뀐다.

## useState 여러번 사용하기

useState 함수는 하나의 상태값만 관리할 수 있기에 만약에 컴포넌트에서 관리해야할 상태가 여러개라면 useState를 여러번 사용 가능

```jsx
import React, { useState } from 'react';

const Info = () => {
  const [name, setName] = useState('');
  const [nickname, setNickname] = useState('');

  const onChangeName = e => {
    setName(e.target.value);
  };

  const onChangeNickname = e => {
    setNickname(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nickname} onChange={onChangeNickname} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임: </b>
          {nickname}
        </div>
      </div>
    </div>
  );
};

export default Info;
```

1. info라는 컴포넌트는 name과 nickname이라는 state를 가지고 있다
2. onChangeName은 함수를 담는데 이 함수는 실행될때마다 setNickname 함수를 통해서 state의 값이 파라미터인 그 이밴트의 실행되는 요소의 값으로 바뀐다
3. Nickname함수도 체인지 함수랑 마찬가지이다
4. 이 함수형 컴포넌트가 반환해주는 값에서 input을 살펴보면 이 input값이 value는 state인 name과 nickname인데 이 input이 바뀔때마다 onChangeName함수나 onChangeNickname함수를 실행한다
5. 즉 input값 이름과 닉네임 input에 적은 값이 바뀔떄마다 그 값으로 컴포넌트가 리렌더링되어 화면에 업데이트 된 값으로 보인다.

## 하나의 useState()로 여러 상탯값 관리하기 (객체)

state가 위처럼 하나의 값만 받는게 아니라 여럿 값을 받아 객체 형태를 이룰 수 있다

초기값이 객체인 셈이다

```jsx
import React, { useState } from 'react'
export default function Profile () {
    const [state,setState] = useState({name:'',age:0})
    return (
        <div>
            <p>{`name is ${state.name}`}</p>
            <p>{`age is ${state.age}`}</p>
            <input type='text' value={state.name} 
            onChange={e=>setState({...state,name:e.target.value})}/>
            <input type='text' value={state.age} 
            onChange={e=>setState({...state,age:e.target.value})}/>
        </div>
    )
}
```

객체 형태로 state 값을 관리할 수 있다

**주의할 점**은 클래스 형 컴포넌트에서는 setState를 하면 병합되지만 함수형 컴포넌트에서는 이전 상탯값을 지운다

따라서 **다른 상탯값들이 지워지지 않도록 Spread Syntax를 이용해 명시적으로 …state를 사용해 펼쳐 넣어줘야한다**

참고자료

[https://react.vlpt.us/basic/07-useState.html](https://react.vlpt.us/basic/07-useState.html) -나중에 볼 자료

[https://velog.io/@velopert/react-hooks](https://velog.io/@velopert/react-hooks)

[https://velog.io/@kwonh/ReactHook-useState-와-useEffect-로-상탯값과-생명주기-사용하기](https://velog.io/@kwonh/ReactHook-useState-%EC%99%80-useEffect-%EB%A1%9C-%EC%83%81%ED%83%AF%EA%B0%92%EA%B3%BC-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

---

# useEffect

## useEffect란?

컴포넌트가 렌더링 될때 특정 작업을 수행하게 하는 함수이다. 어떤 컴포넌트가 렌더링 될때   (mount) 다시 렌더링 될때(update) , 제거될 때 (unmount) 

## useEffect 형태

1. useEffect (   (  )⇒{           }  );

  배열을 안 넣었던 건 처음 렌더링 될때와 업데이트로 인해 화면에 재렌더링 될때 실행한다.

1. useEffect (  () ⇒ {        }  ,[  ]);

 저 배열 안에 값을 넣으면 마운트 되었을떄와 의존성 배열 안에 있는 변수들 중 하나라도 값이           변경되었을 때 실행 만약 의존성 배열에 빈 배열을 넣으면 마운트와 언마운트시에 단 한번씩만 실행이 된다

## useEffect 예시

```jsx
import React, {useState , useEffect} from "react";

function App(){
const [count , setCount ]= useState(1);

const handleCountUpdate =() =>{
setCount(count+1);
};

useEffect(()=>{
console.log( "count 변화 ");
});

return(
<div>
<button onClick={handleCountUpdate}>Update</button>
<span>count = {count} </span>
</div>
);
}

export default App;
```

1. App 함수 컴포넌트를 보면 count라는 상태가 있고 이 초깃값은 1이다.
2. 이를 기반으로 처음 화면에 렌더링 될 때 업데이트 버튼과 카운트가 보이면서 count변화라는 console이 찍힐 것이다.
3. 버튼을 누를때마다 handleCountUpdate라는 함수가 실행되고 그 함수가 실행될때마다 state인 count값이 변화하고 그 업데이트로 인해 화면은 재렌더링된다
4. useEffect함수는 마운트됐을때뿐만아니라 업데이트 되어 화면이 재렌더링될때도 실행하기에 state값이 변화할때마다 count 변화가 출력되는 걸 알 수 있다

## useEffect clean up 함수 (뒷정리하기)

컴포넌트가 필요없어서 해지될 때 사용하는 함수이다

사용법은 useEffect 함수안에 return () ⇒ {   이 안에 원하는 정리 작업들을 적으면 된다  } 

```jsx
import React , { useEffect} from "react";

const Timer =() =>{
useEffect( ()=>{
const timer = setInterval(()=>{
console.log("타이머 돌아가는 중");
}
,1000);

return ()=>{
clearInterval(timer);
console.log("타이머가 종료되었습니다")
}; //cleanup 함수
},[]);

return (<div>
 <span> 타이머를 시작합니다 콘솔을 보세요!</span>
</div>
);
};
export default Timer;
// Timer.jsx

//App.js

import React~~~~
import Timer ~~~~

function App(){
const [showTimer , setShowTimer]= useState(false);
return (<div>
{showTimer && <Timer/>}
<button onClick{() => setShowTimer(!showTimer)}> Toggle Timer</button>
</div>

);}

export default App;

```

1. 먼저 timer 파일을 보자면 처음 렌더링 되었을 때와 언마운트 됐을 때 실행하는 useEffect 함수가 있다 처음 렌더링 되었을 때 1초마다 타이머가 돌아가는걸 출력하는 함수가 실행되고 이 Timer라는 컴포넌트가 제거되었을 때 timer 함수를 제거하고 종료되었다는 문자열을 출력한다
2. 또한 이 컴포넌트 자체는 div 태그안에 span 태그로 타임가 시작합니다 콘솔을 보세요라는 문자열을 보여준다
3. App 파일을 보면 이 App 컴포넌트 안에는 showTimer라는 state와 setShowTimer라는 state 함수가 있다 초깃값은 false 
4. 그러면 return을 할때 showTimer가 false면 Timer 컴포넌트가 보이지않고 true면 Timer 컴포넌트가 보이게된다
5. 또한 버튼을 클릭할때마다 setShowTimer 함수에 의해 showTimer가 true→false로 , false에서 → true로 state를 변경시킨다
6. 그러면 만약에 showTimer가 false일땐 Timer 컴포넌트가 사용이 되지 않아서 (unmount 되어서) timer함수가 제거가 되는 동시에 타이머가 종료되었습니다 라는 메시지가 콘솔창에 뜨게된다

---

# useRef

### useRef란 ?

useRef를 사용하면 ref 오브젝트를 반환하고 useref의 인자값이 current 속성에 저장된다

## useRef 사용 용도

첫번째로 변수관리 용도가 있는데 이를 위해서는 state와 ref의 차이점을 알아야한다

**state와 ref의 차이점**

state는 컴포넌트 내부에서 변화하는 값을 관리해주기 위해 사용되는데 즉 state의 변화는 업데이트로 인지되서 렌더링이 된다. element는 렌더링이 될때 업데이트 해주는 게 아니라 실제 새로운 element를 생성해서 렌더링해주는 것이기에 컴포넌트 내부 변수들을 초기화 시켜준다

하지만 ref는 값이 변화가 일어나도 렌더링이 일어나지않는다 즉 ref 값의 변화로는 렌더링이 일어나지않기에 ref값만 변화했다면 화면에ref 값이 변한것이 업데이트 되지 않는다. 또한 ref는 state 변화로 인해 업데이트 (렌더링)되었다해서 ref 값은 초기화 되지 않고 유지된다

![Untitled](7%2025%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%92%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B3%E1%86%B8%20%E1%84%82%E1%85%A2%E1%84%8B%E1%85%AD%E1%86%BC%2031fa38aa785341138c1fe7c91df9afcc/Untitled.png)

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

이 ref의 object를 우리가 접근하고자하는 요소 태그에 ref속성으로 넣어주김ㄴ 하면 해당 요소에 접근이 가능한 것이다!

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