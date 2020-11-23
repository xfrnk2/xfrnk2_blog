---
title: "LR, SLR, CLR 구문분석"
date: 2020-11-23T22:39:02+09:00
draft: false

categories: ['compiler']
tags: ['compiler']
author: "xfrnk2"
---
## **공부 목표**  
> 1. LR 구문분석 방법의 특성을 이해할 수 있는지?  
> 2. Closure, Goto 함수를 구할 수 있는지?
> 3. Canonical collection을 이해하고 Goto 그래프를 구성할 수 있는지?
> 4. 파싱표를 구성하는 방법을 이해할 수 있는지? 
> 5. 파싱표를 보고 구문분석을 할 수 있는지?  
> 6. SLR 파싱표에서 충돌 문제를 이해할 수 있는지?
> 7. LR(1) 항목에서 lookahead 정보를 계산할 수 있는지?
> 8. lookahead 정보를 이용한 closure와 canonical collection을 이해 할 수 있는지?
---
  
## **용어 설명**
+ **LR** : Left to right scanning and Right parse
+ **LR(k) 문법** : 모든 엔트리(entry)에 대해, 유일하게 정의되는 파싱표를 만들 수 있는 문법. 여기서 k를 lookahead의 길이라고 하며, 이는 handle을 결정하는 데 k개의 입력기호에 이르기까지 조사하는 것
+ **LR(0) 항목** : 생성규칙의 오른쪽에 점기호(dot symbol)를 가진 생성규칙
+ **LR(1) 항목** : LR(0)항목에서 lookahead 정보를 첨가한 것. LR(1) 항목은 [A → α․β, a]형태를 가지며, 여기서 A → αβ ∈ P이고 a ∈ VT ∪ {$} 이다. A → α․β를 core라 부르며 LR(0)항목과 같은 의미
+ **lookahead** : LR(1) 항목 [A → α․β, a]에서 a를 항목의 lookahead라 부르며, reduce 항목 일 때 그 기호에 대해서 reduce행동을 하는 것을 뜻함
+ **증가문법** : G=(VN,VT,P,S)에서 G에 첨가된 문법. 즉, 시작기호 {S'→S} 를 하나 더 추가한 문법
+ **Closure** : 마크기호가 [A→ α․Bβ]와 같이 논터미널인 경우에 이 논터미널을 생성규칙의 왼쪽으로 갖는 LR(0) 항목을 구하는 것을 closure라고 하며, 이 closure는 반복적으로 계속 적용
+ **GOTO 함수** : GOTO(I, X) ＝ closure({[A → αX․β]| [A → α․Xβ∈I})즉, 현재 마킹한 상태에서 구문분석을 하기 위해, 마크기호를 하나씩 읽는 것을 의미
+ **Canonical collection** : LR(0) 항목 집합의 canonical collection은 C0로 표기하며, C0 ＝ {closure ([S'→ ․ S]) ∪ {GOTO (I, X)| I ∈C0, X∈V} 임. 즉, closure ([S'→ ․ S])를 구한 후 반복해서 GOTO (I, X)를 적용한 것들의 집합
+ **SLR 구문분석** : Simple LR 구문분석은 LR 구문분석 방법 중 가장 간단하게 구현될 수 있는 방법. SLR 파싱표를 만드는 방법은 LR(0) 항목의 집합과 FOLLOW를 이용
+ **CLR 구문분석** : Canonical LR 구문분석은 SLR 의 충돌문제를 해결한 구문분석 방법. lookahead를 이용하는 방법인데, lookahead를 구해야 하므로 과정이 복잡하고 시간이 오래 걸리며 파싱표가 커킴
---
  
## **내용 정리**
> 1.  순위 구문분석에서는 handle을 찾기 위해서 여러 가지 제약조건들을 주었으나, 이런 제약조건들 때문에 일반적인 프로그래밍 언어의 구문구조를 표현하기에는 적당하지 못한 단점이 있었다. LR(Left to right scanning and Right parse) 구문분석은 일반적인 프로그래밍 언어의 구문구조를 표현하고 구문분석을 하기 위해서 나온 방법이다. 
> 2.  LR 파싱표를 구성하는 방법은 3가지가 있다. 첫째, LR(0) 항목의 집합과FOLLOW로부터 파싱표를 만드는 Simple LR(SLR) 방법, 둘째, LR(1) 항목의집합으로부터 파싱표를 만드는 방법인 Canonical LR(CLR) 방법, 셋째, LR(0) 항목의 집합과 lookahead로부터 또는 LR(1) 항목의 집합으로부터 구성하는 방법인 LookAhead LR(LALR) 방법 등이다. 
> 3.  증가문법(augmented grammar)이란 G=(VN,VT,P,S)에서 G에 첨가된 문법으로 G'=(VN∪{S'},VT ,P∪{S' → S},S')를 뜻한다. 여기서 생성규칙 S' → S을 첨가하는 이유는 구문분석시 주어진 문장에 accept 되는 것을 S' → S로 reduce되는 경우로 취하기 위한 것이다.
> 4.  마크기호가 [] 와 같이 논터미널인 경우에 이 논터미널을 생성규칙의 왼쪽으로 갖는 LR(0) 항목을 구하는 것은 closure라고 한다. 이 closure는 반복적으로 계속 적용된다. 즉, 한 상태에서 valid한 모든 LR(0) 항목을 구하는것이다.
> 5. SLR 구문분석은 LR 구문분석 방법 중 가장 간단하게 구현될 수 있는 방법으로서, SLR의 파싱표를 만드는 방법은 LR(0) 항목의 집합과 FOLLOW를 이용한다. SLR 파싱표는 shift와 GOTO 행동은 LR(0) 항목의 canonicalcollection으로부터 구해지고, reduce는 생성규칙의 논터미널 기호의 FOLLOW를 보고 결정한다.
> 6. SLR 구문분석 Simple LR 구문분석은 LR 구문분석 방법 중 가장 간단하게 구현될 수 있는 방법이다. SLR 파싱표를 만드는 방법은 LR(0) 항목의 집합과 FOLLOW를 이용한다. 
> 7. 한 상태에서 어떤 논터미널 기호 다음에 나올 수 있는 터미널 기호들을 lookahead라 부르는데 이를 이용하려고 하는 것이 바로 CLR 방법이다.
> 8. LR(0) 항목에서 lookahead 정보를 첨가한 것이 LR(1) 항목이며 [A → α․β, a] 형태를 가지며 여기서 A → αβ ∈ P이고 a ∈ VT ∪ {$} 이다. 첫 번째 부분 A → α․β를 core라 부르며 LR(0)항목과 같은 의미이다. 두 번째 부분 a를 항목의 lookahead라 부르며, reduce 항목 일 때 그 기호에 대해서 reduce 행동을 하는 것을 뜻한다. 
> 9. CLR 파싱표를 구성하기 위해서는 LR(1) 항목 집합의 canonical
collection을 구해야 하는데, 이것 역시 closure 함수와 GOTO 함수가 필요하다. GOTO 함수는 LR(0) 항목 집합의 canonical collection을 구할 때와 같으며 closure 함수는 lookahead 정보 때문에 약간 수정해야 한다.
> 10. CLR방법은 한 상태에서 어떤 논터미널 기호 다음에 나올 수 있는 터미널 기호인 lookahead를 이용하는 방법인데, lookahead를 구해야 하므로 과정이 복잡하고 시간이 오래 걸리며 파싱표가 커지는 단점이 있다. 