# 11주차-Lifiting state up

동일한 데이터에 대한 변경 사항을 여러 컴포넌트에서 반영할 필요가 있다 동기화를 해야한다 가장 가까운 공통 조상으로 state를 끌어올리는 것이 이때 사용되야한다.

```jsx
function BoilingVerdict (props) {
if (props.celsius >=100){
return<p> 물이 끓습니다 </p>;
}
return <p> 물이 끓지 않습니다 </p>;
}

//calculator.jsx
function Calculator (props){
const [temperature , setTemperature]= useState('');

const handleChange =(evenet) =>{
setTemperature(event.target.value);
}

return(
<fieldset>
<legened> 섭씨 온도를 입력하세요 :</legend>
<input value={temperature} onChange={handleChange}/>
<BoilingVerdict celsius={parseFloat(temperature)}/>
</fieldset>
);
}
```

BoilingVerdict  컴포넌트로 물이 끓는다 안끓는다를 출력하고 이 컴포너느를 calculator에서 렌더링 한다

```jsx
//두번째 input 추가하기

const scaleNames={
c :'섭씨',
f :'화씨'
};

function TemperatureInput(props){
const [temperature, setTemeperature]= useState('');
const handleChange=(event)=>{
setTemperature(event.target.value);
}

return(
<fieldset>
<legend>
온도를 입력해주세요 (단위 : scaleNames[props.scale]});
</legend>
<input value={temperature} onChange={handleChange}/>
</fieldset>
)
}

//calculator.jsx
function Calculator (props){
const [temperature , setTemperature]= useState('');

const handleChange =(evenet) =>{
setTemperature(event.target.value);
}

return(
<fieldset>
<legened> 섭씨 온도를 입력하세요 :</legend>
<TemperatureInput scale="c"/>
<TemperatureInput scale="f"/>
<BoilingVerdict celsius={parseFloat(temperature)}/>
</fieldset>
);
}
```

아까 그냥 input 코드를 받을 때와 다르게 섭씨 입력 필드 뿐만 아니라 화씨 입력 필드를 추가하고 두 필드간에 동기화 상태를 유지 필요

TemperatureInput에서 temperature state를 따로 관리하기에 둘 중 하나에 온도를 입력하더라도 다른 하나는 갱신되지 않는 문제가ㅏ 발생한다 동기화 상태를 유지하지 못한다는 것이다. 또한 calculator에서는 온도 state가 TemperatureInput에 숨겨져있기에 알 수 없다

우리가 원하는 것은 두 입력값이 서로의 것과 동기화된 상태로 있는 것  섭씨 온도를 입력하면 화씨온도 역시 변환된 온도를 반영할 수 있어야하는 것

그러면 state를 어떻게 공유하냐?  → temeperatureInput에서 개별적으로 가지는 state를 지우고 Calculaor로 그 값을 옮겨놓는 것이다.

부모 컴포넌트에서 공유될 state를 소유하고 있으면 두 입력 필드가 서로간에 일관된 값을 유지한다 props로 부모 컴포넌트에 받기 때문에! 즉 온도값을 state아닌 props에서 가져와서 사용하는 것

```jsx
function TemperatureInput(props){
const handleChange=(event)=>{
props.onTemperatureChange(event.target.value);
}

return(
<fieldset>
<legend>
온도를 입력해주세요 (단위 : scaleNames[props.scale]});
</legend>
<input value={props.temperature} onChange={handleChange}/>
</fieldset>
)
}

```

지금 props로 받은게 부모 컴포넌트의 state인 temperature와 그 state를 바꾸는 함수까지 받은것이다

자식 컴포넌트로 인해 부모 컴포넌트의 state까지 변경될때 state를 바꾸는 함수까지 인자로 받아 해결한다

```jsx
function Calculator (props){
const [temperature , setTemperature]= useState('');
const [scale  setScale]=useState('c');

const handleCelsiusChange=(temperature)=>{
setTemperature(temperature);
setScale('c');
}

const handleFahrenheitChange=(temperature)=>{
setTemperature(temperature);
setScale('f');
}

const celsius= scale ==='f' ? tryConvert(temperature, toCelsius) : temperature ;
const fahrenheit = scale ===  ==='f' ? tryConvert(temperature, toFahrenheit) : temperature ;

return(
<fieldset>
<legened> 섭씨 온도를 입력하세요 :</legend>
<TemperatureInput
 scale="c"
temperature={celsius}
onTemperatureChange={handleCelsiusChange}/>
<TemperatureInput 
 scale="f"
temperature={fahrenheit }
onTemperatureChange={handleFahrenheitChange}/>
<BoilingVerdict celsius={parseFloat(temperature)}/>
</fieldset>
);
}
```

→ **부모 state로 state 공유 동기화**

→ **자식컴포넌트로 인해 부모 컴포넌트 state 변경될 때 부모 컴포넌트 state를 변경 시키는 함수를 props로 받기 (인자로 받기)**

참고자료

[https://nomadkim880901.tistory.com/m/entry/8-1-React-데이터흐름-State-끌어올리기](https://nomadkim880901.tistory.com/m/entry/8-1-React-%EB%8D%B0%EC%9D%B4%ED%84%B0%ED%9D%90%EB%A6%84-State-%EB%81%8C%EC%96%B4%EC%98%AC%EB%A6%AC%EA%B8%B0)