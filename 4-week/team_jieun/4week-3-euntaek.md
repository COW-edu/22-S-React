7월 29일 학습내용 정리 euntaek

# Forms

= 양식

= 로그인시 아이디/비밀번호 입력, 셀렉트박스 등 사용자가 선택을 해야하는 모든 것

= 사용자로부터 입력을 받기위해 사용

### 리액트, html 각각의 form은 차이가 있다?

리액트는 컴포넌트 내부에서 state를 통해 데이터리관리

반면

html 폼은 엘리먼트 내부에 각각의 state가 존재

html폼

```jsx
<form>
		<label>
				이름:
				<input type="text" name="name" />
		</label>
		<button type="submit">제출</button>
</form>
```

이름을 입력받고 제출하는 간단한 폼

→ js코드를 통해 사용자가 입력한 값에 접근하기에 불편한 구조

접근 제어를 쉽게 하기위해

controlled Components를 알아야함

### controlled Components

사용자가 입력한 값에 접근+제어할 수 있도록 하는 컴포넌트

→ 통제 받는 컴포넌트 → 통제는 리액트가

= 값이 리액트의 통제를 받는 InputForm  Element

Html코드에서는 자체적으로 state를 관리하기에 태그 내부에 state를 가지게되고

js코드를 통해 각각의 값에 접근하기 쉽지않음

그러나  controlled Components는 모든데이터를 state로 관리

→리액트에서 입력값이 state를 통해 관리되어 여러 입력 양식의 값을

원하는대로 조종할 수 있음.

# 여러 종류의 폼

## Textarea 태그

= 여러줄에 걸쳐 긴 텍스트를 입력받기위한 HTML태그

```jsx
<textarea>
		안녕하세요 응용22 최은택입니다.
</textarea>
```

Html에서는 다음과 같이 텍스트를 태그로 감싸는 형태로 사용하나

리액트에서는 textarea라는 태그에 value라는 어트리뷰트를 사용하여 텍스트를 표시함

→ controlled Components방식 →값을 컴포넌트의 state를 사용해서 다룰수 있음

## Select태그

= Drop-down 목록을 보여주기 위한 HTML태그

html에서는 다음과같이

```jsx
<select>
		<option value="apple">사과</option>
		<option value="banana">바나나</option>
		<option selected value="grape">포도</option>
		<option value="melon">멜론</option>
</select>
```

옵션태그를 셀렉트태그가 감싸는형태로 사용

현재 선택된 옵션은 selected 라는 어트리뷰트를 갖고있음

리액트에서는 옵션태그에 selected 속성을 사용하지않음

대신 selected의 value라는 어트리뷰트를 사용해서 값을 표시

```jsx
<select multiple={true} value{['a' , 'a+']}>
```

다음과 같이 true를 쓰고 배열을 넣어 여러개 옵션을 선택할 수 있다.

# File input태그

=디바이스의 저장 장치로부터 하나또는 여러 개의 파일을 선택할 수 있게 해주는 HTML태그이다.

사용방법

```jsx
<input type="file" />
```

input타입을 파일로

file input태그는 값이 읽기전용 → 리액트에서는 Uncontrolled Component가 됨

하나의 컴포넌트에서 여러개의 입력을 받으려면?

→ 여러개의 state선언후 각각의 입력에 대해 사용하면 됨

# Lifting State Up

## shared state

=공유된 state

자식 컴포넌트들이 가장 가까운 공통된 부모컴포넌트의 state를 공유하여 사용하는것

= state에 있는 데이터를 여러개의 하위 컴포넌트에서 공통적으로 사용하는 경우

중복된 값을 사용할 필요가없다 → 응용가능

# Composition vs Inheritance

## Composition

= 구성

= 여러개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것 → 합성이라는 의미와 더 가깝다?

컴포지션의 방법은 여러가지가 있다.

1.Containment

= 뜻은 방지, 견제 but 의미는 담다 포함하다가 가깝다

= 하위 컴포넌트를 포함하는 형태의 합성방법

*보통 sidebar, dialog와 같이 box형태의 컴포넌트는 자신의 하위 컴포넌트를 미리 알 수 없음.

(사용방법)

