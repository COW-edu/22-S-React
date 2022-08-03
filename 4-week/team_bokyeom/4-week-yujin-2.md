# COW STUDY

### 4주차 - 2 notion 정리

## Hook의 규칙

1. Hook은 무조건 **최상위 레벨**에서만 호출해야 한다. (즉 Hook은 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 한다.)

1. **리액트 함수 컴포넌트**에서만 Hook을 호출해야 한다.

- eslint-plugin-react-hooks (훅의 규칙을 따르도록 강제해주는 plugin - 자바스크립트 코드에서 발견되는 문제코드를 식별하기 위한 정적 코드분석 도구)

## Custom Hook (이름이 use로 시작하고 내부에서 다른 Hook을 호출하는 하나의 자바스크립트 함수)만들기

- 만드는 이유 : 여러 컴포넌트에서 반복적으로 사용되는 로직을 훅으로 만들어 재사용하기 위함

 

● Custom Hook을 만들어야 하는 상황 

![d1.jpg](COW%20STUDY%2083559ab2733e468094fc9181a11ba5b6/d1.jpg)

→ userStatus 등 중복된 로직을 제거하기 위함

● Custom Hook으로 추출

● Custom Hook 사용하기 

![d2.jpg](COW%20STUDY%2083559ab2733e468094fc9181a11ba5b6/d2.jpg)

## Custom Hook 주의점

1. Custom Hook의 이름은 꼭 use로 시작해야 한다 

1. 여러 개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있다.

1. 각 Custom Hook 호출에 대해서 분리된 state를 얻게 된다. 

1. 각 Custom Hook의 호출 또한 완전히 독립적이다. 

## [추가조사]

# **useMemo**

- **연산된 값(결과)를 반환하여 재사용할 수 있게 해주는 Hook**
- **첫 번째 인자로 콜백함수, 두 번째 인자로 의존성 배열을 받음**
- **값을 메모이제이션 하여 의존성 배열에 있는 값의 변경을 감지하여 *캐싱된 값을 반환***

import { useMemo } from 'react';
// react에서 useMemo를 import해야 한다.

 const [color, setColor] = useState("red");
 const getColor = useMemo(() => console.log(`색은 ${color} 입니다`), [color]);
 /* 
useMemo에는 함수를 잘 담지 않는다.
값이 바로 선언되기 때문에 변수인 값을 담는다.
즉 해당 값이 의존성 배열에 있는 값이 바뀌어야만 랜더링 시 재호출되기 때문에 
[메모이제이션]을 실현할 수 있다.
*/

......

  const onChangeColor = () => {
    if (color === "red") {
      setColor("blue");
    } else {
      setColor("red");
    }
  };

return (
    <>
      <div>Memo</div>
      <input type="text" readOnly value={color} style={{ color }} />
      <button onClick={onChangeColor}>변경</button>
    </>
  );
};

**위의 useMemo함수를 통해 변화한 color 값을 전달하여 계속해서 랜더링하여 console에 메시지를 보여준다.**

# ****

# **[버튼 클릭 → color 값 변화]**

![https://postfiles.pstatic.net/MjAyMjA3MjRfMjk3/MDAxNjU4Njc0MDcwMTgy.Go-DoAsB1SIQF0EUX4oE4nVrf1QmoyMuYNBsxFhDNkQg.jSh1z68uqHntJSQKozTyWzodTryI_Sv0zuApVARDIJgg.PNG.pjhkim0408/image.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjRfMjk3/MDAxNjU4Njc0MDcwMTgy.Go-DoAsB1SIQF0EUX4oE4nVrf1QmoyMuYNBsxFhDNkQg.jSh1z68uqHntJSQKozTyWzodTryI_Sv0zuApVARDIJgg.PNG.pjhkim0408/image.png?type=w773)

---

# **useCallback**

- **특정 함수를 새로 만들지 않고 랜더링되었을 때 재사용할 수 있게 해주는 Hook**
- **함수를 메모이제이션하여 의존성 배열에 있는 값의 변경을 감지하여 *캐싱된 함수 자체를 반환***

import { useCallback, useState } from "react";
// 마찬가지로 useCallback을 import 해야 한다.

const Callback = () => {
  const [color, setColor] = useState("red");

  const onChangeColor = useCallback(() => { // <-- color의 변화를 감지하여
    if (color === "red") {                  // red일 때 click --> blue
      setColor("blue");                     // blue일 때 click --> red (반복)
    } else {
      setColor("red");
    }
  }, [color]);

return (
    <>
      <div>useCallback</div>
      <input type="text" readOnly value={color} style={{ color }} />
      <button onClick={onChangeColor}>변경</button>
    </>
  );
};
export default Callback;

**버튼 click을 통해 의존성 배열에 있는 color의 값이 변경되면 useCallback에 의해,**

**변경된 color 값이 반영된 함수가 반환된다.**

****

**만약 의존성 배열에 있는 color을 삭제한다면, click을 통해 color의 값이 변경된 후 업데이트된 함수가 반환되지 않는다. 즉, 더 이상의 변화가 일어나지 않는다.**

****

**이와 같은 성질은, useCallback이 함수와 상관없는 상태값이 변할 때 함수 컴포넌트에서 불필요하게 함수를 업데이트하는 것을 방지한다.**

****