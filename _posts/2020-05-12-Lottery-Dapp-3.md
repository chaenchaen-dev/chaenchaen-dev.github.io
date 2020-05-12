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

Lottery.sol 코드

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



<br/>
<br/>
<br/>