리액트 컴포넌트의 props에 기본적으로 포함되어있는 children 속성을 사용하면 됨

예) children prop을 사용한 fancyBorder 컴포넌트

```jsx
function FancyBorder(props) {
		return (
				<div className = { 'FancyBorder FancyBorder-' + props.color}>
						{props.children}
				</div>
		);
}
```

2.Specialization

= 본 뜻은 전문화, 특수화

= 범용적인 개념을 구별이 되게 구체화 하는 것

기존의 객체지향 언어에서는 상속을 사용하여 Specialization을 구현하였다.

반면에

리액트에서는 합성(Composition)을 사용하여 Specialization을 구현한다.

### Containment와 Specialization을 같이 사용하려면 어떻게 해야할까?

Containment를 위해서 props.children을 사용하고 Specialization을 위해 직접정의한 props를

사용하면된다.

## Inheritance

= 본 뜻은 상속 but 의미는 자식클래스가 부모클래스의 정보를 물려받아 사용하는 것

= 다른 컴포넌트로부터 상속을 받아 새로운 컴포넌트를 만드는 것

하지만 리액트에서는 상속을 사용하기보다 컴포지션방법을 사용하고 지향한다.

(핵심은)

복잡한 컴포넌트를 여러개의 컴포넌트로 나누고 그런 컴포넌트들을 조합하여

새로운 컴포넌트로 만드는 것

# Context

리액트에서 컴포넌트의 props를 통하여 데이터를 전달하였다.(단방향으로)

그러나 여러 컴포넌트에 걸쳐 많이 사용되는 데이터의 경우는 효율적이지 않다.

그래서 Context가 나옴

= 리액트 컴포넌트 사이에서 데이터를 기존의 props를 통해 전달하는것이 아닌

컴포넌트트리를 사용하여 곧바로 컴포넌트로 전달하는 방식을 제공

→컴포넌트들이 데이터에 쉽게 접근가능함

Context를 사용할때 주의할 점

Context다른 레벨의 많은 컴포넌트들이 특정 데이터를 필요로 하는경우에 사용

그러나 무분별한 context사용은 지양해야함

→ 컴포넌트와 context가 연동되면 재사용성이 떨어지기때문

→컴포넌트가 많지않다면 기존의 컴포넌트 컴포지션방법이 효율적이다.

Context는 어떻게 사용해야할까?

### Comtext API

Context를 사용하려면 당연히 Context를 생성해야한다.

→ Context를 생성하기위해 React.createContext()함수를 사용한다.

Context생성

```jsx
const MyContext = React.createContext(기본값);
```

이후 Context객체가 생성됨

→리액트에서 렌더링이 일어날때 Context객체를 구독하는 하위컴포넌트가 나오면 현재 Context값을 가장 가까이에있는 상위레벨의 PROVIDE로 부터 받오게됨

→상위레벨의 PROVIDE가 없으면 기본값이 적용

이제 하위컴포넌트들이 해당 Context데이터를 받을 수 있도록 설정해야함

→ Comtext.Provider 사용

=데이터를 제공해주는 컴포넌트

Provider 사용

```jsx
<MyContext.Provider = value = {/* some value*/}>
```

Provider 컴포넌트에는 value라는 prop이 존재, 이것은 Provider 컴포넌트 하위에 있는 컴포넌트들에게 전달됨.

하위컴포넌트들이 이 값을 사용할때 이들이 데이터를 소비한다는 뜻으로 컨슈밍 컴포넌트라고 부른다.

컨슈밍 컴포넌트는 context값의 변화를 지켜보다 값이 변경되면 재렌더링 됨

(하나의 Provider 컴포넌트는 여러개의 컨슈밍 컴포넌트와 연결될 수 있으며

여러개의 Provider 컴포넌트는 중첩되어 사용될 수 있음.)

### Provider value에서 주의해야 할 사항

context는 재렌더링 여부를 확인할때 레퍼런스 정보를 사용한다.

provider의 부모 컴포넌트가 재렌더링되었을 경우 컨슈머 컴포넌트가 재렌더링 될 수 있다.

이를 방지하기위해

→value를 직접 넣지않고 컴포넌트의state로 옮기고 해당 state의 값을 넣어준다.

