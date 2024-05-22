# JavaScript


### 메모리 구조

- kernel (os)
- code (mc)
- data (전역/정적) - null, undefined
- stack (primitive) - variable table(변수 테이블) 존재
- heap (reference)

```jsx
// 평가부
var arr;
var x;

// 실행부
arr = [1, 2];
x = 1;
```

![js_1](js/1.png)

---

## 변수와 타입

- 인터프리터 언어: 선언, 정의 동시에 일어남
- 함수형 프로그래밍 = 선언형

### 변수와 상수

- 변수 = 선언 + 식별자 + 타입 + 값 + 스코프
    - 스코프
        - 전역 스코프: 전 지역에서 사용 가능한 변수 (globalThis)
        - 함수 스코프: 특정 함수 내에서 사용 가능한 변수 (var, function)
        - 블록 스코프: 특정 블록 내에서 사용 가능한 변수 (const, let)
    - 타입
        - 값이 저장될 때 정해짐

```jsx
var i = 0;  // var: 함수 scope, i: 식별자
let l = 9;  // block scope
const c = 1;  //상수변수, immutable

function f(){
    var x = 0;
    const c = 1;
    if c -> { let l = 1; }
}
```

### 식별자 규칙

- 문자, $, _ 로 시작
- 유니코드 (utf8)

### mutable/ immutable

- mutable: 메모리 사이즈가 안바뀌는 곳에서  ex. 배열; 주소값을 가진 메모리
- immutable: 변수의 값을 담고 있는 곳

## 변수 타입

**원시형 / 참조형 중요**

- primitive type (value)
    - stack에 존재, LIFO
    - 6가지: 숫자, 문자열/문자열템플릿, boolean, null, undefined, Symbol
    - 문자열, symbol은 constant pool(stack 영역), 나머지 data 영역
    - 메모리에 값이 들어감 → 값이 변경되면 새로운 메모리가 할당됨
- reference type (object)
    - heap에 존재, FIFO
    - 5가지: Array, Date, RegExp, Map/WeakMap, Set/WeakSet
    - new를 쓰면 다 참조형으로 바뀜
    - b=a 하면 같은 메모리 주소를 쓰지만 그 외는 다 주소가 다름
    - == : 값 비교 연산자, === : 값, 주소, type까지 비교 연산자

### stack

- LIFO, 확장 안됨
- stack pointer: 현재 위치를 계속 가르킴, stack pointer를 올리면 pop, 내리면 push
- program counter: 다음에 일어날 일을 확인
- call stack: 함수들의 집합, main → f() → f2()
- 주어진 메모리를 넘으면 stack overflow → limit register

### constant pool

- data 영역에 존재
    - ps. heap은 무제한으로 메모리를 쓸 수 있음 나머지는 한정된 메모리
- data 영역에서 heap을 참조함
- ~~heap에 있는~~
- string, Symbol

---

## Hoisting (호이스팅)

- 평가하기 위해 선언부를 위로 올리는 것 (변수, 함수 다)
- 없는 선언을 만들어준 것부터 시작됨
- ES5 - strict mode: 선언을 안하면 안돌아감: NaN이 안나오게 하기 위해서
- ES6 -  let, const의 필요성이 대두됨, 변수를 정의하고 사용해
- let, const는 초기화가 필요함; 선언과 초기화가 분리 실행됨, init bit(초기화비트) 필요
- var는 다됨 (전역변수), init bit 없음
- JS: 없는거 호이스팅해도 에러 안남 → typeScript: 없는 거 호이스팅하면 에러

**전역객체**

- var
- 노드를 실행하게 되면

**declarative environment record**

- const, let
- 메모리에 더 가까이 올라가니까 더 빠름
- 빠르기: const > let > var

**해싱**

- 암호화
- 해싱 알고리즘: 진수변환 (긴숫자 짧게)

---

## 연산자

- 산술, 할당, 논리, 비교, 삼항조건, 쉽표, 그룹, 지수, 옵셔널체이닝, 널병합, 비트 등
- 객체/ 클래스: instanceof, in, delet, new, …, destructuring

**산술 연산자**: +, -, *, /, %, 부호, ++, —

