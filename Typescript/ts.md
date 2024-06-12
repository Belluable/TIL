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
    3. structurally typed (Symbol Table)
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

---

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

---

## 유니언과 리터럴

### 타입 별칭(Type Alias)

```tsx
type User = { id: number; name: string; addr?: string };
const user: User = { id: 1, name: 'Alice' };
user.addr = 'Seoul';

type Emp = { id: number; name: string; dept: string | number };
const emp: Emp = {
  id: 1,
  name: 'Kim',
  dept: 'Sales',
};
emp.dept = 12;
```

### 리터럴 타입

- 원시 타입보다 더 구체적인 원시 타입, 원시값 자체가 타입이 됨
- 서로 다른 리터럴 타입은 서로 할당 X

```tsx
const Y = 'y'; // y타입
let Z: 'x' | 'y' | 'z' = 'x'; // literal 타입
Z = 'y';
Z = Y;
let N: 1 | 2 | 3 = 3;
```

### 유니언 타입

- 값에 허용되는 타입을 2개 이상의 가능한 타입으로 확장하는 것
- A or B ??? 둘 이상의 타입 중 하나가 아니다 (개 중 하나)
- 둘 이상의 타입으로 확장된 타입에서 일부 속성들의 조합이 어느 하나의 타입에 할당 가능하면 OK
    
    ![ts_1](ts/1.png)
    
- 유니언으로 선언한 모든 타입에 존재하는 공통되는 속성에만 접근 가능
    
    ![ts_2](ts/2.png)
    

- 유니언 타입에서 특정 타입에만 존재하는 속성에 접근하고 싶다면?
    
    → type guard를 통해 type narrowing
    
- narrowing: 값이 더 구체적인 타입임을 코드에서 유추하는 것
- type guard: narrowing을 하기 위한 논리적인 검사
    1. 값 할당을 통한 내로잉 (x = 1)
    2. typeof 검사를 통한 내로잉
    3. 조건 검사를 통한 내로잉(if x === ‘stringValue’)
    4. in (cf. hasOwnProperty 는 X)
    5. instanceof
    6. Array.isArray (불규칙 허용)
- typeof narrowing 주의: 검사되는 property만 narrowing (who.spend의 타입이 number[] 타입이라고해서 who의 타입이 Member로 narrowing된 것이 아님!)

### strictNullChecks

- null 혹은 undefined 값을 참조/할당 했을 때 타입 에러 발생 여부
    
    ⇒ 10억 달러의 실수 (NullPointerException) ⇒ `‘strict’: true`
    
- 활성화: null 및 undefined에 대한 오류로 부터 안전 →  항상 `‘strict’: true`로 해놓자
- 비활성화: `‘strict’: false` → null/undefined를 찾을 수 없음!!
    
    ⇒ JS와 같아짐 = TS 쓰는 이유 없어짐
    
    ```tsx
    let a: string | undefined;
    console.log(a?.length);
    console.log(a.length); // error
    ```
    

---

## 객체 타입

### freshness

- 아직 가공이 안된 상태, 쓸 수 없음 (ex. 흙 당근)
- freshness 끄는 방법:
1) 변수할당 
2) type casting (type assertion) 
3) union으로 체크에서 제외
4) 함수 통하기 (return)

```tsx
let xuser: {id: number, name: string};
xuser = {id: 1, name: 'xx'}; // OK
xuser = {id: 1}; // Error (Property 'name' missing in type)
xuser = {id: 1, name: 'xx', age: 30}; // Error ({id, name, age} is not assignable to type {id,name} )
// freshness: xuser에 age 없음
```

### Type check system

- Structurally Typed (Symbol Table) 
= Identifier + TypeKeyword + Properties
(Type Literal, Property Signature)
- Type Comparer & Inference System ⇒ Assignable
- Object Assignability
    - Exact Matching
    - Covariance or Bivariance (when freshness off)

### CoVariance & ContraVariance