### Class.contextType

=provide 하위에 있는 클래스컴포넌트에서 context의 데이터에 접근하기위해 사용한다.

컨슈머 컴포넌트는 context의 데이터를 구독하는 방식

함수컴포넌트에서는 Context.Consumer를 사용하면됨

### Context.Consumer

사용방법

```jsx
<MyContext.Consumer>
		{value => /*컨텍스트의 값에 따라서 컴포넌트들을 렌더링 */}
</MyContext.Consumer>
```

컴포넌트의 자식으로 함수가 올 수 있다.

MyContext.Consumer로 감싸주면 자식으로 들어간 함수가 현재 컨텍스트의 value를 받아서 리액트 노드로 리턴하게됨 

함수로 전달되는 value는 provide의 value prop과 동일

상위컴포넌트의 provide가 없다면 vlaue파라미터는 React.createContext(기본값);의 기본값과 동일.

### function as a child

컴포넌트의 자식으로 함수를 사용하는 방법

```jsx
//children이라는 props를 직접 선언하는 방식
<Profile children = { name => <p>이름: {name}</p>) />

//propfile컴포넌트로 감싸서 children으로 만드는 방식
<Profile>{name => <p>이름: {name}</p>}</Profile>
```

리액트에서는 기본적으로 하위컴포넌트들을 칠드런이라는 prop으로 전달해주는데 칠드런으로 컴포넌트대신 함수를 사용하여 위 코드와 같이 사용가능

### Context.displayName

컴포넌트 객체는 Context.displayName이라는 문자열속성을 가진다.

또한 크롬의 리액트 개발자 도구에선 Context의 provider나 consumer를 표시할때 이 디스플레이 name을 함께 표시한다

### 여러개의 Context를 동시에 사용하려면 어떻게 해야할까?

 Comtext.Provider를 중첩하여 사용하면됨

함수컴포넌트에서 context사용하는 방법을 아는것이 더 중요함 (클래스컴포넌트를 잘 사용하지않음)

### useContext()

함수컴포넌트에서 context를 사용하기 위해 useContext()훅을 사용하는것

useContext() 훅은 함수컴포넌트에서 context를 쉽게 사용할 수 있게 해줌

다음과 같이 사용

```jsx
function MyComponent(orios) {
		const value - useContext(MyContext);

		return (
		    ...
		)
}
```

컨텍스트 객체를 인자로 받고 현재 컨텍스트값을 리턴함

# Styling

### 레이아웃과 관련된 속성

### display

1.display: none;

-엘리먼트를 화면에서 숨기기위해 사용

-script태그의 display속성 기본값은 display: none;

2.display: block;

-블록 단위로 엘리멘트를 배치

-p, div, h1~h6태그의 display속성 기본값이 display: block;

3.display: inline;

-엘리멘트를 라인 안에 넣는 것

-span태그의 display 속성 기본값이 display: inline;

4.display: flex;

-엘리먼트를 블록 레벨의 flex container로 표시

-container이기 때문에 내부에 다른 엘리먼트들을 포함

### visibility

1.visibility: visible;

-엘리먼트를 화면에 보이게 하는 것

1.visibility: hidden;

-화면에서 안보이게 감추는 것

-엘리먼트를 안 보이게만 하는 것이고 화면에서의 영역은 그대로 차지

### position

1.static

-기본값으로 엘리먼트를 원래의 순서대로 위치시킴

2.fixed

-엘리먼트를 브라우저 윈도우에 상대적으로 위치시킴

3.relative

-엘리먼트를 보통의 위치에 상대적으로 위치시킴

4.absolute

-엘리먼트를 절대 위치에 위치시킴

### flex-direction

1.row

-기본값이며 아이템을 행(row)을 따라 가로 순서대로 왼쪽부터 배치

2.column

-아이템을 열(column)을 따라 세로 순서대로 위쪽부터 배치

3.row-reverse

-아이템을 행(row)의 역(reverse) 방향으로 오른쪽부터 배치

4.column-reverse

-아이템을 열(rcolumnw)의 역(reverse) 방향으로 아래쪽부터 배치

### align-items

이때 정렬시 크로스 엑시스를 기준으로 한다.

