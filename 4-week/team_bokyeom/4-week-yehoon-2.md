# Hook(2)

**Hook의 특징**

- **Hook은 무조건 최상위 레벨에서만 호출해야 한다. → 반복문이나 조건문 또는 중첩된 함수 안에서 Hook을 호출하면 안 된다.**
- **리액트 함수 컴포넌트에서만 Hook을 호출해야 한다.**

**잘못된 Hook 사용법**


잘못된 이유: if문에 useEffect() Hook이 걸려서, 만약 if문이 false라면 Hook이 호출되지 않기 때문이다. 그렇게 되면 렌더링 할 때마다 Hook이 같은 순서로 호출되지 않는다.

## eslint-plugin-react-hooks

**:Hook의 규칙을 따르도록 강제해주는 프로그램**

**리액트 컴포넌트가 Hook의 규칙을 따르는지 아닌지 확인해준다.**

## Custom Hook

- Custom Hook의 이름은 꼭 use로 시작해야 한다!
- 여러 개의 컴포넌트에서 하나의 Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있다.
- 각 Custom의 호출 또한 완전히 독립적이다.

## **useMemo와 useCallback**

### **먼저 메모이제이션(memoization) 이란?**

**useMemo 함수에 대해서 알아보기 전에 알고리즘 시간에 자주 나오는 메모이제이션(memoization) 개념에 대해서 잠깐 짚는다.**

**memoization이란 기존에 수행한 연산의 결과값을 어딘가에 저장해두고 동일한 입력이 들어오면 재활용하는 프로그래밍 기법을 말한다.**

**memoization을 절적히 적용하면 중복 연산을 피할 수 있기 때문에 메모리를 조금 더 쓰더라도 애플리케이션의 성능을 최적화할 수 있다.**

## **useMemo**

**:메모이제이션된 값을 반환한다.**

**useMemo는 이전 값을 기억해두었다가 조건에 따라 재활용하여 성능을 최적화 하는 용도로 사용된다. (특정 value를 재사용)**

-**기본 사용법-**

```jsx
const memoizedValue = useMemo()) => computeExpensiveValue(a,b), [a,b]);
```

**useMemo 안에 첫 번째 인자는 내가 연산할 함수, 그리고 연산할 값을 배열로 지정합니다.**

**useCallback**

**:메모이제이션된 함수를 반환한다.**

**useCallback은 리액트의 렌더링 성능을 위해서 제공되는 Hook이다.**

**컴포넌트가 렌더링 될 때마다 내부적으로 사용된 함수가 새롭게 생성되는 경우,**

**자식 컴포넌트에 Prop으로 새로 생성된 함수가 넘겨지게 되면 불필요한 리렌더링이 일어날 수 있다. (특정 함수를 재사용)**

-**기본 사용법-**

```jsx
const memoizedCallback = useCallback(
	() => {
		doSomething(a,b);
	},
	[a, b],
);
```

**우리가 선언한 함수를 computeExpensiveValue() 위치에 넣으면 된다.**

## 요약

```jsx
useMemo는 값을 메모이제이션하여
의존성 배열에 있는 값의 변경을 감지하여 캐싱된 값을 반환하는 반면
useCallback은 함수를 메모이제이션 하여
의존성 배열에 있는 값의 변경을 감지하여 캐싱된 함수 자체를 다시 반환하는 것의 차이이다.
```