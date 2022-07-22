---
layout: single
title: "[블록체인 이론] 이더리움 논스란?"
categories: Blockchain
tags: [Blockchain]

toc: true
toc_sticky: true

date: 2022-07-22

permalink: /blockchain/ethernonce/
---

## <span style="font-size:1.125em;">🚀 이더리움 논스</span>

### <span style="font-size:1em;">🔥 논스란? (트랜잭션 논스)</span>

- <span style="font-size:15px;">
논스는 계정의 상태 별로 정의가 다르다. 외부 소유 계정의 상태에서의 논스란, 해당 계정 주소가 전송한 거래/상호작용의 수이지만 계약 계정의 논스란, 해당 계정이 생성한 계약의 수이다.
</span>
- <span style="font-size:15px;">
계정에서 보내는 트랜잭션에 할당된 번호로, 이 트랜잭션이 트랜잭션을 생성하는 어카운트 내에서 몇 번째로 발생하는 트랜잭션 인지를 나타내는 16진수 값이다. (블록체인에 기록된 트랜잭션 개수)
</span>
- <span style="font-size:15px;">
이더리움 백서에는 각 트랜잭션이 오직 한번만 처리되게 하는 일종의 카운터로 설명된다.
</span>
- <span style="font-size:15px;">
nonce는 계정에서 보내는 트랜잭션에 할당 된 번호로
</span>
  - <span style="font-size:15px;">
    트랜잭션(거래)을 전송시 nonce는 1씩 증가한다.
    <span>
  - <span style="font-size:15px;">
    nonce는 계정에서 유일하며 동일한 nonce 가 존재 하지 않는다.
    <span>
- <span style="font-size:15px;">
  계정 생성시 nonce는 0 부터 시작하며
  </span>
  - <span style="font-size:15px;">
    전송한 트랜잭션1 : nonce = 1 
    </span>
  - <span style="font-size:15px;">
    전송한 트랜잭션2 : nonce = 2 
    </span>
  - <span style="font-size:15px;">
    전송한 트랜잭션3 : nonce = 3 
    </span>
  - <span style="font-size:15px;">
    ...
    </span>

### <span style="font-size:1em;">🔥 논스 규칙</span>

- <span style="font-size:15px;">
트랜잭션은 순서대로 이루어져야 한다.
</span>
  - <span style="font-size:15px;">
    현재 계정의 nonce가 1이라면, nonce가 0인 트랜잭션을 전송할 수 없음 (순서를 역행X)
    </span>
- <span style="font-size:15px;">
    순번을 건너 뛰지 않는다.
    </span>
  - <span style="font-size:15px;">
    nonce 는 순차적으로 증가하고 처리되기 때문에 nonce 가 3인 트랜잭션을 전송하려면, nonce의 값 0~2까지 전송한 내역이 있어야 한다.
    예를 들어 nonce 가 3 인 트랜잭션 전송 시, 현재 계정의 nonce가 1일 경우, 트랜잭션이 처리되지 않고 트랜잭션 풀 Queue 에 남아있게 된다. 그리고 nonce가 2인 트랜잭션을 전송하였을 때 2, 3 이 연달아 처리된다.
    </span>

### <span style="font-size:1em;">🔥 트랜잭션 풀 & Queue</span>

<span style="font-size:15px;">
트랜잭션이 생성되면 마이너를 포함한 노드들에게 브로드캐스팅 되고 마이너들이 트랜잭션을 받아서 블록에 기록한다. 마이너를 포함한 이더리움 네트워크의 노드들(이더리움 클라이언트)에는 아직 이더리움 블록에 기록되지 않은 트랜잭션들을 보관하는 대기열이 있는데 이것을 트랜잭션 풀(transaction pool; geth) 혹은 트랜잭션 큐(Transaction queue; parity)라고 부른다. 각 노드들은 여기에 트랜잭션을 보관하고 있다가 이더리움 블록에 트랜잭션이 기록되면 새로운 트랜잭션을 받을 수 있도록 대기열을 비우게 된다.
</span>

