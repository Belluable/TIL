# TypeScript

## TypeScript

- 2012년에 JS를 위해 개발됨
- React TS - 2020

**구성**

1. 프로그래밍 언어
2. 타입 검사기(TypeChecker): 오류 감소
3. 컴파일러(Pre-Compiler): TS → JS
4. 언어지원 서비스(IDE)

⇒ JS에 타입을 부여하고, 부여된 타입을 체크하고, 만족하는 JS 코드를 생성한다.

### TypeScript Compiler (면접 ⭐)

1. Read TSConfig (tsconfig.json): 너가 어디까지 타입을 체크할거니
2. Pre-process Files: follow imports (가능한 모든 파일)
3. Tokenize(Scan) and Parse: text → syntax tree
4. Binder: scope 별로 바인딩
5. Type Check
    1. inference
    2. assignability
6. Transform
7. Emit

### install

```bash
npm init -y
npm install -g typescript ts-node
tsc -v
tsc --init

tsc hello.ts  # js 파일 만듦, tsc만 입력해도 됨
node hello.js  # 파일 실행

ts-node hello.ts  # js 파일 안만듦, ts로 실행해서 결과 보기
```

### 타입의 종류

- 타입이 인식하는 원시 타입(primitive type) → JS 7가지 원시타입과 동일
- TS는 JS를 포함하는 슈퍼셋(상위확장)이므로 JS가 지원하는 타입 그대로 사용 가능
1. null
2. undefined
3. boolean
4. string
5. number
6. bigint
7. symbol

## 타입 시스템

- 프로그래밍 언어가 프로그램에서 가질 수 있는 타입을 이해하는 방법에 대한 규칙 집합

### 타입 스크립트의 타입 시스템이 코드를 이해하는 방법

1. 코드를 읽고 존재하는 모든 타입과 값을 이해
2. 각 값이 초기 선언에서 가질 수 있는 타입 확인
3. 각 값이 추후 코드에서 어떻게 사용될 수 있는지 모든 방법을 확인
4. 값의 사용법이 타입과 일치하지 않으면 사용자에게 오류 표시

### 타입 추론 과정

```tsx
let firstName = "Tom"; 
firstName.length();

// Error : This expression is not callable.
// Type 'Number' has no call signatures.
```

타입스크립트가 오류를 표시하는 순서

1. 코드를 읽고 firstName이라는 변수 이해
2. 초깃값이 “Tom”이므로 firstName은 String 타입이라고 결론 지음
3. firstName의 .length멤버를 함수처럼 호출하는 코드 확인
4. string의 .length멤버는 함수가 아닌 숫자라는 오류를 표시

⇒ 함수처럼 호출할 수 없음

### 구문(Syntax)오류 vs 타입 오류

- 구문 오류: TS가 JS로 변환되는 것을 차단한 경우 = JS Syntax Error
- 타입 오류: 타입 검사기에 다라 일치하지 않는 것이 감지된 경우 = TS check system Error

### Type Annotation

- 진화하는 any
- 초기값을 할당하지 않고도 변수의 타입을 선언할 수 있는 구문
