# 4-week-soyun-2

<aside>
💡 useMemo, useCallback 조사하기 / Hook(섹션7-3,7-4)

</aside>

- 렌더링이 뭘까..
    - 컴포넌트가 렌더링 된다는 것은 그 컴포넌트가 호출되고 실행되는 것을 말한다.
    - 컴포넌트가 실행될 때 마다 내부에 정의된 내용이 매번 다시 선언된다.
    - 컴포넌트는 자신의 state가 변경되거나, 부모에게서 받는 props가 변경되었을 때 마다 리렌더링된다.
- useMemo와 useCallback은 왜 쓸까
    - state와 props가 여러개이고, 그중 하나만 변경되었는데 다른 state와 props가 다같이 렌더링되면 매우 비효율적일것이다.

# useMemo

```jsx
const memoizedValue = useMemo(()=> computeExpensiveValue(a,b) , [a,b] )
```

useMemo함수는

두번째 인자인 의존성 배열이 변경되기 전 까지

첫번째 인자로 받은 값을 기억한다.

```jsx
const getNumber = (number) => {
  console.log("숫자가 변동되었습니다.");
  return number;
};

const getText = (text) => {
  console.log("글자가 변동되었습니다.");
  return text;
};

const ShowState = ({ number, text }) => {
  const showNumber = getNumber(number);
  const showText = getText(text);

  return (
    <div className="info-wrapper">
      {showNumber} <br />
      {showText}
    </div>
  );
```

위 예제에는 number와 text를 props로 하는 ShowState라는 컴포넌트가 있다.

showNumber 나 showText 둘 중 하나라도 변경되면 모두 재렌더링될것이다. 

```jsx
const getNumber = (number) => {
  console.log("숫자가 변동되었습니다.");
  return number;
};

const getText = (text) => {
  console.log("글자가 변동되었습니다.");
  return text;
};

const ShowState = ({ number, text }) => {
  const showNumber = useMemo(()=>getNumber(number),[number]);
  const showText = useMemo(()=>getText(text),[text]);

  return (
    <div className="info-wrapper">
      {showNumber} <br />
      {showText}
    </div>
  );
```

이전 예제에서 showNumber와 showText에 useMemo함수를 사용해서 값을 전달해주는 코드이다.

이렇게 하면 showNumber나 showText 가 변경되면, 변경된 것만 재렌더링할것이다.

# useCallback

useMemo와 같은 이유로 사용된다.

## useMemo와 useCallback의 차이

```jsx
const getItems = useCallback(
    increaseValue => {
      return [
        number + increaseValue,
        number + 1 + increaseValue,
        number + 2 + increaseValue,
      ]
    },
    [number]
  )
```

```jsx
const getItems = useMemo(
    () => increaseValue => {
      return [
        number + increaseValue,
        number + 1 + increaseValue,
        number + 2 + increaseValue,
      ]
    },
    [number]
  )
```

위의 두 코드는 number의 값을 설정하고, increaseValue값(input)이 들어오면 number에 increaseValue를 하나하나 넣어주는 코드이다.

같은 기능이지만, useCallback이냐 useMemo냐에 따라 미세하게 코드가 달라졌다.

```jsx
//useMemo
useMemo(()=>fn, deps)
//useCallback
useCallback(fn, deps)
```

- useMemo는 함수를 반환하지 않고 함수의 값만 memoization해서 반환하기 때문에 파라미터로 함수(위에는 화살표함수)를 전달해줘야한다.
- useCallback은 함수 자체를 반환한다.

# 커스텀 Hook

## 커스텀 Hook이란?

→ 개발자의 입맛에 따라 코드를 묶어 Hook으로 만든것.

→ 반복되는 로직을 재사용하기 위해 사용한다. 

## 사용방법?

1. 커스텀 Hook은 기존의 Hook 규칙을 그대로 따라야 한다.
- Hook 규칙
    1. 리액트 함수 내에서만 사용될것.( 일반적인 js는 안된다.)
    2. 리액트 함수 최상위에서 호출할것.(반복문,조건문 등 호출 순서가 꼬일 가능성을 없애기위해)
1. 다른 훅 들과 똑같이 커스텀 훅을 만들때 앞에 use만 붙여주면 된다.

## etc

- 커스텀 훅을 사용할 때 고려해야할 점
    - 커스텀훅을 너무 남발하지 않았는지. (사용하지 않아도 될 곳에 사용X)
    - 기능을 잘 분리해서 훅으로 만들었는지.( 단일기능? 클린코드와 관련있다고함.)
    - 사이드이펙트가 일어날 가능성은 없는지. (렌더링 중에 실행되는 점을 고려)

# 소감(?)

리액트 프로젝트로 to-do list를 많이 하더라구요.

커스텀 Hook처럼 커스텀 단축키를 직접 지정해서 사용할 수 있는 to-do list를 만들고싶어요.

(파란색 형광펜은 ctrl+alt+b 라던가..)

실현가능한지는 모르겠습니다 ㅎㅎ