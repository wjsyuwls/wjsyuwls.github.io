---
layout: single
title: "[블록체인 이론] 이더리움 동작 원리"
categories: Blockchain
tags: [Blockchain]

toc: true
toc_sticky: true

date: 2022-07-21

permalink: /blockchain/ethereum/
---

## <span style="font-size:1.125em;">🚀 Accounts</span>

### <span style="font-size:1em;">🔥 EOA, CA</span>

<span style="font-size:15px;">
외부 소유 계정(Externally Owned Account) : 개인 키로 제어되는 것으로서 코드를 저장할 수 없다. <br>
계약 계정(Contract Account) : 스마트 컨트랙트 코드에 의해 제어되며 특정 CA에 코드를 저장할 수 있다.
</span>

<img width="652" src="https://user-images.githubusercontent.com/108192885/175842764-2210e291-fcd1-4b4d-8055-035c2fe01019.png">

- <span style="font-size:15px;">
EOA는 자신 이외의 다른 EOA 또는 CA에 메시지를 보낼 수 있으며, 이를 위해 해당 EOA의 개인키를 사용하여 트랜잭션을 생성하고 서명한다.
두 개의 EOA 사이에서 발생하는 메시지는 단순하게 ETH만을 전송(송금)한다. EOA에서 CA로 보내는 메시지는 CA에 저장된 코드를 활성화시키고, 해당 코드에 수반되는 다양한 기능을 활성화시킨다. (토큰 전송, 내부 저장소에 작성, 새로운 토큰 mint, 계산 수행, 새로운 contract 생성 등)
</span>

- <span style="font-size:15px;">
EOA와 달리 CA는 스스로 새로운 트랜잭션을 개시할 수 없다. CA는 자신 이외의 CA, 또는 EOA로부터 받은 트랜잭션에 대한 응답으로만 트랜잭션을 실행할 수 있다.
해당 어카운트는 트랜잭션을 받을 때 마다 자신의 코드를 활성화시키고 이에 따라 데이터를 읽거나 내부 저장공간에 기록하고, 다른 트랜잭션을 보내게 된다.
</span>

<img width="400" src="https://user-images.githubusercontent.com/108192885/175849529-d7b243a2-8abb-4039-a4dc-a685999e240d.png">

<span style="font-size:15px;">
이더리움 블록체인의 모든 기능은 항상 'EOA'가 날린 트랜잭션으로부터 시작된다. CA는 어떤 트랜잭션을 '직접' 날리는 계정이 전혀 아니다.
</span>

### <span style="font-size:1em;">🔥 어카운트 구성 요소</span>

<span style="font-size:15px;">
어카운트는 EOA, CA 그 종류를 불문하고 총 4개의 요소로 구성된다.
</span>

<img width="645" src="https://user-images.githubusercontent.com/108192885/175842809-9b8a17bf-b903-4d14-aaf0-85441aca8759.png">

- <span style="font-size:15px;">
Nonce : EOA의 경우, 이 숫자는 해당 어카운트의 주소로부터 보내진 트랜잭션들의 숫자를 의미한다. CA의 경우, 이 값은 스마트 컨트랙트에서 생성한 다른 스마트 컨트랙트의 숫자를 의미한다. 두 경우 모두, Nonce는 일종의 카운터 역할을 한다. (블록 헤더에 식별자로 기록되는 Nonce와 다름. 어카운트의 nonce는 숫자 세는 용도)
</span>

- <span style="font-size:15px;">
Balance : 계정 유형에 관계없이 한 어카운트가 소유하고 있는 ETH의 양이다.
</span>

- <span style="font-size:15px;">
storageRoot : 머클 패트리샤 트리의 루트 노드를 해시한 값이다. 이 트리는 어카운트에 저장된 요소들의 해시값을 암호화하는데, 초기값은 비어 있다. (머클 패트리샤 트리는 이더리움의 TX를 보관하고 그 유효성을 효과적으로 검증하기 위한 자료 구조)
</span>

