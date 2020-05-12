---

title : "Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 3부 - Lottery 컨트랙트 개발"
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

Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 3부
-------------------

<br/>

### Lottery 규칙에 맞게 코드 작성

!!규칙 캡처 삽입예정!!

**Lottery.sol** 코드 수정
```
pragma solidity >=0.4.21 <0.7.0;


contract Lottery{

    struct BetInfo{
        unit256 answerBlockNumber;
        address payable better;
        byte challenges;
    }

    address public owner;
    uint256 private _pot;

    constructor() public {
        owner = msg.sender;
    }

    function getSomeValue( ) public pure returns (uint256 value){
        return 5;
    }

    function getPot() public view returns (uint256 pot){
        return _pot;
    }

}
```

**lottery.test.js** 코드 추가
```
//only 사용시 모든 테스트 코드를 읽지 않고 only 부분만 테스트함
    it.only('getPot should return current pot', async () => {
        let pot = await lottery.getPot();
        assert.equal(pot, 0)
    })
```
![test4](/assets/pic/200512/test4.png){: width="70%"}


<br/>

* * *

<br/>

### Lottery Domain 및 Queue 설계

**Lottery.sol** 코드

```
pragma solidity >=0.4.21 <0.7.0;

 contract Lottery {

    struct BetInfo {
       uint256 answerBlockNumber;
       address payable bettor;
       byte challenges;
    }

   uint256 private _tail;
   uint256 private _head;
   mapping (uint256 => BetInfo) private _bets;

   uint256 constant internal BLOCK_LIMIT = 256;
   uint256 constant internal BET_BLOCK_INTERVAL = 3;
   uint256 constant internal BET_AMOUNT = 5 * 10 ** 15;
   uint256 private _pot;

    address public owner;

    constructor() public {
       owner = msg.sender;
   }

   function getSomeValue() public pure returns (uint256 value){
       return 5;
   }

   function getPot() public view returns (uint256 pot) {
       return _pot;
   }

    function getBetInfo(uint256 index) public view returns (uint256 answerBlockNumber, address bettor, byte challenges) {
       BetInfo memory b = _bets[index];
       answerBlockNumber = b.answerBlockNumber;
       bettor = b.bettor;
       challenges = b.challenges;
   }

   function pushBet(byte challenges) internal returns (bool) {
       BetInfo memory b;
       b.bettor = msg.sender; // 20 byte
       b.answerBlockNumber = block.number + BET_BLOCK_INTERVAL;
       b.challenges = challenges;

       _bets[_tail] = b;
       _tail++;

       return true;
   }

   function popBet(uint256 index) internal returns (bool) {
       delete _bets[index]; //일정량의 가스를 돌려받음
       return true;
   }
 }
```

<br/>

* * *

<br/>

### Bet 함수 구현

Bet function이 해줘야 할 일
1. Queue에 Bet 정보 확인
2. 돈이 들어왔는지 확인
3. event log 기록

Lottery.sol 에 **Bet** 함수 **추가**
* getSomeValue, test.js에서 basic test 삭제
* @dev 베팅을 한다. 유저는 0.005 ETH를 보내야 하고, 베팅용 1 byte 글자를 보낸다.
* 큐에 저장된 베팅 정보는 이후 distribute 함수에서 해결된다.
* @param challenges 유저가 베팅하는 글자
* @return 함수가 잘 수행되었는지 확인해는 bool 값

```
//event log
event BET(uint256 index, address indexed bettor, uint256 amount, byte challenges, uint256 answerBlockNumber);

function bet(byte challenges) public payable returns(bool result){
     // Check the proper ether is sent
    require(msg.value == BET_AMOUNT, "Not enough ETH");

    // Push bet to the queue
    require(pushBet(challenges), "Fail to add a new Bet Info");

    // Emit event
    emit BET(_tail - 1, msg.sender, msg.value, challenges, block.number + BET_BLOCK_INTERVAL);

    return true;
 }

```

<br/>

여기까지 수행한 후 터미널에서 **컴파일**

```
~ truffle compile
```

![compile1](/assets/pic/200512/compile1.png){: width="60%"}

<br/>

* * *

<br/>

### Bet 함수 테스트

lottery.test.js 테스트 코드 추가

```
    describe('Bet', function () {
        it.only('should fail when the bet money is not 0.005 ETH', async () => {
            // Fail transaction
            await assertRevert(lottery.bet('0xab', {from : user1, value:4000000000000000})); // 0.004 ETH
            // transaction object {chainId, value, to, from, gas(Limit), gasPrice}
        })
        it('should put the bet to the bet queue with 1 bet', async () => {
            await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.005 ETH success
            // bet

            // check contract balance

            // check bet info

            // check log

        })
    })
```

asserRevert.js 파일 test 폴더에 생성 & 코드 작성

```

module.exports = async (promise) => {
    try {
        await promise;
        assert.fail('Expected revert not received');
    } catch (error) {
        const revertFound = error.message.search('revert') >= 0;
        assert(revertFound, `Expected "revert", got ${error} instead`);
    }
}

```

![test5](/assets/pic/200512/test5.png){: width="70%"}

<br/>

* * *

<br/>



### Receipt 값 출력해서 확인하기

**lottery.test.js** 에서 **describe** 수정 후 **테스트**

```
    describe('Bet', function () {
        it('should fail when the bet money is not 0.005 ETH', async () => { //돈이 적절히 들어왔는지 확인
            // Fail transaction
            await assertRevert(lottery.bet('0xab', {from : user1, value:4000000000000000})); // 0.004 ETH
            // transaction object {chainId, value, to, from, gas(Limit), gasPrice}
        })
        it.only('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
            // await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.005 ETH success
            // bet
            let receipt = await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.005 ETH
            console.log(receipt); //receipt 확인해보기
            // check contract balance

            // check bet info

            // check log

        })
    })
```
![receipt_test](/assets/pic/200512/receipt_test.png)

receipt 안의 값이 어떤 것이 있는지 확인할 수 있다.


<br/>
<br/>
<br/>