- 함수의 스펙트럼이 더 큰 놈을 줘야 할당 가능
- CoVariance: 원래 지정된 것보다 더 많이 파생된 형식을 사용할 수 있다.
- ContraVariance : 원래 지정된 것보다 더 제네릭한(덜 파생적인) 형식을 사용할 수 있다.
- TS는 CoVariance

```tsx

type Xuser = {id: number; name: string};
type Xemp = {id: number; name: string; addr: string};
let xuser: Xuser;
let xemp : Xemp;

xuser = {id:1, name: 'hong'};
xemp = {id:1, name: 'lee', addr: 'Pusan'};

xuser = xemp; // CoVariance:(id, name) <= (id, name, addr)
xemp = xuser; // Error! ContraVariance: (id, name, addr) <= (id, name)
```

---

## 함수 타입

### 함수 매개변수

- TS는 함수에 선언된 모든 매개변수가 필수라고 가정함 → 타입 안정성 강화
- 선택적 매개변수
    - 타입 애너테이션의 `:` 앞에 `?`를 추가해 매개변수가 선택적이라고 표시
    
    ```tsx
    const introduce = (name: string, height?: number) => {
      console.log(`이름: ${name}`);
      if (typeof height === 'number') console.log(`키: ${height + 10}`);
    };
    
    introduce('sue');  // OK
    introduce('sue', 160);  // OK
    introduce('sue', undefined);  // OK
    ```
    
    - optional parameter(? 붙은애들)은 중간에 올 수 없음 → 맨 끝에!
- 나머지 매개변수
    - tupule로 개수 지정:  `rest: [number, number]`

### 함수의 반환 타입

- 함수가 반환할 수 있는 가능한 모든 값을 이해하면 함수가 반환하는 타입을 알 수 있다.
- 명시적 반환 타입:
    - 가능한 반환값이 많은 함수가 항상 동일한 타입의 값을 반환하도록 강제함
    - TS는 재귀함수의 반환 타입을 통해하는 타입 유추 거부함
    - 큰 프로젝트에서 타입 검사 속도 향상
    - 함수 선언 반환 타입 애너테이션은
        - 매개변수 목록이 끝나는 `)` 다음에 배치됨
        - 함수 선언 시:  `{` 앞에 배치됨
        - 화살표 함수: `=>` 앞에 배치됨
- 함수 매개변수 타입
    - 함수 타입 구문은 화살표 함수와 유사하지만 함수 본문 대신 타입이 있음
    - 함수 타입은 함수 매개변수(함수로 호출되는 매개변수)를 설명하는 데 자주 사용됨
    
    ```tsx
    //nothingGivesString 변수 타입은 매개변수가 없고 string 타입을 반환하는 함수
    let nothingGivesString : () => string;
    
    //inputAndOutput 변수 타입은 String[] 매개변수와 count 선택적 매개변수 및 number를 반환하는 함수
    let inputAndOutput : (songs: string[], count?: number) => number;
    ```
    
- 함수 타입 괄호
    - 함수 타입은 다른 타입이 사용되는 모든 곳에 배치할 수 있음. 유니언 타입 포함
    - 유니언 타입의 애너테이션에서 함수 반환 위치를 나타내거나 타입을 감싸는 부분을 표시할 때 괄호를 사용
    
    ```tsx
    // 타입은 string | undefined 유니언을 반환하는 함수
    let returnsStringOrUndefined :() =>string | undefined;
    
    //타입은undefined나 string 을 반환하는 함수
    let maybeReturnsString : (() =>string) | undefined;
    ```
    
