---

title : "투표 DApp 구현하기 1부 - 개발환경 설정 MacOS"
excerpt : "인프런(inflearn.com)의 이더리움(Ethereum) & 솔리디티(Solidity) 기반의 투표 DApp 구현하기를 수강하며 정리한 포스팅. MacOS 개발환경 설정에 관하여"

categories:
  - Ethereum
  - Blockchain
  - Solidity
  - DApp

tags:
  - Ethereum

---

<br/>

개발환경 설정 MacOS
-------------------

<br/>

개발환경 설정을 진행하기에 앞서 HomeBrew를 설치하지 않았을 경우, <https://brew.sh/>의 지침에 따라 설치 (한국어 지원)

<br/>

### **Node.js** 와 **npm** 설치
  Node.js를 다운받을 때 npm도 함께 다운받을 수 있다. (npm이 Node package Manager이기 때문에)

  > 참고블로그 <https://kdydesign.github.io/2017/07/15/nodejs-npm-tutorial/>

  MacOS Node.js [다운로드](https://nodejs.org/en/) 를 완료 했다면, Terminal에 아래 명령어를 이용하여 버전을 확인한다. **Node.js는 7버전 이상** 이어야 함
```
$ node -v
$ npm -v
```

<br/>

* * *

<br/>

### **가나슈**(Ganache), **Web3.js**, **솔리디티 컴파일러** 설치
```
$ mkdir ethereum_voting_dapp
$ cd ethereum_voting_dapp/
$ mkdir chapter1
$ cd chapter1/
```
패키지를 설치하기 위한 공간을 정하기 위해 ethereum_voting_dapp, chapter1 폴더 생성 -> **chapter1 폴더로 이동**

```
$ npm install ganache-cli web3@0.20.3 solc
```
![솔리디티 컴파일러 가나슈 웹3 결과 스샷](/assets/voting1.png)

위의 스크린샷은 3가지가 설치 되었을 시에 나오는 결과 화면

> 이 부분에서 npm 오류가 대량 발생해서 Node.js와 npm을 전체 삭제하고 새로 진행하였다. 맥에서 Node.js 삭제하는 방법 블로그 <https://gomugom.github.io/how-to-remove-node-from-macos/>

> 또 다른 오류로 WARN이 굉장히 많이 뜨는 경우에는 HomeBrew가 설치되지 않아서일 수 있다. 첫 줄 참고!

제대로 동작하는지 확인하기 위해 **가나슈를 실행**
```
$ node_modules/.bin/ganache-cli
```

terminal에 Available Accounts 10개와 각 계정에 해당하는 Private Keys 10개가 출력되었다면 성공 (각 계정에는 테스트로 100이더가 들어있음)

- 실제로 계정마다 100이더씩 들어 있는지 확인하기
  1. 터미널 탭을 하나 새로 연다
  2. ```$ cd ethereum_voting_dapp/chapter1/``` 프로젝트 디렉토리로 진입
  3. ```$ node``` 노드 콘솔 시작
  4. ```Web3 = require('web3')```
  5. ```web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"))``` 8545는 블록체인이 실행되는 포트
  6. ```web3.eth.accounts```
  7. 출력되는 계정들은 가나슈를 실행했을 때 만들어진 Available Accounts 목록과 같다.
  8. ```web3.eth.getBalance('0x00fee25262cb7E0E02d0E6fF27DB2d7c84C671B3')``` //'' 안에는 본인에게 만들어진 계좌 중 하나 사용할 것
  9. 해당되는 계좌의 잔고를 알 수 있다. ( 단위는 wei **1ETH = 10^18wei** )
    ETH 단위로 얼마인지 알고 싶다면 ```web3.fromWei('_______잔액______','ether')```을 사용하면 된다.


<br/>
<br/>
