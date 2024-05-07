# 컴퓨터 구조

Tags: VM, compile, linking, os, 컴파일러, 컴퓨터 아키텍처
Start Date: 2024년 5월 2일

내부를 이해해야함, 어떻게 돌아가느냐

리액트를 어떻게 만들 것이냐

자체 프레임워크: 페이스북, 네이버, 카카오 등 → 프레임워크를 개발하는 개발자: 상위 1% → 엔진에 대한 지식이 뛰어남

protocol: 컴퓨터 규약

## 컴퓨터 구조

![ca_1](ca/1.png)

- 프로세스/앱이 어떻게 구동되는지
- 마더보드 안에 CPU, Memory(주기억 장치)+캐시메모리, 보조기억장치(SSD), VGA(비디오카드)+GPU, LAN(여러개, 무선, 유선), LT(키보드, 마우스 등)
    - cpu-memory, cpu-ssd 사이에 Bus(넙적한 선) 있음 : 데이터, 명령어 이동함
    - os: 64bit, 32bit → bus 갯수
    - cpu, memory, bus, lan → 컴퓨터 속도를 좌우함
    - gpu: 짐을 싣는다 ex) 1톤 트럭, 10톤 트럭 속도는 1톤이 빠름 → 10톤 트럭: 병목현상 생김
    - 8bit = 1byte
    - cell 하나에 2^5(32bit)/ 2^6(64bit)
    - 색표현: 255*255*255+opacity(투명도) → 이 연산이 복잡하네? → GPU 쓰자(비디오카드)
    - 인공지능에서 gpu를 많이 사용하는 이유: 컴퓨터 언어인 2진수를 계산하는게 빠름, 학습시키는 모든 데이터가 2진수임, 비슷한 2진수 배열을 학습한다고 보면 됨, 단어도 2진수로 바꿔서 계산(A = 65)
    - IDC(데이터센터)는 cpu 열 관리가 중요해서 추운 해저에 많이 심음

### 요즘 쓰는 cpu

- cpu 안에 캐시 메모리가 들어가 있음
- 양자 컴퓨터: 데이터가 2진수가 아니어서 훨씬 빠름, 복호화도 빠름, 소수를 사용
- 생각만 하는 느낌 ex. 물마시러 가야지
- 실행은 메모리가 함

### 레지스터(8개)

- cpu가 쓰는 메모리
- 캐시, 주기억 보다 빠름
- 용도가 다른 8개 레지스터들에 잠깐 잠깐 담아둔다. (ex. 앞접시)
- program counter: 명령어가 실행될때 다음에 해야하는 행동을 알려줌
- address register
- flag register: 0/1 표현

### 캐시메모리

- 자주 쓰니까 옆에 두고 쓴다
- 이제는 캐시메모리를 자주쓰다보니 cpu에 점점 가까워 져서 L1, L2, L3 다 cpu에 들어가 있음
- ex. 텀블러에 물 담아놓고 옆에 두는거처럼
- 버스 안타고 바로 갈 수 있는 자주쓰는 메모리들
- 사이즈: L1 < L2 < L3 → L1을 가장 자주 사용

### 보조기억장치(disk, ssd)

- 주기억에 올라가는 것 bus 타고 감: cpu가 함
- 속도는 주기억장치보다 느림
- ex. 자주 안보는 영화

### 주기억장치(memory)

- 컴퓨터 끌 때까지(프로세스 종료) 영구적인 데이터
- 구역(context)가 나눠져있음
- 코드를 실행하면 cpu가 각 구역별 내용을 주기억장치로 올려줌
- kernel
- code: 내가 작성하는 코드(함수, 명령어), 한번이라도 컴파일 된거(목적코드라도)
- data: 변하지 않는 데이터(const)
- stack: 목차 부분, 사이즈가 정해진 데이터(data type 있음: int, double)
- heap: 데이터 부분, 사이즈가 정해지지 않은 데이터(String)

```c
int i = 10;
// int: 데이터타입, 크기: 4B(2^32 = 42억정도)
// i: 식별자, 변수, int i는 43억을 못가짐
// 10: 값(literal)

double d = 100;
// double: 크기: 8B(2^64)
// i, d 메모리 나눠 가짐
// i, d 각각 메모리 주소를 가짐 -> &i = 100, &d = 104 (목차 개념)
// 메모리공간, 주소공간 생김
// i, d 는 stack에 있음 -> 4B, 8b로 길이 정해짐

String s = 'ABC';
// 2^8 = 256
// A = 65, B = 66, C = 67 -> 각각 1B 안에 표현 가능 -> 총 3B 필요
// &A = 100, &B = 101, &C = 102 하고 끝이 아니라 null 까지 읽음
// s는 heap에 있음 -> 언제든 길이가 변할 수 있어서 크기가 정해지지 X
// ex. i = 100으로 바껴도 크기가 변하진 않음/ s = 'ABSER"로 바뀌면 5B 필요

const c = 100;
// const: 고정값, 상수 -> data 영역에 있음

function add(a, b){
	return a+b;
}
// 함수/명령어들도 메모리에 있어야 실행하니까 -> code 영역
```

