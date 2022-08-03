# COW STUDY

### 4주차 - 1 notion 정리

# Hooks

: 갈고리 _ 끼어들어가 같이 실행

(모두 use로 시작)





# useState()

: state를 사용하기 위한 Hook





[코드설명]

Counter component는 버튼을 클릭하면 count를 하나씩 증가시키고 현재 상태를 보여주는 

단순한 component

count값을 변수로 사용하면 버튼 클릭시 count값을 증가시킬 수는 있지만 재렌더링이 일어나지 않아 새로운 count값이 화면에 표시되지 않게 됨 

→ 여기에서 state를 통해 값이 바뀔때마다 재렌더링하게 해야하는데 

함수component는 해당 기능이 없기 때문에 usestate를 이용해 해당 내용을 업데이트 해야함

● 사용법


초기값을 설정하고 출력하면 배열이 나타나는데 첫번째 항목은 이펙트 함수 , 두번째 항목은 의존성 배열

# useEffect()

: Side effect를 수행하기 위한 Hook 

리액트의 함수 컴포넌트에서 Side effect를 실행할 수 있게 해주는 Hook (class component에서 제공하는 생명주기 함수인 componentdidMount, componentdidUpdate, componentwillUnMount와 동일한 기능을 하나로 통합해서 제공함)

● 리액트에서 말하는 Side effect = 효과, 영향

(서버에서 데이터를 받아오거나 수동으로 dom을 변경 -

이런 작업들을 effect라고 부르는 이유 : 이 작업들이 다른 컴포넌트에 영향을 미칠 수 있으며, 

렌더링 중에는 작업이 완료될 수 없기 때문)

● effect가 의존하고 있는 배열이 의존성 배열 안에 있는 값이 하나라도 변경되면 effect 함수가 실행됨



[코드설명]

useEffect안의 effect함수에서는 브라우저에서 제공하는 API를 사용해서 

document의 title을 업데이트함

이 코드처럼 의존성 배열없이 useEffect를 사용하면 react는 dom이 변경된 이후에 해당 effect함수를 실행하는 의미로 받아들임 

→ 매번 렌더링 될 때마다 effect가 실행

● effect는 함수컴포넌트 안에서 선언되기 때문에 해당 컴포넌트의 props와 state에 

접근할 수도 있음



# useMemo

: Memoized value를 리턴하는 Hook

● Memoization : 비용이 높은, 즉 연산량이 많이 드는 함수의 호출 결과를 저장해두었다가 

입력값으로 함수를 호출하면 새로 함수를 호출하지 않고 저장해놓았던 호출결과를 반환하는 것



● useMemo로 전달된 함수는 렌더링이 일어나는 동안 실행됨

→ 그렇기 때문에 렌더링이 일어나는 동안 실행되어서는 안될 작업을 넣으면 안됨



# useCallback

: useMemo() Hook과 유사하지만 값이 아닌 함수를 반환 (component가 렌더링될때마다 매번 함수를 새로 정의하는 것이 아니라 의존성 배열의 값이 바뀐 경우에만 새로 정의해서 리턴해줌)

● 의존성 변수가 하나라도 바뀌면 memoizedCallback함수를 반환



# useRef

: Reference를 사용하기 위한 Hook 

(Reference : 특정 컴포넌트에 접근할 수 있는 객체)

● useRef와 직접 current를 만들어 사용하는 것의 차이점 :

렌더링될 때마다 항상 같은 reference객체 반환함

● useRef() Hook은 내부의 데이터가 변경되었을 때 별도로 알리지 않는다



### [추가조사]

**📍useState**

컴포넌트가 렌더링 될 때 특정 작업을 실행할 수 있도록 하는 Hook.

리액트의 useEffect 훅을 사용하면 함수 컴포넌트에서도 side effect를 사용할 수 있다.

useEffect란? [https://blog.naver.com/ghdalswl77/222662749403](https://blog.naver.com/ghdalswl77/222662749403)

**📍useEffect**

component가 마운트 / 언마운트 / 업데이트될 때, 할 작업을 선택하도록 하는 Hook함수.

외부 API를 요청하거나, 반복작업 / 작업예약 등에 쓰인다.

- *mount*

: component가 처음 화면에서 보여질 때 (=새로고침 시) (componentDidMount 메서드)

- *unmount*

: component가 화면에서 사라질 때 (componentWillUnmount 메서드)

- *update*

: 특정 props가 바뀌어 component가 업데이트될 때 (componentWillUpdate 메서드)

**📍useRef**

특정 DOM을 가리킬 때 사용하는 Hook함수. 대상에 대한 포커스 설정, 특정 엘리먼트의 크기나 색상을 변경할 때 사용한다. HTML에서 id를 사용하여 DOM에 이름을 다는 것처럼 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법.

ref 자체의 뜻이 Javascript의 getElementById() 처럼 컴포넌트의 어떤 부분을 선택할 수 있게 해주는 방법. DOM에 접근 할 수 있도록 하는 기능을 가진 hook이라고 보면 된다. 리액트에 있는 모든 컴포넌트는 reference element(래퍼런스 엘리먼트)를 가지고 있어서 어떤 컴포넌트에 ref={변수명}을 넣어준다면 리액트에서 해당 컴포넌트를 참조하게 된다.

📝 ***id, getElementById() 사용하지 않고 ref를 사용하는 이유***

예를들어 컴포넌트 안에 있는 DOM에 id를 지정한 후 해당 컴포넌트를 여러번 재사용하게 됐을 경우를 생각해보자. 유일해야 하는 id가 더이상 유일하지 않고 그에 따른 문제가 생길 수 있다. 그래서 id값 대신 ref를 사용하는 것을 추천한다.

📝

ref는 객체로써 취급되며 Heap에 저장되어 프로그램 종료시까지 같은 메모리 주소를 가진다.

참고 : [https://muhly.tistory.com/95](https://muhly.tistory.com/95)

함수형 컴포넌트 사용시 호출 때마다 모든 변수가 초기화 되어 값이 변경된다.

하지만 ref를 사용하는 경우는 임의로 값을 변경하지 않는다면 값이 계속 유지된다.

또한, 값이 변경되어도 컴포넌트의 리렌더링이 없어 빠른 참조가 가능하다.

일반적으로 id 값, 인스턴스 값, scroll 위치와 같이 초기화되어 처음부터 넘버링 되지 않는 경우에 ref를 사용한다.