```jsx
//** 연산
console.log(2 ** 3 ** 2);  // 뒤에서부터 계산 512(2^9)

// ++ 연산
x = 1;
y = x++;  // y: 1, x = 2
                    // y = x -> x = x+1

y = ++x;  // y: 3, x = 3
                    // x = x+1 -> y = x
```

**할당 연산자**: 기본적으로 뒤에거를 줌

```jsx
// 할당연산자: 뒤에거 가져옴
let a = 1, b = 2;
const c = (a++, b++);  // c = b = 2
```

**논리 비교 연산자**

```jsx
s === 'a';
NaN === NaN;  // false
null == undefined   // true -> 값만 비교: 논리연산 상 둘다 falsy 
null === undefined  // false -> 값 + type(메모리) 비교

// 아래 상태에서 계산식에 넣었을 때 쓰이는 값
undefined // NaN
null      // 0

let un; // undefined = NaN
console.log(un + 1); // NaN

let nu = null; // null = 0
console.log(nu + 2); // 1
```

**쉼표 연산자**

```jsx
// 쉼표+그룹 연산자
q = (p = x = 1, y=2, z=3); // (x: 1, y: 2, z: 3, p: 1, q: 3)
```

**nodemon**

```bash
# nodemon을 global하게 깔기
npm i nodemon -g

# nodemon: deamon이어서 안꺼짐
# -d 붙이면: develop할 때만 씀
```

```jsx
// void 연산
d = void(c = a + b)  // 평가/실행 후 undefined 반환
```

**Falsy vs Truthy**

- falsy: undefined, null, false, 0, NaN, ‘’
- truthy: ‘ ‘(빈칸이 있으면 값이 있다고 봄)

### TDZ (Temporary Dead Zone)

```jsx
// 실행부
console.log(l);
let l = 1;

//선언부
let l;
console.log(l);  // error! 초기화 전에 써서
// ---------------- 여기까지 TDZ; l의 데드존
l = 1;

// - const 일때 -
// 실행부
console.log(l);
const l = 1;

//선언부
const l;
console.log(l); 
l = 1;
```

**freshness**

- 초기화 비트 = 0 인 상태→ 바로 사용 못함
    - 흙당근을 쓰려면 씻어야지 바로 쓸 수 없음
- const, let은 초기화 비트가 있음

**비트 연산자**

| 비트연산자 | 기호 | True |
| --- | --- | --- |
| AND | & | 둘다 참 |
| OR | | | 하나만 참 |
| XOR | ^ | 둘다 거짓 |
| NOT | ~ | False |

### 숫자

- parseInt: 문자 → 정수
- parseFloat: 문자 → 실수
- .toFixed: 자리수 지정
- Number.EPSILON: 끝에 붙은 값이 쓰레기 값인가?
- Math.trunc(숫자): 정수부만 남기기
- 엘런튜닝의 한계: 보수의버그

```jsx
// 착각하기 수쉬운 연산
null == 0  //flase
[] == 0    // true
false === !!''  // true
!![]  === !!0   // true = false: false
isFinite(Infinity)  // false
parseInt(null)    // NaN
parseFloat(null)  // NaN
Number(null)  // 0
typeof null   // 'object'
new Number()  // [Number: 0]
isNaN(null)   // false
isNaN('9')    // false
isNaN(dt)     // true: date는 숫자로 저장돼 있음
```

객체/배열 특화 연산자

| 연산자 | 의미 | 사용 |
| --- | --- | --- |
| .(점) | 해당 주소로 가라
키값 불러오기 | u.name ↔ u[’name’]
object의 key 값은 모두 string 이어야함 |
| [](대괄호) |  | [1, 2, 3] |
| in | 안에 있는지 확인 | ‘id’ in u ↔ u.hasOwnProperty(’id’) ↔ Reflect.has(u, ‘id’) |
| new | 생성자
객체 → 인스턴스 | const d = new Dog()
: d는 Dog의 인스턴스 |
| instanceof |  | d instanceof Dog |
| …(rest) | 배정되고 남은 값 다가져옴
잔반처리 | function ff(a, b, …c) {} ⇒ f = (…args) |
| delete | (heap에서) 삭제 | delete u.addr |
| arr?.length | arr이 no error나 undefined면 . 해라 | Optional-Chaining (no error, undefined) |

