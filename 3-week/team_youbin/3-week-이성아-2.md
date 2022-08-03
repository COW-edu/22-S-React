- SPA& MPA
    
    - SPA
        
        Single Page Application의 약자
        
        하나의 페이지만 존재하는 웹 사이트 또는 웹 애플리케이션
        
        정적 리소스를 최초에 한 번에 다운로드한다.
        
        그 후 새로운 페이지 요청이 있을 경우, 
        
        페이지 갱신에 필요한 데이터만전달받아 페이지를 갱신
        
    - MPA
        
        Multi Page application의 약자
        
        여러 페이지가 존재하는 웹 사이트 또는 웹 애플리케이션
        
        새로운 페이지를 요청할 때마다 정적 리소스가 다운로드 되고,
        
        그에 맞춰 전체 페이지를 다시 렌더링

- 자료형 : Data Type
    
    1 Number Type : 
    
    숫자를 다루기 위한 자료형
    
    2 String Type : 
    
    문자열을 다루기 위한 자료형
    
    큰따옴표나 작은따옴표를 이용
    
    3 Boolean Type : 
    
    true, false 두 가지로만 이루어진 자료형
    
    4 Null Type : 
    
    의도적으로 값이 없음을 명시하기 위한 기본 자료형
    
    객체가 아니다.
    
    5 Undefined Type : 
    
    정의되지 않은 자료형
    
    6 Array Type :
    
    배열을 나타내는 자료형
    
    다양한 자료형의 변수가 함께 들어갈 수 있다.
    
    각 변수는 순서를 나타내는 Index 값을 0, 1, 2 … 로 가진다.
    
    7 Object Type :
    
    객체를 다루기 위한 자료형
    
    - Java Script에서의 객체
    
    Key와 Value로 이루어진 쌍의 집합

- 연산자 : Operator
    - 대입 연산자 (Assignment operator)
    
    : =을 사용
    
    대입 연산자는 항상 오른쪽에서 왼쪽으로 흐름이 흘러간다.
    
    - 산술 연산자 (Arithmetic operator)
    
    : +(Plus), -(Minus), *(Multiplication), /(Division), %(Remainder)
    
    - 증가 연산자(++)
    - 감소 연산자(—)
    
    postfix 방식 : a++
    
    증감 전에 값을 반환하고, 이후에 변수의 값이 증감된다.
    
    prefix 방식 : ++a
    
    먼저 변수의 값을 증감시키고, 이후에 값을 반환한다.
    
    - 관계 연산자(Relational operators) = 비교 연산자 (Comparison operators)
    
    : <, >, <=, >=
    
    - 동등 연산자(Equality operators)
    
    : ==, !=
    
    - 일치 연산자(Strict equality operators)
    
    : ===, !==
    
    값과 자료형 모두를 비교한다.
    
    - 이진 논리 연산자(Binary logical operators)
    
    : &&(모두 true일 경우), ||(하나라도 trued일 경우)
    
    - 삼항 연산자(Ternary operator)
    
    : 조건식 ? true일 경우 : false일 경우

- 자바스크립트와 형 변환
    
    ```jsx
    10 + "10"
    "1010"
    ```
    
    Number Type + String Type = String Type

- 함수
    
    : 입력을 받아서 정해진 출력을 하는 것
    
    여기서 입력을 파라미터(parameters) 또는 인자(arguments)라 한다.
    
    1 function statement > function 이용
    
    2 arrow function expression > const 이용
