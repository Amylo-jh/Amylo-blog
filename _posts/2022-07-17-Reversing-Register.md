---
title : "리버싱 - 01. 레지스터 종류"
categories :
 - CTF
tags :
 - CTF
 - Reversing
 - Register
toc : false
toc_label : "목차"
toc_sticky : false
last_modified_at : 2022-07-17T20:03
---

# 리버싱

CTF를 해 보려고 하는데 프로그램을 분석할 때 사용되는 레지스터들의 이름, 역할이 헷갈려서 정리해 보았다.

## 01. 레지스터 종류 (x86 아키텍처)

- 범용 레지스터
    - 범용 레지스터는 역할에 엄격히 규정되어 사용하지 않아도 된다. 대부분의 경우 이렇게 사용하고 있을 뿐 다른 용도로 사용하여도 문제가 생기지 않는다.

| 레지스터 이름 | 원문 | 한글이름 | 역할 |
| --- | --- | --- | --- |
| EAX | Extended Accumulator Register | 산술 레지스터 | 연산 결과를 저장한다. |
| ECX | Extended Counter Register | 카운터 레지스터 | 루프 숫자 등의 카운트를 저장한다. |
| EDX | Extended Data Register | 데이터 레지스터 | 연산에 사용하는 데이터를 저장한다. |
| EBX | Extended Base address Register | 베이스 레지스터 | 주소의 기본 값을 저장한다. |
| ESI | Extended Source Index | 출발지 인덱스 레지스터 | 일부 데이터 전송 명령에서 데이터 출발지를 저장한다. |
| EDI | Extended Destination Index | 목적지 인덱스 레지스터 | 일부 데이터 전송 명령에서 데이터 목적지를 저장한다. |

- 특수 레지스터
    - 특수 레지스터는 각각 전용의 용도가 있다.
    프로그램을 실행하는 데 매우 중요한 역할을 한다.

| 레지스터 이름 | 원문 | 한글이름 | 역할 |
| --- | --- | --- | --- |
| EBP | Extended Base Pointer | 베이스 포인터 레지스터 | 현재 스택 프레임의 기준 주소를 저장한다. |
| ESP | Extended Stack Pointer | 스택 포인터 레지스터 | 현재 스택의 최상단 주소를 저장한다. |
| EIP | Extended Instruction Pointer | 명령 포인터 레지스터 | 다음에 실행할 어셈블리 명령 주소를 저장한다. |

- 플래그 레지스터
    - 플래그 레지스터는 주로 이전 명령을 통해 발생한 결과의 상태나 프로세서의 상태를 저장한다.
    - 이 레지스터에 저장된 값에 따라 명령이 변하거나, 조건문 같은 실행 순서 분기가 변하기도 한다.
    - 32비트에서의 EFLAGS는 17개의 플래그가 저장된다.

| 레지스터 이름 | 플래그 이름 | 원문 | 한글이름 | 역할 |
| --- | --- | --- | --- | --- |
| EFLAGS | CF | Carry Flag | 캐리 플래그 | 연산 명령으로 Carry(자리 올림) 또는 Borrow(부포 비트 앞 비트에 변경이 생김)가 발생할 때 설정(1) 된다. |
|  | ZF | Zero Flag | 제로 플래그 | 연산 결과가 0이 되는 경우 설정(1) 된다. 아닐 떄는 해제(0) 된다. |
|  | SF | Sign Flag | 부호 플래그 | 연산 결과가 음수가 될 때 설정(1) 된다. 아닐 때는 해제(0) 된다. |
|  | DF | Direction Flag | 방향 플래그 | 문자열 처리를 할 때 연속되는 문자열 처리 방향에 따라 설정(1) 된다. |
|  | OF | Overflow Flag | 오버플로우 플래그 | 부호 있는 연산 결과가 레지스터에 저장 가능한 용량보다 큰 경우에 설정(1) 된다. |
| 세그먼트 레지스터 | CS | Code Segment Register | 코드 세그먼트 레지스터 | 코드 세그먼트 주소를 저장한다. |
|  | DS | Data Segment Register | 데이터 세그먼트 레지스터 | 데이터 세그먼트 주소를 저장한다. |
|  | SS | Stack Segment Register | 스택 세그먼트 레지스터 | 스택 세그먼트 주소를 저장한다. |
|  | ES | Extra Segment Register | 보조 세그먼트 레지스터 | 보조 세그먼트 주소를 저장한다. |
|  | FS | F Segment Register | F 세그먼트 레지스터 | F 세그먼트 (2번째 추가 세그먼트) 주소를 저장한다. |
|  | GS | G Segment Register | G 세그먼트 레지스터 | G 세그먼트 (3번째 추가 세그먼트) 주소를 저장한다. |

모든 레지스터의 길이는 32비트이다.

EAX, EBX, ECX, EDX 레지스터의 하위 16비트는 각각 AX, BX, CX, DX라고 부른다.

그 안에서도 상위 8비트는 AH, BH, CH, DH, 하위 8비트는 AL, BL, CL, DL이라고 한다.

ESI, EDI 레지스터의 하위 16비트는 각각 SI, DI 레지스터라고 한다.

x64-86 아키텍쳐 (64비트 시스템)의 레지스터는 이것을 확장하여 이름이 각각 RAX, RCX, RDS, RBX, RSI, RDI, RBP, RSP, RIP 가 되었다. 하위 32비트는 x86에서 부르던 이름 그대로 사용한다. 마찬가지로 하위 32비트의 하위 16비트, 하위 8비트는 x86 아키텍처의 구조와 동일하고, 이름도 동일하게 사용한다.

자료 출처 : "CTF 정보보안 콘테스트 챌린지북" 에서 발췌

---
<mark style='background-color: #dcffe4'>
<b>글에 오류가 있거나 궁금한 것, 의견 등이 있으면 자유롭게 댓글로 달아주시면 감사하겠습니다!</b>
</mark>