*객체 vs 인스턴스

인스턴스: 클래스가 메모리에 올라가면 인스턴스 (new로 메모리에 올림)

java vs js

- java : 스레드 하나 문제 생기면 하나만 버리고 다음거 실행
- js: 스레드 하나 문제 생기면 그 프로세스 다 죽음

---

## 제어문

### 조건문

- if 문

```jsx
const n = 2;

// if문
if (n == 1){
    console.log('one');
} else if (n == 2){
    console.log('two');
} else if (n == 3){
    console.log('three');
} else {
    console.log('etc');
}

let outStr = 'etc';
if (n == 1){
    outStr = 'one';
} else if (n == 2){
    outStr = 'two';
} else if (n == 3){
    outStr = 'three';
}
console.log(outStr);
```

- switch-case문

```jsx
// switch
switch(n){
    case 1:
        console.log('one');
        break;
    case 2:
        console.log('two');
        break;
    case 3:
        console.log('three');
        break;
    default:
        console.log('etc');
}
```

- 3항 연산자

```jsx
// 3항 연산자
outStr = n === 1 ? 'one' : n === 2 ? 'two' : n === 3 ? 'three' : 'etc';
console.log(outStr);
```

- OR 연산

```jsx
// || 연산자
// outStr = n === 1 || n === 2 || n === 3 ? ['one', 'two', 'three'][n-1] : 'etc';
outStr = 
    (n === 1 ? 'one' : '') ||
    (n === 2 ? 'two' : '') ||
    (n === 3 ? 'three' : 'etc');
console.log(outStr);
```

### 반복문

- for, while, do-while 문

```
// for문
let s = 0;
for (i = 1; i <= 100; i++){
    s += i;
}
console.log("🚀 ~ s:", s);

// while문
s = 0; i = 1;
while(i <= 100){
    s += i++;
}
console.log("🚀 ~ s:", s);

// do-while문
s = 0; i = 1;
do {
    s += i++;
} while(i <= 100);
console.log("🚀 ~ s:", s);
```

- iterable/iterator 한 배열, string에  `of` 사용 가능

```jsx
// 배열 출력
const arr = [1, 2, 3, 4, 5];
for (let i = 0; i < arr?.length; i++){
    console.log(`🚀 ~ arr[${i}]:`, arr[i]);
}

// of 사용해서 배열 출력
for (const t of arr){
    console.log("🚀 ~ t:", t);
}

// 문자열도 이터러블 객체 -> of 사용 가능
const WeakNames = '월화수목금토일';
for (const c of WeakNames){
    console.log("🚀 ~ c:", c);
}
```

---

## 디스트럭처링 (destructuring: 구조 분해 할당)

### **Object Destructuring**

```jsx
const u = {id: 1, name: 'hong', age: 29};
let {id, name, addr} = u;  // let id = user.id; let addr = undefined;
let {id, ...info} = u;  // id = 1, info = {name: 'hong', age: 29}

let id, name;
{id, name} = u;  // Error! 이미 선언했기 때문에 (괄호)로 싸줘야함
({id, name} = u);  // OK! 위에처럼 선언과 같이해주는게 젤 좋음
```

```jsx
const user = { id: 1, name: 'Hong', addr: { city: 'Seoul', road: '길' } };

// id 가져오기
// user.id = user['id'] 이므로 아래 코드들은 모두 같은 의미
const id = user.id;
const { id: id } = user;
const {id} = user;
const {['id']} = user;
x = 'id';
const {[x]} = user;

// id, addr 가져오기
const { id, addr } = user;
const { id: userId, name: userName } = user;

// addr의 city 가져오기
const { city } = addr; // city = 'Seoul'

// id, addr의 city 가져오기
const { id, addr: { city }, } = user;

// id, name, addr의 city 변수명 바꿔서 가져오기
const {
  id: userId,
  name: userName,
  addr: { city: addrCity },
} = user;

console.log(userId, userName, addrCity); //1 Hong Seoul
```

