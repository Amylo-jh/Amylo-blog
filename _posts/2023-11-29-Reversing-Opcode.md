---
title : "리버싱 - 02. x86-64 명령어 종류"
categories :
 - CTF
tags :
 - CTF
 - Reversing
 - Opcode
toc : false
toc_label : "목차"
toc_sticky : false
last_modified_at : 2023-11-29T00:00
---

CTF를 해 보려고 하는데 프로그램을 분석할 때 사용되는 어셈블리의 명령어 이름, 역할이 헷갈려서 정리해 보았다.

## 01. 명령어
- 인텔의 x64에는 매우 많은 종류의 명령어가 있다.
- 그 중 자주 쓰이는 21개의 명령어를 다룰 예정.

|명령 의미|명령 코드|
|---|---|
|데이터 이동 (Data Transfer)|mov, lea|
|산술 연산 (Arithmetic)|inc, dec, add, sub|
|논리 연산 (Logical)|and, or, xor, not|
|비교 (Comparison)|cmp, test|
|분기 (Branch)|jmp, je, jg|
|스택 (Stack)|push, pop|
|프로시저 (Procedure)|call, ret, leave|
|시스템 콜 (System Call)|syscall|

---
### 데이터 이동
어떤 값을 레지스터나 메모리에 옮기도록 지시한다.
- mov 명령어
	> 사용법 : `mov dst, src`<br>
	> 의미 : src에 들어있는 값을 dst에 대입 
	- 예시
		- `mov rdi, rsi` : rsi의 값을 rdi에 대입
		- `mov QWORD PTR[rdi], rsi` :  rsi의 값을 rdi가 가리키는 주소에 대입
		- `mov QWORD PTR[rdi+8*rcx], rsi` : rsi의 값을 di+8\*rcx가 가리키는 주소에 대입
- lea 명령어
	> 사용법 : lea dst, src<br>
	> 의미 : src의 유효 주소(Effective Address, EA)를 dst에 저장한다. 주소 위치를 저장하는 것에 유의.
	- 예시
		- `lea rsi [rbx+8*rcx]` : rbx+8\*rcx를 rsi에 대입

---
### 산술 연산
덧셈, 뺄셈, 곱셈, 나눗셈 연산을 지시한다.
`곱셈, 나눗셈은 여기에서 설명하지 않는다.`
- add 명령어
	> 사용법 : add dst, src<br>
	> 의미 : dst에 src 값을 더한다.
	- 예시
		- `add eax, 3` : eax += 3
		- `add ax, WORD PTR[rdi]` : ax += \*(WORD*)rdi : ax 레지스터에 rdi가 가리키는 곳의 word만큼의 값을 가져와서 더한다.
- sub 명령어
	> 사용법 : sub dst, src<br>
	> 의미 : st에서 src의 값을 뺀다.
	- 예시
		- `sub eax, 3` : eax -= 3
		- `sub ax, WORD PTR[rdi]` : ax -= \*(WORD*)rdi : ax 레지스터에 rdi가 가리키는 곳의 word만큼의 값을 가져와서 뺀다.
- inc 명령어
	> 사용법 : inc op<br>
	> 의미 : op의 값을 1 증가시킴
	- 예시
		- `inc eax` : eax += 1
- dec 명령어
	> 사용법 : dec op<br>
	> 의미 : op의 값을 1 감소시킴
	- 예시
		- `dec eax` : eax -= 1

---
### 논리 연산
and, or, xor, not 등의 비트 연산을 지시
이 연산은 비트 단위로 이루어진다.
- and 명령어
	> 사용법 : and dst, src<br>
	> 의미 : dst와 src의 비트가 모두 1이면 1, 아니면 0을 하여 dst에 값을 쓴다.
	- 예시
		- `eax = 0xffff0000`
		- `ebx = 0xcafebabe`
		- `add eax, ebx`
		- eax의 값은 `0xcafe0000`이 된다.
