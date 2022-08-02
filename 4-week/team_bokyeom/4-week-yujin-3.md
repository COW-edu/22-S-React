# COW STUDY

### 4주차 - 3 Notion 정리

## Event

: 특정 사건을 의미 

→ ex) 버튼 클릭 이벤트 : 사용자가 버튼을 클릭한 사건

## Event Handling

: Event를 처리하는 것

### ● Dom의 Event와 리액트의 Event 차이점

![E4.jpg](COW%20STUDY%20f5e166eeafdb4628a8152054a248647b/E4.jpg)

→ 리엑트는 Camel표기법을 사용하고, dom에서는 event를 처리할 함수를 문자열로 전달하지만, react에서는 함수 그대로 전달한다.

### ● camelCase (카멜 표기법)

: 첫글자는 소문자, 중간에 나오는 새로운 단어의 첫글자는 대문자로 사용하여 글자의 크기가 변하는 모습이 마치 낙타의 등과 같다고 해서 지어진 이름

## Event Handler (= Event Listener)

: 이벤트가 발생했을 때 해당 이벤트를 처리할 함수 (어떤 사건이 발생하면 처리하는 역할)

### ● 함수 컴포넌트에서 이벤트를 처리하는 방법

![E3.jpg](COW%20STUDY%20f5e166eeafdb4628a8152054a248647b/E3.jpg)

→ 함수 컴포넌트에서 이벤트를 처리할 때 this를 사용하지 않고 onclick에 곧바로 정의한 event handler를 넘기면 된다.

## Argument (= Parameter)

: 함수에 주장할 내용 (함수에 전달할 데이터 → Event Handler에 전달할 데이터)

## Arguments 전달하기

: ex) 특정 사용자 프로필을 클릭했을 때 해당 사용자의 id를 매개변수로 전달해서 정해진 작업을 처리해야 되는 경우

### ● 함수 컴포넌트에서 이벤트 핸들러에 매개변수를 전달할 때

![E2.jpg](COW%20STUDY%20f5e166eeafdb4628a8152054a248647b/E2.jpg)

→ 매개 변수의 순서는 원하는대로 설정 가능