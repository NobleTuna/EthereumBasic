# EthereumToy

- geth version : Version: 1.9.0
- Ubuntu 18.04 LTS

---

genesis.json

<pre><code>
{
  "nonce":"0x0000000000000042",
  "timestamp":"0x0",
  "parentHash":"0x0000000000000000000000000000000000000000000000000000000000000000",
  "extraData":"0x00",
  "gasLimit":"0x80000000",
  "difficulty":"0x4000",
  "mixhash":"0x0000000000000000000000000000000000000000000000000000000000000000",
  "coinbase":"0x3333333333333333333333333333333333333333",
  "config": {
                  "chainId": 15,
                  "homesteadBlock": 0,
                  "eip155Block": 0,
                  "eip158Block": 0
  },
  "alloc":{}
}
</pre></code>


geth 초기화
<pre><code>
geth --datadir /home/"User"/data_testnet init /home/"User"/data_testnet/genesis.json
</pre></code>

geth 가동

`geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/"User"/data_testnet console 2>> /home/"User"/data_testnet/geth.log`

- --networkid 4649 : 네트워크 식별자
- --nodiscover : 생성자의 노드를 다른 노드에서 검색할 수 없게 하는 옵션.
- --maxpeers 0 : 생성자의 노드에 연결할 수 있는 노드의 수 지정
- --datadir /home/"User"/data_testnet : 데이터 디렉터리 지정 (미지정시 기본 디렉터리 사용)
- console : 대화형 자바스크립트 콘솔 기동
- 2>> /home/"User"/data_testnet/geth.log : 로그파일을 만들기 위한 옵션. geth가 아닌 리눅스 셸 명령어

---
