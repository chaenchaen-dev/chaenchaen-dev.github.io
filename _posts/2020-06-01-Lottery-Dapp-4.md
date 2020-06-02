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
계좌 잔액 확인 결과 100이더씩 있음에도 에러
-> 갑자기 정상작동

lottery.test.js Bet 부분 수정

```
describe('Bet', function () {
        it('should fail when the bet money is not 0.005 ETH', async () => { //돈이 적절히 들어왔는지 확인
            // Fail transaction
            await lottery.bet('0xab', {from : user1, value:4000000000000000}); // 0.004 ETH
            // transaction object {chainId, value, to, from, gas(Limit), gasPrice}
        })
        it.only('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
            // bet
            let receipt = await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.005 ETH
            //console.log(receipt);

            let pot = await lottery.getPot();
            assert.equal(pot, 0);
            // check contract balance
            let contractBalance = await web3.eth.getBalance(lottery.address);
            assert.equal(contractBalance, 5000000000000000)

            // check bet info
            let currentBlockNumber = await web3.eth.getBlockNumber();


            let bet = await lottery.getBetInfo(0);
            assert.equal(bet.answerBlockNumber, currentBlockNumber + bet_block_interval); // answerBlockNumber 확인
            assert.equal(bet.bettor, user1); // bettor 확인
            assert.equal(bet.challenges, '0xab') // challenges 확인
            // check log

       })
    })
```

test 결과화면
![bet_sucess](/assets/bet_sucess.png)

<br/>

***

<br/>

#### Log가 제대로 찍히는지 확인
lottery.test.js 에 코드 추가
```
console.log(receipt)
```

test 폴더에 expectEvent.js 파일 생성
```
//expectEvent.js 에 작성
//이벤트가 없으면 에러를 출력함

const assert = require('chai').assert;

const inLogs = async (logs, eventName) => {
    const event = logs.find(e => e.event === eventName);
    assert.exists(event);
}

module.exports = {
    inLogs
}
```


lottery.test.js에 expectEvent 추가
```
const expectEvent = require('./expectEvent')
```

lottery.test.js 수정

```
describe('Bet', function () {
        it('should fail when the bet money is not 0.005 ETH', async () => { //돈이 적절히 들어왔는지 확인
            // Fail transaction
            await lottery.bet('0xab', {from : user1, value:4000000000000000}); // 0.004 ETH
            // transaction object {chainId, value, to, from, gas(Limit), gasPrice}
        })
        it.only('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
            // bet
            let receipt = await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.005 ETH
            //console.log(receipt);

            let pot = await lottery.getPot();
            assert.equal(pot, 0);
            // check contract balance
            let contractBalance = await web3.eth.getBalance(lottery.address);
            assert.equal(contractBalance, 5000000000000000)

            // check bet info
            let currentBlockNumber = await web3.eth.getBlockNumber();


            let bet = await lottery.getBetInfo(0);
            assert.equal(bet.answerBlockNumber, currentBlockNumber + bet_block_interval); // answerBlockNumber 확인
            assert.equal(bet.bettor, user1); // bettor 확인
            assert.equal(bet.challenges, '0xab') // challenges 확인
            // check log

            //console.log(receipt);

            await expectEvent.inLogs(receipt.logs, 'BET');

       })
    })
```

#### test 결과 오류 발생
- Error: Cannot find module 'chai'
- 해결방법
    ```
    lottery-smart-contract ~ npm install chai
    ```
![chai](/assets/chai.png)

다시 truffle test test/lottery.test.js

![chai_success](/assets/chai_success.png)

여기까지 **bet function** 테스트 **완료** !!

<br/>



<br/>


<br/>
