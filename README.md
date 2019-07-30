# EthereumToy

- geth version : Version: 1.9.0
- Ubuntu 18.04 LTS

---

## 초기화

- genesis.json
<pre><code>{
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
</code></pre>  

- geth 초기화
<pre><code>geth --datadir /home/"User"/data_testnet init /home/"User"/data_testnet/genesis.json
</code></pre>  

- geth 가동
<pre><code>geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/"User"/data_testnet console 2>> /home/"User"/data_testnet/geth.log`
</code></pre>  

--networkid 4649 : 네트워크 식별자

--nodiscover : 생성자의 노드를 다른 노드에서 검색할 수 없게 하는 옵션.

--maxpeers 0 : 생성자의 노드에 연결할 수 있는 노드의 수 지정

--datadir /home/"User"/data_testnet : 데이터 디렉터리 지정 (미지정시 기본 디렉터리 사용)

console : 대화형 자바스크립트 콘솔 기동

2>> /home/"User"/data_testnet/geth.log : 로그파일을 만들기 위한 옵션. geth가 아닌 리눅스 셸 명령어

- 콘솔 종료
<pre><code>exit</code></pre>  

---

## 계정생성

+ EOA(Externally Owned Account) 생성 (패스워드)
* EOA : 일반 사용자가 사용하는 계정으로, 비밀키로 관리된다. Ether를 송금하거나 계약을 실행할 수 있다.
* Contract 계정 : 이름처럼 계약용 계정으로, 계약을 블록체인에 배포할 때 만들어지는 계정으로 블록체인에 존재한다. 다른 계정으로부터 메시지를 수신해 코드를 실행하고 계정에 메시지를 보낼 수 있다.
<pre><code>personal.newAccount("password")</code></pre>

- 계정 리스트 확인
<pre><code>eth.accounts</code></pre>  

- 인덱스로 계정 확인
<pre><code>eth.accounts[0]</code></pre>  

---

## 채굴

- 채굴 보상을 받는 계정(etherbase) 확인
<pre><code>eth.coinbase</code></pre>  

- etherbase 변경
<pre><code>miner.setEtherbase(eth.accounts[1])</code></pre>  

- 블록체인 블록 수 확인
<pre><code>eth.blockNumber</code></pre>  

- 채굴 시작(사용할 스레드 수)
<pre><code>miner.start(1)</code></pre>  

- 채굴중인 로그 파일 확인
<pre><code>tail -100f ~/data_testnet/geth.log</code></pre>  

- 채굴중인지 여부 확인
<pre><code>eth.mining</code></pre>  

- 해시 속도 확인 (초당 해시 값 연산 수, 채굴중일시 1 이상)
<pre><code>eth.hashrate</code></pre>  

- 채굴 중지
<pre><code>miner.stop()</code></pre>  

- 잔고 확인 (coinbase, 단위는 wei,  1 ether = 1,000,000,000,000,000,000 wei )
<pre><code>eth.getBalance(eth.coinbase)</pre></code>
<pre><code>eth.getBalance(eth.accounts[0])</pre></code>  

- 화폐단위 ether로 변환
<pre><code>web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")</code></pre>  
