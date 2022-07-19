---
layout: single
title: "[블록체인 이론] 블록체인과 블록체인 프로그래밍"
categories: Blockchain
tags: [Blockchain]

toc: true
toc_sticky: true

date: 2022-07-19

permalink: /blockchain/basic/
---

## <span style="font-size:1.125em;">🚀 블록체인이란?</span>

<span style="font-size:15px;">
데이터 분산 저장 기술의 일종으로 블록 단위의 데이터를 체인 형태로 연결하여 저장하는데 이때 저장된 데이터를 모든 사용자에게 분산하여 저장한다.
이러한 분산저장 특성 때문에 분산원장기술(분산장부기술, Distributed Leder Technology, DLT)이라 한다. <br>
블록체인 프로그래밍은 기존 시스템을 전부 대체한다기 보다 확인과 검증을 하는 신뢰 중개 코드를 바탕으로 기존 시스템을 개선하는 것이다.
</span>

<span style="font-size:15px;">
트랜잭션의 집합이 블록을 만들고 블록의 집합이 블록체인을 만든다. <br>
과정은 아래와 같음 <br>
(흘러가보자)
</span>

## <span style="font-size:1.125em;">🚀 노드</span>

<span style="font-size:15px;">
노드란 탈중앙화 시스템의 참여자를 위한 블록체인 소프트웨어와 그것이 설치된 장비, 하드웨어를 집합적으로 부르는 말이다.
쉽게 말하자면 블록체인 네트워크 참여 구성원을 말한다.
</span>
 1
- <span style="font-size:15px;">노드를 펼쳐보면 5계층이 있음 (아래서 위로)</span>
  - <span style="font-size:15px;">
    탈중앙화 애플리케이션 (웹 또는 엔터프라이즈 애플리케이션)
    </span>
   - <span style="font-size:15px;">
    가상머신 샌드박스상의 애플리케이션 로직 (애플리케이션 로직을 실행하는 환경을 구성)
    </span>
   - <span style="font-size:15px;">
    블록체인 프로토콜 (트랜잭션과 블록 합의 같은 블록체인의 기능을 구현)
    </span>
   - <span style="font-size:15px;">
    네트워크 오퍼레이팅 시스템
    </span>
   - <span style="font-size:15px;">
    컴퓨터 시스템 하드웨어
    </span>


- <span style="font-size:15px;">
  4번째 층은 애플리케이션을 호스팅하고 있고 이 층에서 데이터 엑세스 컨트롤, 확인, 검증, 저장을 위한 함수 코딩과 같은 문제들을 해결합니다.
  </span>
- <span style="font-size:15px;">
  5번째 층은 HTML, JS, 관련된 프레임워크와 같은 웹 프로그래밍이 이루어지고 이 요소들은 Dapp과 UI 레이어를 구성합니다.
  </span>

<span style="font-size:15px;">
블록체인 어플리케이션은 참여자들을 노드 네트워크를 통해 연결하고 네트워크는 트랜잭션, 블록을 브로드캐스팅 할 수 있다.
</span>

#### <span style="font-size:1em;">🔥 풀 노드, 라이트 노드</span>

- <span style="font-size:15px;">
  풀 노드(full node)는 블록체인의 제네시스 블록부터 현재까지의 모든 내역을 저장하는 노드이다. PC에 모든 내역이 저장되어 있기 때문에 스스로 거래 검증이 가능하다는 것이 가장 큰 특징이며, 모든 내용을 저장하기 위해 소요되는 시간이 길고 큰 용량이 필요하다는 단점이 있다.
  </span>
- <span style="font-size:15px;">
  라이트 노드(light node)는 풀 노드의 단점을 해결하기 위해 나온것이다. 라이트 노드는 블록체인에 참여하여 거래를 생성하는 노드로, 풀 노드에 거래 데이터를 요청하여 개별 거래를 검증하는 기능을 수행한다. 풀 노드와 다르게 자료의 일부분만을 다운받는 대신, 거래를 위해 내용 검증이 필요할때마다 풀 노드에 자료를 요청해야만 한다.
  </span>

### <span style="font-size:1.125em;">🚀 트랜잭션</span>

<span style="font-size:15px;">
트랜잭션이란 어떤 오퍼레이션을 실행할지, 그 오퍼레이션을 실행하기 위한 데이터 파라미터 그리고 메시지 송신인, 수신인, 트랜잭션 수수료, 저장할 때의 타임스태프 등을 포함하는 P2P 메시지로서 블록체인에 기록된다.
</span>

- <span style="font-size:15px;">트랜잭션이 만들어지는 과정</span>
  - <span style="font-size:15px;">
    위 노드 4층을 참고해보면 애플리케이션이 스마트 컨트랙트 함수를 호출하거나 실행하면 블록체인에 기록할 트랜잭션을 생성한다. 만약 확인과 검증에 위배하면 이 함수 호출은 취소되고 성공하면 블록체인 네트워크를 통해 이 트랜잭션을 브로드캐스팅하고 변조 불가능한 분산 장부에 기록한다.
    </span> 

<span style="font-size:15px;">
다시 블록체인 과정이 만들어지는 과정으로 되돌아가보면
</span>

<span style="font-size:15px;">1. </span>
<span style="font-size:15px;">
검증 단계를 거친 수집한 네트워크의 트랜잭션들은 풀로 모이고 노드는 블록을 만들기 위해 풀에서 수수료가 비싼 트랜잭션부터 선택해 블록을 만든다.
</span>