1.stretch

-기본값으로써 아이템을 늘려서(stretch) 컨테이너에 가득 채움.

2.flex-start

-크로스 엑시스의 시작 지점으로 아이템을 정렬

3.center

-크로스 엑시스의 중앙으로 아이템을 정렬

4.flex-end

-크로스 엑시스의 끝 지점으로 아이템을 정렬

5.baseline

-아이템을 baseline 기준으로 정렬

### justify-content

어떻게 나란히 맞출것인가를 → 메인엑시스를 기준으로함

1.flex-start

-메인엑시스의 시작지점으로 아이템을 맞춤

2.center

-메인엑시스의 중앙으로 아이템을 맞춤

3.flex-end

-메인엑시스의 끝 지점으로 아이템을 맞춤

4.spece-between

-메인엑시스를 기준으로 첫 아이템은 시작 지점에 맞추고

마지막 아이템은 끝 지점에 맞추며 중간에 있는

아이템들 사이(between)의 간격(spece)이 일정하게 되도록 맞춤.

5.spece-around

-메인엑시스를 기준으로 각 아이템의 주변(around) 간격(spece)을 동일하게 맞춤

## 폰트와 관련된 속성

-font-family

-font-size

-font-weight

-font-style

등등이 있다.

### font-family

어떤 글꼴을 사용할것인지 정하는 속성

일반적인 글꼴분류(generic font family)

-serif

각 글자의 모서리에 작은 테두리를 갖고있는 형태의 글꼴

-sans-serif

모서리에 테두리가 업싱 깔끔한 선을 가진 글꼴

컴퓨터 모니터에서는 Serif보다 가독성이 좋음

-monospace

모든 글자가 같은 가로 길이를 가지는 글꼴. 코딩을 할 때 주로 사용

-cursive

사람이 쓴 손글씨 모양의 글꼴

-fantasy

장식이 들어간 형태의 글꼴

### font-size

글꼴의 크기와 관련

px 

em

rem

vw

### font-weight

글꼴의 두께와 관련

값으로는

nomal

bold

등을 사용하며

100~900까지 백단위의 숫자의 값을 사용할 수 있음

### font-style

-nomal

일반적인 글자의 형태를 의미

-italic

글자가 기울어진 형태로 나타남

별도로 기울어진 형태의 글자들을 직접 디자인해서 만든 것

-oblique

글자가 비스듬한 형태로 나타남

## 기타 많이 쓰이는 속성들

### background-color

배경색 지정을 위한 속성

색상의 값을 넣어 사용

ex)16진수 ff0000 , 투명도는 뒤에 추가로 두자리수 , rgb , 색상이름 등등

## Styled-components(중요)

css문법을 그대로 사용하면서 결과물을 스타일링된 컴포넌트 형태로 만들어주는

오픈소스 라이브러리이다.

### 기본사용법

tagged template literal를 사용하여 구성요소의 스타일을 지정함

template literal → js에서 제공하는 문법중 하나

이것을 알기전 literal의 개념에 대하여 알아야 함

literal → 소스코드의 고정된 값을 의미

다시돌아와서

template literal은 literal를 template 형태로 사용하는 js의 문법

→ untagged template literal과 tagged template literal로 나뉨

tagged template literal를 사용하면 문자열과 expression을 펑션의 파라미터로 넣어 호출한결과를 받을수 있다.

## Styled-components 사용예시

```jsx
import React from "react";
import styled from "styled-components";

const Wrapper = styled.div`
		padding: 1em;
		background: grey;
;
```

.Styled-components는 tagged template literal를 사용하여 css속성이 적용된 리액트 컴포넌트를 

만들어 준다.

Styled-components를 사용하는 기본적인 방법은 백티스로 둘러싸인 물자열 부분에 css속성을 넣고

태그함수 위치에는 위와 같이 styled.html태그형태로 사용함

→해당 엘리먼트 태그에 css속성들이 적용된 형태에 리액트 컴포넌트가 만들어짐

### Styled-components의 props 사용하기

조건이나 동적으로 변하는 값을 사용해서 스타일링 하기위해 props를 사용한다

→리액트 컴포넌트의 props와 같은 개념으로 생각해도 됨