- <span style="font-size:15px;">
CodeHash : EVM(Ethereum Virtual Machine)에서 실행될 코드의 해시값이다. CA의 경우 한 어카운트에 포함된 EVM 코드의 해시값이고, EOA에서는 비어 있는 문자열의 해시값이다 (EOA에는 코드를 저장할 수 없기 때문).
</span>

## <span style="font-size:1em;">🚀 트랜잭션과 메시지</span>

<span style="font-size:15px;">
각각의 어카운트(EOA)에서 발생하는 트랜잭션들은 이더리움의 Global State를 바꾼다.<br>
트랜잭션은 메시지 호출을 일으키는 트랜잭션과 새로운 이더리움 컨트랙트를 생성하는 트랜잭션 두가지 유형이 있음.
</span>

<span style="font-size:15px;">
모든 트랜잭션은 그 종류에 관계없이 다음의 8가지 데이터 필드를 포함한다.
</span>

- <span style="font-size:15px;">
nonce : 발신자에 의해 보내진 트랜잭션의 개수로 0부터 시작<br>
특정 주소에서 보내는 트랜잭션에 할당된 번호로, 거래 전송 시마다 nonce 값이 1씩 증가하며, 한 주소에 동일한 nonce를 갖는 트랜잭션은 존재하지 않는다.
nonce는 순차적으로 증가되고 처리된다. (역행 불가)
아직 트랜잭션이 transaction pool에 남아있는 경우에 한해서, nonce를 이용해 트랜잭션을 재전송하여 취소된 효과를 볼 수 있다.
이중 지불 문제를 방지시킨다. (같은 nonce에 여러 트랜잭션이 전송된 경우, 제일 높은 가스비를 지불한 트랜잭션을 처리시킨다.)
</span>
- <span style="font-size:15px;">
gasPrice : 각 실행단계에서 지급되는 Gas 비용
</span>
- <span style="font-size:15px;">
gasLimit : 트랜잭션 수행시 지급가능한 최대가스비용
</span>
- <span style="font-size:15px;">
to : 트랜잭션 수신자의 주소. 수신자가 null로 비어있는 경우는 해당 수신자가 컨트랙트 어카운트를 의미함
</span>
- <span style="font-size:15px;">
value : 수신처로 전송할 이더의 양으로 Wei단위
</span>
- <span style="font-size:15px;">
v, r, s : 트랜잭션 발신자를 식별할 서명을 생성할 때 사용되는 변수로 ECDSA 전자서명을 만드는데 사용되는 값
</span>
- <span style="font-size:15px;">
data (메시지 콜에서만 존재) : 메시지 콜의 입력 데이터 (인자들).
</span>
- <span style="font-size:15px;">
Payload : 옵션필드로 메시지 호출시 매개변수 등이 전달.
</span>
- <span style="font-size:15px;">
init (CA에서만 존재) : 새로운 CA를 생성하는데 사용되는 EVM 코드 조각. init은 오직 한번만 수행되고 버려진다. 처음 init이 수행될 때는 account code의 body를 호출하는데, 이는 CA와 관련있는 코드 조각이다.
</span>

<img width="655" src="https://user-images.githubusercontent.com/108192885/175846728-b3aa09f9-cd53-4203-84e9-7b3ebb0d904d.png">

<span style="font-size:15px;">
트랜잭션 실행 (=State의 변화)
</span>

<img width="645" src="https://user-images.githubusercontent.com/108192885/175850547-44cb1f16-9855-4896-81fd-6dbb8eb26fee.png">

<span style="font-size:15px;">
어카운트 종류에 따라서 각각 다른 종류의 트랜잭션이 사용된다.
</span>

## <span style="font-size:1em;">🚀 World state</span>

<span style="font-size:15px;">
이더리움의 Global State는 어카운트(EOA, CA)의 주소와 어카운트 간의 (key, value) 맵핑으로 구성된다.
즉 ‘상태값’이란 이러한 맵핑의 결과값이며 이는 머클 패트리샤 트리 자료 구조에 저장된다. 이 때, 어카운트의 4가지 구성요소를 모두 포함하여 맵핑이 이루어진다.
</span>

<img width="674" src="https://user-images.githubusercontent.com/108192885/175844240-aca1ea4c-a23b-4b69-8b55-7cbb6d03af78.png">