- void 반환 타입
    - 일부 함수는 어떤 값도 반환하지 않을 수 있음 → 타입스크립트는 void 키워드를 사용해 반환값이 없는 함수의 타입을 확인할 수 있음
    - 함수 타입 선언시 void 반환 타입은 매우 유용!
    함수 타입을 선언할 때 void를 사용하면 함수에서 반환되는 값은 모두 무시됨**! (in TS)** ⇒ **"**리턴 값을 쓰지 마**!"**
    - undefined와 void를 구분: void를 반환하도록 선언된 타입 위치에 전달된 함수가 반환된 모든 값을 무시하도록 설정할 때 유용!
    
    ```tsx
    // function returnVoid: void
    function returnVoid() {
      return;
    }
    
    let lazyValue : string | undefined;
    
    lazyValue = returnVoid();
    
    // Type 'void' is not assignable to type 'string | undefined'.
    //-> undefined를 포함하는 대신 void 타입의 값을 할당하려고 하면 타입 오류 발생
    ```
    
    - void 타입은 자바스크립트가 아닌 함수의 반환 타입을 선언하는데 사용하는 타입스크립트 키워드!
    - void 타입은 함수의 반환 값이 자체적으로 반환될 수 있는 값도 아니고, 사용하기 위한 것도 아님!!
    
    ```tsx
    const records : string[] = [];
    
    // function saveRecords(newRecords : string[]): void
    function saveRecords (newRecords : string[]){
        newRecords.forEach(record =>
                    records.push(record));
    }
    
    saveRecords(['Go', 'Walk', 'Run']);
    
    ```
    
- never 반환 타입
    - **(**의도적으로**)** 항상 오류를 발생시키거나 무한 루프를 실행하는 함수
    - 함수가 절대 반환하지 않도록 의도하려면 명시적 `: never` 타입 애너테이션을 추가해 해당 함수를 호출한 후 모든 코드가 실행되지 않음을 나타냄.
    - never와 void의 차이
        - never: 끝나지 않는 함수를 위한 것
        - void: 아무것도 반환하지 않는 함수를 위한것
    
    ```tsx
    // never를 생략하면 void 타입!
    function fail(message : string) : never {
        throw new Error(`Invariant Failure : ${message}`);
    }
    
    function workWithUnsafeParam(param : unknown){
        if(typeof param !== "string"){ // unknown은 narrowing해야 사용 가능
            fail(`param should be a string, not ${typeof param}`);
        }
    
        // 여기에서 param의 타입은 string으로 알려짐
        param.toUpperCase();
    }
    
    ```
    

### 함수 오버로드

- 동일한 이름에 매개변수만 다른 여러 버전의 함수를 만드는 것
- 호출 시그니처가 다르면 서도 다른 함수처럼 다르게 실행하고 싶은거

```tsx
// 서로 다른 버전의 함수들 -> `오버로드 시그니처`
function func(a : number): void;
function func(a : number, b : number, c : number) : void;

// 실제 구현부 -> `구현 시그니처`
function func(a:number, b?: number, c?:number) {
  if(typeof b === 'number' && typeof c === 'number') {
    console.log(a + b + c);
  } else {
    console.log(a * 20);
  }
}

func(1);  // 마치 다른 함수인 것 처럼
func(1,2,3);
```

### this

- this의 타입을 정할 때는 함수의 첫 번째 매개변수 자리에 `this`를 쓰고 타입을 입력한다.
- 매개변수와 같이 this의 타입을 적어주지만 실제로 인자값을 받는 매개변수는 `this: 타입`을 제외한 나머지임으로 헷갈리면 안된다.

```tsx
function 함수명(this: 타입) {
  // ...
}
```

---

## 배열과 튜플

- 배열과 함수 타입
    
    ```tsx
    // string 배열을 반환하는 함수
    let getNames: () => string[];
    // string을 반환하는 함수들의 배열
    let nameGetters: (() => string) [];
    ```
    
- 유니언 타입 배열
    
    ```tsx
    // string 또는 number 배열
    let stringOrArrayOfNumber: string | number[];
    // string 또는 number를 요소로 갖는 배열
    let ArrayOfStringOrNumber: (string | number)[];
    ```
    
- any[]: 미정/미확정 타입, any로 고정되면 계속 any
- 배열 멤버
    
    ```tsx
    let onlySting: string[] = [];
    ```
    

### noUncheckrdIndexedAccess