### ALU (Arithmetic and Logical Unit)

- 연산을 하는 장치
- 컴퓨터는 기본적으로 더하기 밖에 못함 (ex. 6/2 = 6+(-3)+(-3))
- 곱하기, 나누기는 논리 연산을 함 (OR, AND, XOR, NOR, NAND 등)

### Accumulator(누산기)

- 이전 값을 잠깐 담아두는 곳
- 기술 발전 → 2~4개 까지 둠 (2주소 방식, 4주소 방식)
- [명령어 구조](https://www.notion.so/9cd673efc375473488e3fbb4b8c9c2bd?pvs=21)를 보자

```c
a = 1+2; // a=3을 누산기에 잠깐 담아둠
b = a+3; // 메모리까지 가지 않고 누산기에 있는 a를 가져옴
// 누산기 -> 실행이 빨라짐
```

### CU (control unit)

- bus를 통해 코드를 찾아옴
    1. address bus: 주소 필요
    2. data bus: 명령어 필요
    3. control bus: 명령이 어떤 동작을 해? 코드 줘
    4. 메모리에서 코드 찾음
    5. control bus: 나 돌아가
    6. data bus: add 코드 들어있어
    7. address bus: add 함수 주소 있어
- CU는 program counter만 보고 움직임: program counter가 명령어 실행 순서를 알고 있음  ex. 1-1(a=1) → 1-2(a한테 2줌) → 1-3(b에 담음)
- 사람이 쓴 코드를 컴파일 하면 → 컴퓨터가 이해하는 코드: 명령어 구조

### MMU (Memory Management Unit)

- 논리주소 ↔ 물리주소
- **hello.c, hello.java**: source code 
→ `컴파일`
**hello.o, hello.class**: 기계어(2진수), object code(목적코드) 
→ `linking` or `VM` or `Runtime`
**hello.exe**: **ML (OS, cpu):** 기계어
- linking
    - 링킹할 때 메모리 주소를 만듦
    
    ```c
    int a, b, c, d;  // 4B 4개 -> 16B 필요
    // 컴파일할때 물리적이 위치가 정해짐
    // 기준 주소는 os가 줌
    // a: &0, b: &4, c:&8, d:&12 -> 논리주소
    // a: &100, b: &104, c:&108, d:&112 -> 물리주소(실제 존재)
    // program register는 0번지로 알고 있지만 MMU가 실제주소인 100번지를 알고 있음
    ```
    

### Hit Ratio

- hit / (hit + miss)
- 눈감고 집었는데 내가 딱 원하는 걸 잡은 경우
- miss 보다 hit 할 확률이 높으면 효율 올라감
- hit ratio 증가 → 연산 속도 증가 → 메모리 가는 경우 줄어듬
- hit ratio 고려해서 코드를 짜야 좋은 코드
- 컴파일을 잘해주면(지금 쓰는 4세대 프로그래밍 언어) 코드를 대충짜도 잘 해주지

## 명령어 구조

![ca_2](ca/2.png)

- 명령어는 기계어마다 다름 → CPU 만든 사람 마음 (intel, mac 등)
- 명령어로만 하는 코딩 = 1세대 언어: 어셈블리어
    - 2세대 언어: c
    - 3.5세대 : python, js
- Operation Code(4bit)
- Operande(6*2 bit)
- 

```c
//operation code  operand
add 1 2
mv a     // 누산기에 a = 3 담음
add a 2
mv b     // 누산기에 b = 5
```

- cpu 종류: CISC, RISC
    - CISC
    - RISC: 프로그램 짜기 힘듬, 속도 빠름

## OS

HW를 operating system

os를 구동하기 위해 programing code(source code)가 필요

cpu가 이해할 수 있는 코드를 작성해야함

반도체 판 안에 다 os가 있음  ex. imbeded linux, android, chronium(키오스크) 등

### CMOS = ROM BIOS

- CMOS → booting → window/mac
- CMOS(ROM): 마더보드에 있는 메모리
    - 몇기가, 코드 이것저것 담겨있음
    - read only memory
    - 비휘발성, 읽기만 가능
- RAM
    - random acess memory
    - 휘발성
- Kernel
    - 펌웨어에 가까움 hw에 가까운 프로그램
    - hw ↔ sw
    - cmos에 있는 데이터를 memory(RAM)으로 가져옴
    - 응용sw, 시스템 sw, 펌웨어, hw 를 다 control 함
    - 미러링: c드라이브, d드라이브가 있을 때 c를 그대로 d로 복제 → c 메모리가 날라갈 경우를 대비
    - 리눅스에는 FAT과 NFS가 있고 요즘은 다 NFS 씀
- 가상 메모리
    - 스와핑: 실제 메모리와 가상메모리 왔다갔다 하는 것
    많아지면 느려짐(병목현상), 보조메모리가 주메모리보다 느려서 → ZRAM(메모리 압축해서 용량 감소)
    - fragmentation(단편화): 데이터를 담을 때 빈공간이 생기는 경우, 낭비되는 공간들
    - paging: 8/16kB 사이즈로 공간 나눔 → 빈공간은 생기지만 찾기는 빠름 (ex. 서랍정리)
    - garbage collector:
- DRAM: 메모리 주소 변형이 일어남, 느림, RAM에 붙어있음
- SRAM: 빠름, 비쌈, Cache에 붙어있음

### 컴파일러

| VM
(JVM = java VM) | Virtual Machine/ Engine
Runtime/ Environment | runtime: object code를 기계어로 바꾸는 과정
*engine, runtime 차이 중요 |
| --- | --- | --- |
| OS |  |  |
| HW |  |  |
- complier 언어: 한번에 전체 통역
- interpreter 언어: 실시간 한줄씩 통역, 상황에 따라 바뀔 수 있어서 유연성이 있음, AI 언어처럼
- 두 언어 모두 compile을 하지만 컴파일 하는 방식이 다름
- 대부분의 언어가 interpreter 언어로 바뀌고 있음 → 그래서 JVM라는 언어를 바꿔주는 VM이 필요함
    - java는 반반 가지고 있음

- 절차형, 함수형 모두 객체지향 언어임, class를 함수로 가는 바로가기 링크라고 생각함면 됨
    - 함수 → 함수 vs 생성자 함수: new f(); `new`를 붙여주면 생성자 함수
- 절차형 언어: 대부분 compiler 언어, 명령형, 각본대로 진행, class/interface로 객체지향을 표현 (c는 class가 없기 때문에 객체지향이 아님)
    
    ```c
    for(i=1, 100, i++)
    	if (i%2 == 1)
    		더해
    		
    1. 물 끓여
    2. 스프 넣어
    3. 면 넣어
    4. 계란 넣어
    ```
    
- 함수형 언어: 대부분 interpreter 언어, 선언형, 만들기 어럽지만 수정이 용이함, 함수로 객체지향을 표현
    
    ```c
    loop(i=1, 100, i++)
    	더해 if (i%2 == 1) // 더해 뒤를 상황에 따라 바꿀 수 있음
    	
    넣어 (스프)
    넣어 (면) -> 넣어 (반 쪼개 면) -> 반 쪼개라는 함수도 만들 수 있음
    => x = g(넣어, 반, 넣어)
    => x(스프, 면, 계란)
    ```
    
- 영한 변역이 좋아진 이유: 한국어 ↔ 일어/중국어 ↔ 영어 **←** **VM도 이런 역할**
- c, linux 등은 어디서 컴파일 했느냐에 따라 실행가능한 곳이 한정되는데 java는 JVM을 사용함으로써 제한이 없어짐
- 우리가 사용하는 브라우저들에는 모두 Html/CSS/JS Runtime이 들어있음

```jsx
a.js

i = 1 //0번지

a.obj
메모리 주소 정렬
// i는 그대로 0번지
// 옮겨진 주소들을 다 전달받음
i, j // stack에 있음

$> node a.js  // 명령어 실행
// node = Engine + Runtime
// a.js: 잠수함
// engine: 잠수함 부품; console 등
// runtime: 바다, 조립해서 바로 실행
// node, dno
```

```jsx
b.js

j = 2  //0번지

b.obj
// 0번지가 겹치니까
// j -> 10번지로 이동시킴
// console -> 100번지
```

- 객체지향 vs 절차지향
    
    ```jsx
    //객체지향 - python
    Dog() = 개를 구성하는 명령들
    Bbobbi = new Dog(); // 선언형
    
    //절차형 - c, java
    다리 4개
    멍멍
    .
    .
    .
    
    // 위에 코드는 속도가 같음 -> 함수 선언하면 절차형이 함수안에 들어있으니까
    // 실행(start)은 함수형이 빠름 - 메모리를 안잡고 엔진에 대한 것만 잡으면 되니까
    // 실행은 절차형이 느림 - 메모리를 한번에 다 잡아야하니까
    ```
    
- interface
    
    ```jsx
    x = new 붕어빵();  // new: 생성자 함수
    y = new 붕어빵();
    // x, y: 먹을 수 있는 거
    // 붕어빵(): x, y를 만들 수 있는 틀
    ```
    

### Process vs Thread

- node 가 실행되면 process(cpu가 수행하는 연산) 형태로 있음
clock이 구동되면 마더보드의 timer가 작동됨
- **process**: cpu가 일하고 있는 것, cpu 갯수만큼 만들 수 있음
- **thread**: 해야하는 일을 나눠서 여러개를 하는 것
- 프로세스 방식이 더 빠르나 프로세스 개수를 늘리면 cpu가 여러개 필요함
- 프로세스가 하나밖에 없으면 한개 일을 할때 반대쪽은 무조건 쉬어야함
- shared memory를 둬서 thread를 처리

### IPC (Inter-Process/Thread Communication)

프로세스들 사이에 서로 데이터를 주고받는 행위 또는 그에 대한 방법이나 경로

### PCB(Process Control Block)

- 프로세스 정보를 저장
- **Context Switching** : 홍길동 ↔ 김길동, 캐시메모리에 저장