- or 명령어
	> 사용법 : or dst, src<br>
	> 의미 : dst와 src의 비트 중 하나라도 1이면 1, 아니면 0을 하여 dst에 값을 쓴다.
	- 예시
		- `eax = 0xffff0000`
		- `ebx = 0xcafebabe`
		- `or eax, ebx`
		- eax의 값은 `0xffffbabe`가 된다.
- xor 명령어
	> 사용법 : xor dst, src <br>
	> 의미 : dst와 src의 비트가 서로 다르면 1, 같으면 0 하여 dst에 값을 쓴다.
	- 예시
		- `eax = 0xffffffff`
		- `ebx = 0xcafebabe`
		- `xor eax, ebx`
		- eax의 값은 `0x35014541`이 된다.
		- a 값에 대해 b로 xor연산을 두 번 하면 원래 값으로 돌아가는 성질이 있다.
		- 이 특성을 이용하여 단순한 암호가 개발되기도 한다.
- not 명령어
	> 사용법 : not op<br>
	> 의미 : op의 비트 전부 반전
	- 예시
		- `eax = 0xffffffff`
		- `not eax`
		- eax의 값은 `0x00000000`이 된다.
  
---
### 비교
두 피연산자의 값을 비교하여 플래그를 설정한다.
- cmp 명령어
	> 사용법 : cmp op1, op2<br>
	> 의미 : op1와 op2를 비교
	- cmp는 두 피연산자를 빼서 대소를 비교한다.
	- 연산의 결과는 op1에 대입하지 않ㄴ는다.
	- 예를 들어 서로 같은 수를 빼면 결과가 0, ZF가 설정되므로, CPU는 이 플래그를 보고 값이 같았는지를 판단할 수 있다.
	- 예시
		- `mov rax, 0xA`
		- `mov rbx, 0xA`
		- `cmp rax, rbx`
		- 플래그 레지스터 중 ZF가 1이 된다.
- test 명령어
	> 사용법 : test op1, op2<br>
	> 의미 : op1과 op2를 비교
	- test는 두 피연산자에 AND 비트연산을 취한다.
	- 연산의 결과는 op1에 대입하지 않는다.
	- 예시
		- `xor rax, rax` (같은것끼리 xor하였으니 무조건 값은 0이 됨)
		- `test rax, rax` (0과 0을 and 연산, 결과로 0)
		- 플래그 레지스터 중 ZF가 1이 된다.

---
### 분기
분기문은 여기에서 다루는 것 외에도 굉장히 많은 명령어가 존재한다.
그러나 다루지 않는 것들은 이름을 통해 직관적으로 파악할 수 있기 때문에, 별도로 찾아보는 것을 권장
분기 명령어는 `rip`를 이동시켜 실행 흐름을 바꾼다.
	rip : Register Instruction Pointer, 명령 포인터 레지스터로, 다음에 실행할 어셈블리 명령 주소를 저장한다.
- jmp 명령어
	> 사용법 : jmp addr<br>
	> 의미 : addr로 rip를 이동시킨다.
	- 예시
		- `xor rax, rax`
		- `jmp 1` (1번째줄 코드로 점프)
- je 명령어
	> 사용법 : je addr<br>
	> 의미 : 직전에 비교한 두 피연산자가 같으면 점프 (Jump if Equal)
	- 예시
		- `mov rax, 0xcafebabe`
		- `mov rbx, 0xcafebabe`
		- `cmp rax, rbx` (둘이 같다. 그리 고 ZF에 0이 설정됨)
		- `je 1` (jump to line 1)
- jg 명령어
	> 사용법 : jg addr <br>
	> 의미 : 직전에 비교한 두 연산자 중 전자가 더 크면 점프 (Jump if Greater)
	 - 예시
		 -  `mov rax, 0x31337`
		 - `mov rbx, 0x13337`
		 - `cmp rax, rbx` (rax > rbx)
		 - `jg 1` (jump to line 1)

---
<mark style='background-color: #dcffe4'>
<b>글에 오류가 있거나 궁금한 것, 의견 등이 있으면 자유롭게 댓글로 달아주시면 감사하겠습니다!</b>
</mark>