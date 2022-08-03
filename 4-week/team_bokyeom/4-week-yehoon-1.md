# Hook

## **왜 생겼나?**

**Component의 종류**

- **Class Component**
- **Function Component**

**Class Component 특징**

1. **생성자에서 state를 정의**
2. **setState() 함수를 통해 state 업데이트**
3. **Lifecycle methods 제공**

## 이에 반해 Function Component는 state 사용 불가하고 Lifecycle에 따른 기능 구현 불가하다.

**useState()**

**:state를 사용하기 위한 Hook**

```jsx
const [변수명, set 함수명] = useState(초기값);
```

**useEffect()**

**:Side effect를 수행하기 위한 Hook(★보통 side effect는 부작용을 의미하지만 리액트에서 side effect는 효과, 영향 정도로 볼 수 있다.)**

```jsx
useEffect(이펙트 함수, 의존성 배열);
```

Effect function이 mount, unmount 시에 단 한 번씩만 실행하려면?

```jsx
useEffect(이펙트 함수, []);
```

의존성 배열을 생략하면 컴포넌트가 업데이트될 때마다 호출 된다.

**useMemo()**

**:Memoized value를 리턴하는 Hook**



**의존성 배열을 넣지 않을 경우, 매 렌더링마다 함수가 실행된다.**

**의존성 배열이 빈 배열일 경우, 컴포넌트 마운트 시에만 호출 된다.**



**useCallback()**

**:useMemo() Hook과 유사하지만 값이 아닌 함수를 반환한다는 차이가 있다.**


**useRef()**

**:Reference를 사용하기 위한 Hook**

**리액트에서 Reference: 특정 컴포넌트에 접근할 수 있는 객체**



**useRef() Hook은 내부의 데이터가 변경되었을 때 별도로 알리지 않습니다.**

**Hook의 규칙**

- **Hook은 무조건 최상위 레벨에서만 호출해야 한다. ex)반복문, 중첩문에서 Hook을 호출하면 안된다.**
- **Hook은 컴포넌트가 렌더링 될 때 마다 같은 순서로 호출되어야 한다.**
- **리액트 함수 컴포넌트에서만 Hook을 호출해야 한다.**

**eslint-plugin-react-hooks**

**:위의 두 규칙을 강제하는 플러그인**

**Custom Hook**

- **이름은 꼭 use로 시작해야 한다.**
- **여러 개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 분리되어 있다.**