<img width="681" src="https://user-images.githubusercontent.com/108192885/177063641-a698ce82-3e85-41af-91dd-5e2fe8f2eca1.png">

<span style="font-size:15px;">
트랜잭션의 홍수가 일어날 때는 어떻게 될까?
</span>

<span style="font-size:15px;">
이더리움 네트워크는 이더리움 블록이 특정 시간 간격으로 생성되도록(15초) 난이도를 자동으로 조절하게 되어 있기 때문에 처리할 수 있는 트랜잭션의 수가 정해져 있다. 마이닝 풀에서 시간당 처리할 수 있는 트랜잭션의 수는 정해져 있는데 트랜잭션이 쏟아져 들어오면 대기열이 꽉 차버린다. 그러면 정해진 규칙(Eviction Rule)에 따라 이후에 도착하는 트랜잭션들은 버려진다. 
</span>

- <span style="font-size:15px;">
 Geth 1.6.2 버전 기준 GlobalSlots의 기본값은 4,096으로 설정되어 있어 대기열에 쌓여있는 트랜잭션 개수가 4,096개가 넘어가면 버려지게 된다. 
</span>

- <span style="font-size:15px;">
Geth만큼 널리 쓰이는 Parity 클라이언트에도 Tx-pool 크기를 설정할 수 있는 옵션인 --tx-queue-size가 있다. 버전 1.6.8 기준으로 기본값이 1,024개 밖에 안된다. Parity 클라이언트에는 대기열에 쌓여 있는 트랜잭션을 볼 수 있는 Dapp인 TX-Queue Viewer가 기본으로 탑재되어 있다.
</span>

### <span style="font-size:1em;">🔥 왜 nonce가 필요할까?</span>

<span style="font-size:15px;">
비트코인은 UTXO로 잔고를 관리하지만 이더리움은 계정에 직접 잔고를 두고 관리한다.
</span>

<span style="font-size:15px;">
비트코인 전송에 사용된 UTXO들은 트랜잭션이 처리되고 나면 UTXO set에서 제거되어 더 이상 사용할 수 없게 된다. 같은 트랜잭션을 복제해서 실행한다고 해도 입력이 참조한 UTXO가 더 이상 UTXO가 아니기 때문에 유효하지 않은 트랜잭션이 된다.
</span>

<span style="font-size:15px;">
이더리움의 경우 잔고는 잔고 값으로 관리된다. 트랜잭션이 실행되면 잔고 값이 바뀌는 것이다. 잔고 값만 바꿀 수 있다면 잔고를 조작하는 것이 가능하다는 것이다. 같은 트랜잭션을 반복한다면 잔고가 있는 한 반복해서 이더를 인출할 수 있게 된다. 이러한 반복 실행을 막기 위해 이더리움이 선택한 방법이 논스다.
</span>

<span style="font-size:15px;">
nonce는 중복되지 않고 순차적이기 때문에, 같은 nonce 에 여러 트랜잭션 전송이 발생하였다면 해당 nonce 중 제일 높은 가스비를 지불한 트랜잭션이 처리된다. 이더리움에서는 이러한 방법으로 이중 지불 문제를 방지한다.
</span>

<span style="font-size:15px;">
UTXO는 새로 생성하는 것이지만 ACCOUNT는 모델을 바꿔준다 from to value <br>
이더리움 백서 state 참조 
</span>

### <span style="font-size:1em;">🔥 nonce 를 통해 트랜잭션을 취소할 수 있을까?</span>

<span style="font-size:15px;">
트랜잭션이 네트워크에 정상적으로 전파되었다면 취소하는 방법은 존재하지 않는다. 그러나 nonce 를 기존보다 높게 설정되었거나, 너무 낮은 가스를 지불하였을 경우 아직 트랜잭션 풀에 남아있는 상태(Pending)라면 nonce를 이용하여 해당 트랜잭션을 재전송하여 취소된 효과를 볼 수 있다.
</span>
