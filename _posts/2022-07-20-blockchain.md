---
layout: single
title: "[블록체인 이론] 블록체인 암호학"
categories: Blockchain
tags: [Blockchain]

toc: true
toc_sticky: true

date: 2022-07-20

permalink: /blockchain/cryptography/
---

## <span style="font-size:1.125em;">🚀 블록체인 암호학</span>

<span style="font-size:15px;">
블록체인에서 암호학은 탈중앙화 참여자를 위한 아이덴티티로 쓸 수 있는 어카운트 주소를 만들어 내는 핵심적인 역할을 한다.<br>
</span>

- <span style="font-size:15px;">
  탈중앙화 블록체인 기반 솔루션에서 암호학의 역할
  </span>
  - <span style="font-size:15px;">
    참여자와 다른 엔티티를 위한 디지털 아이덴티티를 생성  
    </span>
  - <span style="font-size:15px;">
    데이터와 트랜잭션의 보안 보장
    </span>
  - <span style="font-size:15px;">
    데이터의 프라이버시를 확립
    </span>
  - <span style="font-size:15px;">
    문서에 디지털 서명을 함
    </span>

#### <span style="font-size:1em;">🔥 대칭키 암호학</span>

<span style="font-size:15px;">
탈중앙화 시스템에서는 사용자를 식별하고 인증하기 위해 사용자명-암호 접근 방식을 사용할 수 없기 때문에 암호학적인 키 쌍을 이용한다.<br>
암호화 체계는 암호화 키와 복호화 키가 일치하는지 여부에 따라 대칭형 키 암호화 기법과 비대칭형 암호화 기법으로 나뉜다.
</span>

<span style="font-size:15px;">
대칭키 암호학은 암호화하고 이것을 푸는 과정에 같은 키를 사용한다.
</span> 

- <span style="font-size:15px;">
  송신자는 비밀키로 원문을 암호문으로 변환 후 수신자에게 암호문, 비밀키를 전송
  </span>
- <span style="font-size:15px;">
  수신자는 비밀키로 암호문을 해독
  </span>

<img width="613" src="https://user-images.githubusercontent.com/102231167/174424399-5b9751f4-8391-4754-a61d-8eb0834b9887.png"><br>
<span style="font-size:15px;">
출처: <a href="https://blog.lgcns.com/1230">LG CNS 공식 블로그</a>
</span> 


<span style="font-size:15px;">
대칭키 방식은 참여자에게 어떻게 키를 비밀리에 전달할 수 있는가 하는 문제가 있다. 이 키를 공개해 버리면 누구나 메시지를 해독할 수 있기에 탈중앙화 블록체인에서는 알지 못하는 참여자들을 고려해야 하기 때문에 이 문제가 더욱 심각해진다. 이 문제를 해결하기 위해 암호화하고 해독하는 데 쓰이는 키가 서로 다른 비대칭 방식을 사용한다.
</span> 

#### <span style="font-size:1em;">🔥 비대칭키 암호학</span>

<span style="font-size:15px;">
비대칭키 암호학은 일반적으로 공개키 암호학으로 알려져 있다. 이 방식은 대칭키 방식에서 사용했던 하나의 비밀키 대신 두개의 다른 키를 사용한다.
</span> 

- <span style="font-size:15px;">
  작동 원리 {개인키, 공개키}<br>
  입력 데이터는 함수와 개인키를 사용해 암호화 👉 암호화된 메시지는 같은 함수를 사용해 해독하지만 이번에는 공개키를 사용해 메시지를 추출
  </span>

<span style="font-size:15px;">
이와 같이 개인키로 암호화한 메시지는 공개키로 해독할 수 있다. 이로써 대칭키 방식의 키 배포 문제도 해결 가능하다. 공개키는 누구나 쓸 수 있도록 배포하되 개인키는 암호를 사용해 안전하게 별도로 보관하면 된다. 비대칭키 방식은 키 배포 문제도 해결해주지만 탈중앙화된 참여자 아이덴티티 이슈도 해결할 수 있다.
</span>

#### <span style="font-size:1em;">🔥 이더리움 주소 생성</span>

<span style="font-size:15px;">
이더리움은 외부 소유 어카운트(EOA), 스마트 컨트랙트 어카운트(SCA)로 두가지 유형의 어카운트가 있다.<br>
이더리움은 어카운트 주소를 생성할때 비대칭키 암호학 방식을 사용한다.
</span> 

- <span style="font-size:15px;">
  256비트 난수를 생성해 개인키로 설정
  </span>
- <span style="font-size:15px;">
  타원 곡선 알고리즘을 개인키에 적용해 고유한 공개키를 추출(개인키는 패스워드 보호를 받고 공개키는 외부에 공개)
  </span>
- <span style="font-size:15px;">
   공개키를 SHA256해시를 사용하여 해시화하고 다시 RIPEMD160을 이용하여 160비트 크기로 해시화한다. 이 Public Key Hash 를 Base58Check(길이가 긴 숫자열을 압축해서 표현) 인코딩을 통해 어카운트 주소를 얻는다.
  </span>

<span style="font-size:15px;">
RIPEMD란 MD4라는 해시 함수를 기반으로 개발한 암호화 해시 알고리즘으로 임의의 길이의 메시지에서 128또는 160비트의 메시지 요약을 생성하며 32비트 연산에 최적화 되어있다.
</span> 

<img width="652" src="https://user-images.githubusercontent.com/102231167/174425607-94359693-ddee-4895-be4b-f0f99fd475cc.png"><br>
<span style="font-size:15px;">
출처: <a href="https://github.com/AhnSungJoo/bitcoinbook/blob/develop/ch04.asciidoc">Mastering Bitcoin Ch04</a>
</span> 


