# Event

**:특정 사건을 의미한다.**

**ex) 사용자가  버튼을 클릭한 사건**

**Event를 처리하는 것을 Event를 Handling한다고 말한다.**

**Dom의 클릭 이벤트 처리**

```jsx
<button onclick="activate()">
	Activate
</button>       //버튼을 누르면 ACtivate 함수 호출
```

**리액트의 클릭 이벤트 처리**

```jsx
<button onClick={activate}>
	Activate
</button>    //이벤트를 처리할 함수를 함수 그대로 전달한다 
```

**Event Handler(Event Listener)**

**:사건을 처리하는 역할**



**버튼을 클릭하면 Event Handler함수인 handleClick함수가 호출된다.** 

**자바스크립트에서는 기본적으로 클래스 함수들이 바운드 되지 않는다. bind를 하지 않으면 this.handleClick은 global scope에서 호출되는데 global scope에서 this.handleClick은 undefined이므로 사용할 수 없다.**

**bind를 사용하고 싶지 않을 때 Class field syntax사용**



**bind와 class field문법도 사용하기 싫을때**



**이 방식은 콜백함수가 하위 컴포넌트의 prop으로 넘겨지게 되면 하위 컴포넌트에서 추가적인 rendering이 일어나게 된다.**

**class component는 복잡하고 잘 쓰이지 않는다.**

**함수 컴포넌트에서의 이벤트 처리 방법**

**함수 컴포넌트에서는 클래스 컴포넌트에서 event를 넣어줄때처럼 this를 사용하지 않고 바로 eventhandler를 넘기면 된다**

**Argument = parameter**

**:’주장’이라는 의미와 가깝다**

**함수(event handler)에 주장할 내용 = 함수에 전달할 데이터**

**event handler에 매개변수를 전달할 상황은 매우 많다.**

 **예를 들어, 특정 사용자 프로필을 클릭했을 때 해당 사용자 id를 매개변수로 전달해서 정해진 작업을 처리하는 경우**


