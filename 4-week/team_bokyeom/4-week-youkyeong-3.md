# Handling Events

## Event

: 특정 사건

---

## Event Handling

: 특정 사건을 처리함

### DOM에서의 Event Handling

- event를 처리할 함수를 문자열로 전달

```jsx
//버튼 click -> activate함수 호출
<button onclick='activate()'>Activate</button>
```

### React에서의 Event Handling

- event를 처리할 함수를 함수 그대로 전달

```jsx
<button onClick={activate}>Activate</button>
```

- camelcase(camel표기법)사용

<aside>
💡 camelcase(camel표기법)
→ 첫 글자는 소문자로 시작하지만, 중간에 나오는 새로운 단어의 첫 글자는 대문자로 사용는 양상

</aside>

- false를 사용해도 기본동작 방지 불가능 →preventDefault 명시적 호출 요구

```jsx
//JS
<form onsubmit="console.log('You clicked submit.'); return false">
  <button type="submit">Submit</button>
</form>

//React.js
function Form() {
  function handleSubmit(e) {
    e.preventDefault();    
    console.log('You clicked submit.');
  }
  return (
    <form onSubmit={handleSubmit}>   
     <button type="submit">Submit</button> 
    </form>);
}
```

---

## Event Handler=Event Listener

: 특정 사건(event)를 처리하는 함수

### Class Component에서 Event Handler추가하는 방법

1. **Bind사용**
    - Bind
        - 일반적으로 함수의 이름 뒤에, 괄호 없이 사용하는 경우에는 해당 함수 bind필수
        - JS 기본적으로 class 함수들이 bound되지 않기때문에, 따로 bind를 하지 않으면 this.handleClick은 global 변수(undefind)에서 호출됨
    
    ```jsx
    class Toggle extends React.Component{
      constructor(props){
         super(props);
         this.state={isToggleOn:true};
        
        //handleClick bind
         this.handleClick = this.handleClick.bind(this);
    }
    
    //handleClick정의 part
    handleClick(){
       this.setState(prevState =>({
             isToggleOn:!prevState.isToggleOn
    }));
    }
    render(){
       return(
           <button onClick={this.handleClick}>
                  {this.state.isToggleOn ? '켜짐':'꺼짐'}
           </button>       
         );
       }
    }
    ```
    
2. **Class fields syntax 사용**
    
    ```jsx
    class Toggle extends React.Component{
     
       //Class fields syntax
       handleClick = () => {
        console.log('this is : ' ,this);
      }
       //
      render(){
        return(
         <button onClick={this.handleClick}>클릭</button>
        );
      }
    }
    ```
    
3. **Arrow function(화살표 함수) 사용**
    - 해당 Compontnent(Toggle component)가 rendering될 때마다, 다른 callback함수가 생성되는 문제를 갖음 → 해당 component가 하위component의 prop으로 넘겨지게 되는 경우에는 하위 component에서 추가 rendering을 야기함
    
    ```jsx
    class Toggle extends React.Component{
        handleClick() {
        console.log('this is : ' ,this);
      }
    
      render(){
         return(
    //Arrow function
         <button onClick={()=> this.handleClick()}>클릭</button>
    //
        );
      }
    }
    ```
    

### 함수 Component에서 Event Handler추가하는 방법

- 이벤트를 넘겨줄 때 this를 사용하지 않고, 정의한 eventHandler 넘겨주면 ok
- 함수 내부에 함수 정의,  Arrow function으로 정의 2가지 방법 존재

```jsx
function Toggle (props){
  const [isToggleOn, setIsToggleOn]=useState(true);
   
   function handleClick(){//함수 내부에 함수로 정의하는 방법
      setIsToggleOn((isToggleOn)=>!isToggleOn);
   }
   const handleClick= () => {//Arrow function을 사용하여 정의하는 방법
      setIsToggleOn((isToggleOn)=>!isToggleOn);
   }

   return(
       <button onClick={handleClick}>//this.함수이름 NO, '함수이름'만
              {isToggleOn ? '켜짐':'꺼짐'}
       </button>       
     );
   }
```

---

### Arguments

: Arguments=parameter=매개변수=eventHandler에 전달할 내용

### Event Handler로의 Arguments전달 방법 - **class component**

- 사용빈도 낮음
- 아래 두개의 button은 동일 기능 수행
- event = react의 event객체를 의미
- 첫번째 parameter:id, 두번째 parameter:event

```jsx
//Arrow function 사용
<button onClick={(event) => this.deleteItem(id,event)}></button>

//bind 사용
//event가 자동으로 id이후에 두번째 parameter로 전달
<button onClick={this.deleteItem.bind(this,id)}></button>
```

### Event Handler로의 Arguments전달 방법 - 함수 **component**

- 사용빈도 높음
- 매개변수의 순서는 원하는대로 변경 가능

```jsx
function MyButton(props){
    const handleDelete=(id, event) =>{
         console.log(id,event.target);
    };
    return(
      <button onClick={(event)=> handleDelete(1,event)}></button>
   );
}
```