- 해당 index의 값이 undefined일 수 있음을 체크
- 이 옵션을 true로 설정하면 인덱스 시그니처에 undefined를 포함
- 인덱스 시그니처?
    - 받을게 너무 많을 때 줄여서 쓸 수 있음
    - 웬만하면 다 적어주는게 좋음
    
    ```tsx
    // 인덱스 시그니처
    type NaverKakaoUser = {
      id: number;  // id도 포함되지만 필수로 들어가니까 따로 빼주면 좋음
      // nickname: string;
      // name?: string;
      // email: string;
      [k: string]: string | number; // 인덱스 시그니처, id 때문에 number 적어줘야함 
    };
    
    const KUser: NaverKakaoUser = { id: 1, nickname: 'hong', age: 12 };
    
    ```
    
- type vs interface?
    - interface: 규칙 정해버림, 적어뒀던 애들은 다 포함해줘야함
    - type: 단순히 이런 타입이다
    - TS가 compile할 때 parsing하는데 type은 T 자리에 다 복사되고 interface는 class랑 똑같이 작동됨(참조)
    
    ```tsx
    type T = { id: number; getId: () => number };
    interface I {
      id: number;
      getId: () => number;
    }
    
    class IxUser implements I {
      id = 1;
      getId() {
        return this.id;
      }
    }
    
    const ix1 = new IxUser();
    console.log(ix1.id, ix1.getId());
    
    const ix2: T = { id: 2, getId: () => 2 };
    ```
    

### 스프레드

- 두 배열을 결합하면 어떤 타입이 될가?
    - concat은 결합하려는 두 배열의 타입이 같아야한다.
    - 서로 다른 타입의 두 배열을 결합하려면 → spread!
    
    ```tsx
    const nums1 = [1, 2, 3, 4, 5]; // number[]
    const nums2 = [10, 20, 30, 40, 50]; // number[]
    const strings1 = ['lim', 'eun', 'ha'];  // string[]
    
    const result1 = nums1.concat(nums2);  // 당연히 result1은 number[]
    const result2 = result1.concat(strings1);  // Error! concat은 타입이 같아야함
    const result2 = [...result1, ...strings1];  // 스프레드 연산자 사용
    ```
    

### 튜플

- 고정된 크기의 배열
- 각 인덱스에 알려진 특정 타입을 갖는다
- `let tuple:[number, string, boolean];`

```tsx
//let tuple1: [number, string, boolean];
type IdNameDidoutAddr = [number, string, boolean, string];  // 확장성 고려
let tuple1: IdNameDidoutAddr;
tuple1 = [2, 'a', false, 'Seoul'];

function tf(id:number, name:string, didOut:boolean, addr: string){
  console.log(arguments); // arguments는 유사배열객체
}
tf(1, 'a', false, 'Seoul');

function tf2(params: IdNameDidoutAddr) {
  console.log(params); 
}
tf2([1, 'a', false, 'Seoul']);
tf2(tuple1);
```

- 튜플 추론
    - 명시적 튜플 타입
- const assertion
    - 읽기 전용 튜플로
    - primitive, object 타입의 값 뒤에 `as const` 연산자가 붙으면 값 자체가 타입이 됨
    - object 타입의 값 뒤에 `as const` 연산자가 붙으면 read-only 타입이 됨
    - 배열상수 = tuple + readonly
    - `enum` 쓰지 말자
- 타입 선언에서의 spread와 tuple
    
    ```tsx
    type TA = [string, number];
    const ta1: TA = ['a', 1];
    
    type TB = [boolean, TA];  // [boolean, [string, number]]
    type TB2 = [boolean, ...TA];  // [boolean, string, number]
    const tb1: TB = [true, ta1];
    const tb2: TB2 = [true, ...ta1];
    
    type AA = (string | number)[];
    type CC = [boolean, AA];  // [boolean, (string | number)[]]
    const cc1: CC = [true, ['a', 1]];
    ```
    

> Ex) sizeOption[size.id]에서 오류가 나는 이유는?
> 

→ SIZE가 고정되지 않아 언제든 바꿀 수 있어서

→ `const SIZE = [ … ] as const;`  as const를 붙여서 고정