```jsx
// 배열에서 원하는 것만 가져오기
const arr = [1, 2, 3, 4, 5];
const { 1: x1, 3: x2 } = arr;
console.log('🚀 ~ x1, x2:', x1, x2); //  2 4
```

```jsx
const mainField = user.id > 5 ? 'name' : 'addr'; // 화면 크기에 따른 firstname, lastname 같은거 할 때 많이 사용함
const { [mainField]: target } = user; // target = {city: 'Seoul', road: '길'}
console.log('🚀 ~ target:', target);

target = 'kim'; // TypeError: Assignment to constant variable.
const { name: target } = user; // SyntaxError: Identifier 'target' has already been declared

```

### **Array / Iterator Destructuring**

```jsx
// swap
const [a, b] = [1, 2];
[a, b] = [b, a];  // a: 2, b: 1
```

```jsx
// 부분 발췌
const [ , , x, y, , z] = [1, 2, 3, 4, 5, 6];
console.log(x, y, z);  // 3, 4, 6

// no semi colon error
console.log('no-semi-colon')
[c, d] = [1, 2];  // <- SyntaxError!
```

```jsx
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 3, name: 'Park' },
];
const [, , { id: usrId }] = users;
console.log('usrId:', usrId); // 3

```

### Default Value Destructuring

```jsx
// 초기화
const u = {id: 1, name: 'hong', age: 29};
let {id, name, addr = 'Seoul'} = u;  // addr 없으면 Seoul로 초기화

// 배열 초기화
const [d, e, f = 3] = [1, 2];
console.log(d, e, f); // 1 2 3

const [g, h, o = 3] = [1, 2, 0];
console.log(g, h, o); // 1 2 0
```

```jsx
// 위에 users 사용
const [user1, ] = users;
console.log(user1);

const { id, name, addr = 'Seoul' } = { id: 1, name: 'Lee' };
console.log(id, name, addr); // 1 Lee Seoul

const { id, name = 'Lee' } = { id: 1, name: '' };
console.log(id, name); // 1 ''

const { id, name = 'Lee' } = { id: 1, name: undefined };
console.log(id, name); // 1 Lee
```

```jsx
const obj = {i: 1, j:2, l:3, m:4, n:5};
let {j, i, k = i * j} = obj;  // j = 2, i = 1, k = 2
const {l, m, ...restObj} = obj;  // l = 3, m = 4, restObj = { i: 1, j: 2, n: 5 }

// Error
let {i, j, k = i * j * n} = obj;  // Error! not initialized -> n 선언 안됨
let {k = i * 10, i, j} = obj;  // Error! before initialization -> i 선언이 뒤에 있어서 (freshness 상태)

// 미리 선언하면 undefined 값을 가져서 error 안남
let q, s, r;
({r = q * 10, q, s} = {q: 10, s: 20};
```

```jsx
// spread 연산자: rest 연산자랑 다르게 값으로 쓰일때 -> 객체 복사
user2 = { id: 1, name: 'hong', age: 29 };

spread_x = { ...user2 };  // spread 연산자: 객체 복사
console.log(spread_x);    // { id: 1, name: 'hong', age: 29 }

user2.age = 30;           // user 객체의 age 프로퍼티 값 변경
console.log(user2);       // { id: 1, name: 'hong', age: 30 }
```

### Arguments Destructuring

```jsx
function print({ id, name }) {
  // 객체를 인수로 받음
  console.log(`${id}: ${name}`);
}
print(user2); // 1: hong

// ex2
function fn({ a, b }) {
  console.log(a, b);
}
fn({ a: 1, b: 2 }); // 1 2
```

### Class Destructuring

```jsx
class A{
    constructor(x, y){
        this.a = x;
        this.b = y;
    }
}

const x = new A(1, 2);  // {a: 1, b: 2}로 객체화 됨
const {a, b} = x;
```

### Array to Object Destructuring

```jsx
// {}앞에 const가 있으면 destructuring - 변수명 정함
// const가 없으면 block - key, value
const {id: idd, name: nm} = u;  // 변수명을 idd, nm으로 할게: idd = 1, nm = 'hong'
```