<span style="font-size:15px;">
즉 이더리움의 상태는 머클 패트리샤 트리의 형태로 저장되고 이 때의 머클 패트리샤 트리를 State Trie라고 부름. 이더리움에서 활용되는 머클 패트리샤 트리의 종류에는 크게 3가지가 있고, State Trie가 그 중 1가지다.
</span>

- <span style="font-size:15px;">
state trie : 해당 블록을 통해서 변경된 account 정보를 가짐
</span>
- <span style="font-size:15px;">
transaction trie : 현재 block의 transaction 정보를 가짐
</span>
- <span style="font-size:15px;">
receipts trie : 현재 block의 receipt(거래 영수증) 정보를 가짐
</span>

<img width="628" src="https://user-images.githubusercontent.com/108192885/175845565-6d10e0c4-9f47-463c-91be-84a8ea0cefa0.png">

## <span style="font-size:1em;">🚀 TX Execution Model과 EVM</span>

<span style="font-size:15px;">
EVM(Ethereum Virtual machine)은 스마트 컨트랙트의 바이트 코드를 실행하는 32바이트 스택 기반의 실행 환경으로 최대 크기는 1024Byte이다. 이더리움의 각 노드는 EVM을 포함하고 있으며, EVM을 통해 컨트랙트의 바이트 코드를 Op코드로 변환 후 내부에서 실행한다.
</span>

<img width="749" src="https://user-images.githubusercontent.com/108192885/176092650-bd47c5c3-8012-4a39-a191-f4d2491fb6bb.png">

<span style="font-size:15px;">
EVM은 휘발성, 비휘발성 메모리로 구성되어 있으며, 여기에 바이트 배열 형태로 스택의 항목들을 저장한다.
</span>

- <span style="font-size:15px;">
비휘발성 (non-volatile)
</span>
  - <span style="font-size:15px;">
  storage (32Byte당 20,000Gas)
  </span>
    - <span style="font-size:15px;">
    블록체인에 영구적으로 기록하기 위한 저장 공간
    </span>
    - <span style="font-size:15px;">
    Key(32Byte)/Value(32Byte)를 mapping하기 위한 구조
    </span>
  - <span style="font-size:15px;">
  code
  </span>
    - <span style="font-size:15px;">
    스마트 컨트랙트의 컴파일된 바이트 코드가 저장
    </span>
- <span style="font-size:15px;">
volatile (휘발성) 
</span>
  - <span style="font-size:15px;">
  stack
  </span>
    - <span style="font-size:15px;">
    OP 코드를 실행하기위한 스택영역
    </span>
    - <span style="font-size:15px;">
    32Byte size로 저장
    </span>
    - <span style="font-size:15px;">
    스택의 최대 크기는 1024개로 제한
    </span>
    - <span style="font-size:15px;">
    내부 함수 호출도 마치 일반적인 연산처럼 EVM stack을 사용
    </span>
    - <span style="font-size:15px;">
    다른 컨트랙트의 method를 호출할 때 메시지 콜을 위한 스택을 사용(메시지 콜 크기도 1024로 제한)
    </span>
  - <span style="font-size:15px;">
  rom(call data)
  </span>
    - <span style="font-size:15px;">
    컨트랙트 호출시에 넘어오는 인자를 저장 (트랜잭션을 요청했을 때 전송되는 데이터들이 기록되는 저장 공간)
    </span>
  - <span style="font-size:15px;">
  Log (32Byte당 96Gas)
  </span>
    - <span style="font-size:15px;">
    스마트 컨트랙트가 실행될 때 부가적인 정보를 기록하기 위한 공간
    </span>
     - <span style="font-size:15px;">
    Log영역의 데이터를 읽으려면 web3.js를 이용하여 DApp을 개발해야 함
    </span>
  - <span style="font-size:15px;">
  memory (32Byte당 1Gas)
  </span>
    - <span style="font-size:15px;">
    함수를 호출하거나 메모리 연산을 수행할 때 임시로 사용되는 영역
    </span>
    - <span style="font-size:15px;">
    메시지 호출이 발생할 때마다 초기화된 메모리 영역이 컨트랙트에 제공
    </span>
    - <span style="font-size:15px;">
    읽기: 256비트 단위
    </span>
    - <span style="font-size:15px;">
    쓰기: 8비트 or 32비트 단위 모두 가능
    </span>