```tsx
const SIZE = [
  { id: 'XS', price: 8000 },
  { id: 'S', price: 10000 },
  { id: 'M', price: 12000 },
  { id: 'L', price: 14000 },
  { id: 'XL', price: 15000 },
] as const; // assertion readonly로 바꿔서 다른 값은 못들어가게

const sizeOption = { XS: 1, S: 5, M: 2, L: 2, XL: 4 };
const totalPrice = SIZE.reduce(
  (currPrice, size) => currPrice + sizeOption[size.id] * size.price,
  0
);
```

### UUID (Universally Unique Identifier)

- 네트워크 상에서 고유성이 보장되는 id를 만들기 위한 표준 규약
- `npm i uuid`

```jsx
// ES6
import { v4 as uuidv4 } from 'uuid';
uuidv4(); // ⇨ '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'

//CJS
const { v4: uuidv4 } = require('uuid');
uuidv4(); // ⇨ '1b9d6bcd-bbfd-4b2d-9b5d-ab8dfbbd4bed'
```

---

## 인터페이스 (`interface`)

- `interface` 같은 이름이 허용됨 vs `type` 은 같은 이름 안됨!!
- 속성 타입
    - 선택적 속성 (optional property)
    - 읽기 전용 속성 (readonly property)
- 함수와 메서드
    - 메서드 구문 (method syntax)
    - 속성 구문 (property syntax)
- 호출 시그니처
    - 함수를 호출하는 방법에 대한 타입 시스템 설명
    - 매개변수 목록(with type)과 반환 타입을 포함
    - `(name: string, age: number, addr: string[]) => boolean`
- 인덱스 시그니처
    - 임의의 key를 받고, 해당 key에 대한 value의 타입을 지정함
    - `[key: string]: number`
    - 주의!! 인덱스 시그니처를 사용하면 프로퍼티의 존재 유무를 알 수 없다! `noUncheckedIndexedAccess`
    - 속성과 인덱스 시그니처 혼합
        
        ```tsx
        interface Novel {
          title: string; // 필수 속성 (실제 사용할 속성)
          [key: string]: string | number | boolean;
        }
        
        const novel: Novel = {
          title: 'novel',
          page: 130,
        };
        ```
        
    - 숫자 인덱스 시그니처
- 중첩 인터페이스 (nested interface)
    - 속성의 타입이 다른 인터페이스(or 자체 인터페이스)나 객체 타입을 가질 수 있다.
- interface 확장 (extends) vs type 확장 (&)
    
    ```tsx
    // interface 확장
    interface A {
        id: number;
    }
    
    // B has id + name
    interface B extends A {  // A type alias라도 가능!
        name: string;
    }
    
    // Type alias로 extends 하기 
    type A = {
        id: number;
    }
    
    type B = { 
        name: string;
    } & A;
    
    let hong: B = {
        id: 1, name: 'Hong'
    };
    ```
    
- interface 타입 재정의 (type override)
    - 부모 타입 > 자식 타입 ⇒ 통과!
- interface 병합 활용
    
    ```tsx
    declare global {
      // Array는 내가 안만든 거니까
      interface Array<T> {
        // Array interface 병합
        first(): T;
        mapBy: (prop: string) => any;
      }
    }
    
    // mapBy
    Array.prototype.mapBy = function (prop: string) {
      return this.map((a) => a[prop]);
    };
    
    const users = [
      { id: 1, name: 'Hong' },
      { id: 2, name: 'Kim' },
    ];
    console.log(users.mapBy('name')); // ['Hong', 'Kim']
    
    export {};
    ```
    

> Ex) `type Ud2 = (TUser | TDept) & {addr: string};` 을 interface로 어떻게 정의할까?
> 

```tsx
interface User2 {
  id: number;
  name: string;
}

interface Dept {
  id: number;
  dname: string;
  captain: string;
}

interface Ud2 {
  // <이 부분을 작성하시오>
  id: number;
  name?: string;
  dname?: string;
  captain?: string;
  addr: string;
}

// 다음 코드가 오류가 없으면 통과!
const ud2: Ud2 = { id: 1, name: 'HH', addr: 'Seoul' };
const ud3: Ud2 = { id: 1, dname: 'HH', captain: 'HH', addr: 'Seoul' };
```