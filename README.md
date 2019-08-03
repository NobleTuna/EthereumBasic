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
<pre><code>geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/"User"/data_testnet console 2>> /home/"User"/data_testnet/geth.log
</code></pre>  

`--networkid 4649` : 네트워크 식별자

`--nodiscover` : 생성자의 노드를 다른 노드에서 검색할 수 없게 하는 옵션.

`--maxpeers 0` : 생성자의 노드에 연결할 수 있는 노드의 수 지정

`--datadir /home/"User"/data_testnet` : 데이터 디렉터리 지정 (미지정시 기본 디렉터리 사용)

`console` : 대화형 자바스크립트 콘솔 기동

`2>> /home/"User"/data_testnet/geth.log :` 로그파일을 만들기 위한 옵션. geth가 아닌 리눅스 셸 명령어  
  

- 콘솔 종료
<pre><code>exit</code></pre>  

---

## 계정생성

* EOA : 일반 사용자가 사용하는 계정으로, 비밀키로 관리된다. Ether를 송금하거나 계약을 실행할 수 있다.
* Contract 계정 : 이름처럼 계약용 계정으로, 계약을 블록체인에 배포할 때 만들어지는 계정으로 블록체인에 존재한다. 다른 계정으로부터 메시지를 수신해 코드를 실행하고 계정에 메시지를 보낼 수 있다.
+ EOA(Externally Owned Account) 생성 (패스워드)
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

## 송금

- 트랜잭션 발행은 유료(from에 지정된 주소가 수수료를 지불)이므로 기본적으로 잠금상태. 언락시 해제 유효시간은 기본적으로 300초
- 트랜잭션 발행 후 팬딩중인 트랜잭션은 채굴이 진행되어야 송금됨


- 계정 잠금 해제
<pre><code>personal.unlockAccount(eth.accounts[0], "password")</code></pre>


- 송금 (발행한 트랜잭션 ID 출력)
<pre><code>eth.sendTransaction({from:eth.accounts[0], to:eth.accounts[1], value:web3.toWei(10,"ether")})</code></pre>


- 트랜잭션 확인
<pre><code>eth.getTransaction("transaction id")</code></pre>


- 계류 중인 트랜잭션 확인
<pre><code>eth.pendingTransactions</code></pre>


- 블록 확인
<pre><code>eth.getBlock("blockNumber")</code></pre>


## 백그라운드로 Geth 기동

<pre><code>nohup geth --networkid 4649 --datadir /home/"User"/data_testnet/ --mine -minerthreads 1 --rpc &</code></pre>

`--nohup` : SIGHUP을 무시한 상태로 프로세스 기동. 셸로부터 SIGHUP이 전송돼도 무시하기 떄문에 로그아웃 후에도 프로세스가 종료되지 않는다. 중지하기 위해서는 kill 명령어를 사용한다.

`--mine` : 채굴활성화

`--minerthreads 1` : 채굴에 사용할 CPU 스레드 수. 기본값 1

`--rpc` : HTTP-RPC 서버를 활성화. 별도의 콘솔에 연결할 때 필요한 옵션

`&` : 이 명령을 백그라운드에서 실행
  


- Geth 콘솔 접속
<pre><code>geth attach rpc:http://localhost:8545</code></pre>


- 채굴작업 확인
<pre><code>eth.mining</code></pre>


- 콘솔 종료 후 geth 프로세스 확인
<pre><code>exit</code></pre>
<pre><code>ps -eaf | grep geth</code></pre>


- 프로세스 kill
<pre><code>kill "process id"</code></pre>


## JSON-RPC

- RPC(remote procedure call) : 별도의 원격 제어를 위한 코딩 없이 다른 주소 공간에서 함수나 프로시저를 실행할 수 있게하는 프로세스 간 통신 기술이다. 다시 말해, 원격 프로시저 호출을 이용하면 프로그래머는 함수가 실행 프로그램에 로컬 위치에 있든 원격 위치에 있든 동일한 코드를 이용할 수 있다.

<pre><code>nohup geth --networkid 4649 --nodiscover --maxpeers 0 --datadir /home/lyy7661/data_testnet/ --mine --minerthreads 1 --rpc --rpcaddr "0.0.0.0" --rpcport 8545 --rpccorsdomain "*" --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" 2>> /home/lyy7661/data_testnet/geth.log &</code></pre>

`--rpc` : HTTP-RPC 서버를 활성화 한다

`--rpcaddr` "0.0.0.0" : HTTP-RPC 서버의 수신 IP를 지정한다. 기본값은 "localhost"다. "0.0.0.0"을 지정하면 localhost뿐만 아니라 어떤 인터페이스에 대해 접근해도 수신한다.

`--rpcport 8545` : HTTP-RPC 서버가 요청을 받기 위해 사용하는 포트를 지정한다. 기본 포트 번호는 8545다.

`--rpccorsdomain "*"` : 자신의 노드에 RPC로 접속할 IP주조를 지정한다. 쉼표로 구분해 여러개를 지정할 수 있다. `"*"`로 지정하면 모든 IP에서 접속을 허용한다.

`--rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3"` : RPC를 허가할 명령을 지정한다. 쉼표로 구분해 여러 개를 지정할 수 있다. 기본값은 "eth, net, web3" 이다.
  
- 계정생성  
<pre><code>curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0", "method":"personal_newAccount", "params":["pass3"], "id":10}' localhost:8545</code></pre>


**invalid content type, only application/json is supported 에러 발생시 `-H "Content-Type: application/json"`를 붙여 헤더를 수동으로 설정해줘야 한다.**
(https://ethereum.stackexchange.com/questions/30651/geth-response-invalid-content-type-only-application-json-is-supported)



- 계정목록 표시 (method : personal_listAccounts)
<pre><code>curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0", "method":"personal_listAccounts", "params":[], "id":10}' localhost:8545</code></pre>


- 현재 채굴 여부 표시
<pre><code>curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0", "method":"eth_mining", "params":[], "id":10}' localhost:8545</code></pre>
  

- 해시 속도 확인 eth_hashrate
<pre><code>curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0", "method":"eth_hashrate", "params":[], "id":10}' localhost:8545</code></pre>


- 10진수로 변경(셸 명령어)
<pre><code>printf '%d\n' "0x104fa"</code></pre>
  

- 블록 번호 확인 (method : eth_blockNumber, 16진수로 응답)
<pre><code>curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0", "method":"eth_blockNumber", "params":[], "id":10}' localhost:8545</code></pre>
  
  
- 계좌 잔고 확인 (method : eth_getBalance, 16진수로 응답)
<pre><code>curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0", "method":"eth_getBalance", "params":["0x824f65aa23d148015933cc024b4aa15f5c46d337", "latest"], "id":10 }' localhost:8545</code></pre>