## <span style="font-size:1em;">🚀 이더리움 트랜잭션 전체 흐름</span>
<br>
<img src="https://user-images.githubusercontent.com/108192885/175840874-a098847a-128d-45fb-a5b8-fddfc336f0d8.png">

### <span style="font-size:1em;">🔥 트랜잭션 처리 순서</span>

<span style="font-size:15px;">
 &nbsp;&nbsp;&nbsp;&nbsp; 특정 주소 A -> B 로 트랜잭션을 보낸다고 할 때.
</span>

- <span style="font-size:15px;">
해당 트랜잭션에 대해 사용자 A가 자신의 개인키로 ECDSA 전자 서명 암호화시킨다.
</span>

- <span style="font-size:15px;">
이더리움 클라이언트는 해당 트랜잭션을 마이너를 포함 모든 노드들에게 브로드 캐스팅한다. 
</span>

- <span style="font-size:15px;">
마이너에서 유효성을 검증한다.
</span>
  - <span style="font-size:15px;">
  트랜잭션이 문법에 맞게 구성되었는지
  </span>
  - <span style="font-size:15px;">
  A의 공개키를 이용해 전자서명은 유효한지 (신원 검증)
  </span>
  - <span style="font-size:15px;">
  사용자A의 계정에 있는 NONCE와 일치하는지 (이중 지불 방지)
  </span>
  - <span style="font-size:15px;">
  가스비용을 계산하고, 전자서명 수신처인 사용자B의 어카운트 주소를 확인한다.
  </span>

- <span style="font-size:15px;">
노드들이 모든 검증 작업을 완료하면 해당 트랜잭션을 트랜잭션 풀에 등록한다.
</span>

- <span style="font-size:15px;">
마이너가 트랜잭션 풀에서 트랜잭션을 처리 시 가스 실행 비용이 높은 순으로 트랜잭션을 선택한다.
</span>
   - <span style="font-size:15px;">
     선택된 트랜잭션 송금 값 A 어카운트에서 B어카운트로 전송
     </span>
    - <span style="font-size:15px;">
     B의 어카운트가 컨트랙트면 컨트랙트 코드 실행
     </span>

### <span style="font-size:1em;">🔥 마이너의 트랜잭션 처리 과정</span>

- <span style="font-size:15px;">
블록에 담을 트랜잭션들을 Transaction Pool에서 가져온다.
</span>

- <span style="font-size:15px;">
트랜잭션들에 대해 Gas price와 Account별 Nonce를 기준으로 트랜잭션 처리 순서를 정한다.
</span>

- <span style="font-size:15px;">
마이너가 트랜잭션 내부에 선언된 Gas fee(Gas limit * Gas price)를 확인하고 송신 Account로 부터 Gas를 확보 (Gas price가 높을 수록 트랜잭션 채택 확률 증가)
</span>

- <span style="font-size:15px;">
B의 어카운트가 컨트랙트면 트랜잭션을 실행하며, 트랜잭션에 선언된 명령어에 따라 가스를 소모한다.
(B의 어카운트가 EOA라면 Transaction Execution 과정을 거치지 않음 그래서 그냥 트랜잭션 송금 값 B로 전송)
</span>
  - <span style="font-size:15px;">
  모든 명령 실행 후 Gas limit 남는 Gas는 해당 트랜잭션 발신 주소로 다시 돌려준다.
  </span>
  - <span style="font-size:15px;">
  가스가 모자라는 경우 트랜잭션이 Revert되며, 가스는 다시 돌려주지 않는다. (Out Of Gas Exception)
  </span>
- <span style="font-size:15px;">
이상없는 실행된 트랜잭션들을 블록에 담는다. (트랜잭션들의 모든 개별 Gas Limit의 총합은 Block Gas Limit를 넘길 수 없다.)
</span>