<span style="font-size:15px;">2. </span>
<span style="font-size:15px;">
참여 노드들은 합의 알고리즘을 사용해 체인에 추가 될 하나의 블록에 대해 합의를 한다.
</span>

<span style="font-size:15px;">3. </span>
<span style="font-size:15px;">
해시를 새롭게 추가될 블록에 더해 체인 링크를 만든다.
</span>

<span style="font-size:15px;">
위 과정으로 첫 번째 제네시스 블록이 생성되고 블록체인에 참여하는 모든 노드는 제네시스 노드로부터 시작해 모두 동일한 복사본을 가진다.
이렇게 블록체인은 분산된 변조 불가 장부라는 특징이 있다.
</span>

### <span style="font-size:1.125em;">🚀 블록</span>

<span style="font-size:15px;">
블록은 블록 헤더와 트랜잭션 머클 트리로 구성된 데이터 구조다. 
</span>

#### <span style="font-size:1em;">🔥 구조</span>

- <span style="font-size:15px;">블록 구조 (필드/필드 크기(바이트))</span>
  - <span style="font-size:15px;">
    block size 4 : 다음 필드부터 블록 끝까지의 데이터 크기
    </span> 
  - <span style="font-size:15px;">
    block header 80 : 블록 헤더 정보
    </span> 
  - <span style="font-size:15px;">
    transaction counter 1~9 : 블록에 포함된 거래의 수
    </span> 
  - <span style="font-size:15px;">
    transactions 가변적 : 거래의 목록
    </span> 

- <span style="font-size:15px;">블록 헤더 구조</span>
  - <span style="font-size:15px;">
    version 4 : 소프트웨어 또는 프로토콜 버전 정보
    </span> 
  - <span style="font-size:15px;">
    previous block hash 32 : 부모 블록의 해시값
    </span> 
  - <span style="font-size:15px;">
    merkle root 32 : 머클 트리 루트의 해시값
    </span> 
  - <span style="font-size:15px;">
    timestamp 4 : 블록 생성 시간
    </span> 
  - <span style="font-size:15px;">
    difficulty target 4 : 블록 생성 시 pow 난이도
    </span> 
  - <span style="font-size:15px;">
    nonce 4 : PoW에서 사용하는 카운터
    </span> 

#### <span style="font-size:1em;">🔥 블록 높이</span>

<span style="font-size:15px;">
블록 높이는 블록이 얼마나 쌓여 있는가, 데이터의 양을 의미합니다. 블록의 높이가 0이면 제네시스 블록이라 말한다.
</span>

#### <span style="font-size:1em;">🔥 이더리움 블록</span>

<span style="font-size:15px;">
이더리움의 블록 크기는 제한이 없기 떄문에 블록의 크기가 엄청나게 커지는 것을 막기 위해 계정 정보와 이더 잔액은 상태 트리에 저장한다. 상태 트리는 블록 외부에 보관되며 블록에는 상태 트리의 루트 노드 값(블록에 저장한 거래를 기반으로 생성)을 저장한다.<br>
상태 트리의 루트 노드값은 각 계정의 상태를 상태 트리의 노드에 넣고 노드의 계정 주소(키)들로 해시값을 계산하여 구한다. 상태가 바뀌면 루트 노드 값이 변하므로 데이터 조작을 바로 확인 할 수 있다. 이더리움은 머클 패트리샤 트리(patricia tree)를 사용한다. 
</span>

### <span style="font-size:1.125em;">🚀 해시, 합의 알고리즘</span>
<a href="https://www.banksalad.com/contents/%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85%ED%95%98%EB%8A%94-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8%EC%9D%98-%EC%9B%90%EB%A6%AC-%EC%B1%84%EA%B5%B4-%ED%95%B4%EC%8B%9C-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%9E%91%EC%97%85%EC%A6%9D%EB%AA%85-qvCud" target="_blank">해시와 작업증명 👈 <span style="font-size:18px;">PoW</span></a>

- <span style="font-size:15px;">
  지분 증명(Proof of Stake, PoS)
  </span>
  - <span style="font-size:15px;">
    지분 증명은 코인을 스테이킹한 풀 노드가 체인에 추가해야 할 블록을 결정한다. 네트워크에 지분이 있는 노드가 악의적인 공격을 하지 않을 거라는 아이디어다.<br>
    스테이킹을 많이 한 노드가 독점하지 못하도록 라운드 로빈이라는 방식을 사용한다. 블록 생성마다 주어지는 블록 보상 이외에 트랜잭션의 수수료를 모아 벨리데이터에게 지급함으로써 경제적 동기를 충족시켜준다.
    </span>

- <span style="font-size:15px;">
  프랙티컬 비잔틴 장애 허용(PBFT, Practical Byzantine Fault Tolerance)
  </span>
  - <span style="font-size:15px;">
    노드들은 리더 노드를 선정하고 리더 노드는 다른 노드들에게 메시지(블록검증)을 전송한다. 노드들은 서로 메시지를 교환하며 PoW나 PoS와 달리 다수결로 의사결정 한 후 블록을 생성하기 때문에 랜덤한 독립적인 오류나 악한 노드가 있더라도 합의를 이룰 수 있다.
    </span>

<span style="font-size:15px;">
합의는 블록체인 프로토콜에서 핵심적 요소다.  
</span>