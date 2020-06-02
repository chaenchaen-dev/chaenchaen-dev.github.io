---

title : "Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 4부 - Lottery 컨트랙트 개발 2"
excerpt : "인프런(inflearn.com)의 블록체인 'Ethereum 실전! 초보자를 위한 Lottery Dapp 개발' 강의를 수강하며 정리한 포스팅. Truffle project 세팅부터 Lottery 개발, UI 까지의 과정을 포함한다."

categories:
  - Ethereum
  - DApp
  - Truffle
  - Solidity
  - Lottery

tags:
  - DApp
  - Truffle
  - Solidity

---

<br/>

Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 4부
-------------------

<br/>

### Balance, Bet, Log 체크하기

- 실행하기 전 잊어버리지 말고 가나슈 실행 후 truffle 사용하기
```
~ which ganache-cli
~ // 위의 명령어로 출력된 가나슈의 경로를 복사해 붙임
```

#### Balance Check
코드 추가
```
let contractBalance = await web3.eth.getBalance(lottery.address);
assert.equal(contractBalance, 5000000000000000)
```
![balancecheck](/assets/balancecheck.png)
```
~ truffle test test/lottery.test.js
```
![balance_test_success](/assets/balance_test_success.png)
테스트 성공!

```
let betAmount = 5 * 10 * 15; //contract에서 let lottery 밑에 추가
```

betAmount 변수 추가 후 5000000000000000을 betAmount로 변경


![not_enough_ETH](/assets/not_enough_ETH.png)

테스트 잘 하다가 갑자기 이더 부족 에러
계좌 잔액 확인 결과 100이더씩 있음에도 에러뜸
