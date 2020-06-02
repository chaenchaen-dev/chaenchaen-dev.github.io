---

title : "Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 5부 - Lottery 컨트랙트 개발 3"
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

Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 5부
-------------------

<br/>

### Distribute 함수 구현

```
function distribute() public {
  // head 3 4 5 6 7 8 9 10 11 12 tail // 큐 - 새로운 정보는 tail방향 부터 추가
  uint256 cur; // head 부터 tail 방향으로 도는 루프

  BetInfo memory b;
  BlockStatus currentBlockStatus; // 현재 BlockStatus

  for(cur = _head; cur < _tail ; cur++) {
      b = _bets[cur]; // 베팅 info 불러옴
      currentBlockStatus = getBlockStatus(b.answerBlockNumber);
      // Checkable : block.number > AnswerBlockNumber && block.number  <  BLOCK_LIMIT + AnswerBlockNumber 1
      // 정답을 체크할 수 있는 상태
      if(currentBlockStatus == BlockStatus.Checkable) {
          // if win, bettor gets pot

          // if fail, bettor's money goes pot

          // if draw, refund bettor's money

       }

       // Not Revealed : block.number <= AnswerBlockNumber 2
       if(currentBlockStatus == BlockStatus.NotRevealed) {
           break;
       }

       // Block Limit Passed : block.number >= AnswerBlockNumber + BLOCK_LIMIT 3
       if(currentBlockStatus == BlockStatus.BlockLimitPassed) {
           // refund
           // emit refund
       }

       popBet(cur);
   }
}
```

### BlockStatus() 구현

```
enum BlockStatus {Checkable, NotRevealed, BlockLimitPassed}

function getBlockStatus(uint256 answerBlockNumber) internal view returns (BlockStatus) { // BlockStatus 리턴
    if(block.number > answerBlockNumber && block.number  <  BLOCK_LIMIT + answerBlockNumber) {
        return BlockStatus.Checkable;
    }

    if(block.number <= answerBlockNumber) {
        return BlockStatus.NotRevealed;
    }

    if(block.number >= answerBlockNumber + BLOCK_LIMIT) {
         return BlockStatus.BlockLimitPassed;
    }

    return BlockStatus.BlockLimitPassed; //환불

  }  //getBlockStatus

```

### isMatch() 구현

```
/**
* @dev 베팅글자와 정답을 확인한다.
* @param challenges 베팅 글자
* @param answer 블락해쉬
* @return 정답결과
*/
function isMatch(byte challenges, bytes32 answer) public pure returns (BettingResult) {
    // challenges 0xab
    // answer 0xab......ff 32 bytes

    byte c1 = challenges;
    byte c2 = challenges;

    byte a1 = answer[0];
    byte a2 = answer[0];

    // Get first number
    c1 = c1 >> 4; // 0xab -> 0x0a
    c1 = c1 << 4; // 0x0a -> 0xa0

    a1 = a1 >> 4;
    a1 = a1 << 4;

    // Get Second number
    c2 = c2 << 4; // 0xab -> 0xb0
    c2 = c2 >> 4; // 0xb0 -> 0x0b

    a2 = a2 << 4;
    a2 = a2 >> 4;

    if(a1 == c1 && a2 == c2) {
        return BettingResult.Win;
    }

    if(a1 == c1 || a2 == c2) {
        return BettingResult.Draw;
    }

    return BettingResult.Fail;

}
```

여기까지 작성 후 컴파일로 확인

```
~ truffle compile
```

컴파일 결과화면
![compile_result](/assets/compile_result.png)

isMatch() 테스트를 위해서 lottery.test.js 파일에 코드 추가 & 수정

```
describe.only('isMatch', function () {
    it('should be BettingResult.Win when two characters match', async () => {
        let blockHash = '0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86'
        let matchingResult = await lottery.isMatch('0xab', blockHash);
        assert.equal(matchingResult, 1);
     })
}) //isMatch()
```
테스트 결과화면

![isMatch_test](/assets/isMatch_test.png)
성공!

isMatch() 경우의 수 추가

```
describe.only('isMatch', function () {
        let blockHash = '0xab03e5d54fb802fb0dc3ef6100aed0dad20c72b7a27a53f6c9e0e15cd1c0aba2';
        it('should be BettingResult.Win when two characters match', async () => {

            let matchingResult = await lottery.isMatch('0xab', blockHash);
            assert.equal(matchingResult, 1);
         })

         it('should be BettingResult.Fail when two characters match', async () => {
            let matchingResult = await lottery.isMatch('0xcd', blockHash);
            assert.equal(matchingResult, 0);
        })

        it('should be BettingResult.Draw when two characters match', async () => {
            let matchingResult = await lottery.isMatch('0xaf', blockHash);
            assert.equal(matchingResult, 2);

            matchingResult = await lottery.isMatch('0xfb', blockHash);
            assert.equal(matchingResult, 2);
        })
    }) //isMatch()
```
![match_test](/assets/match_test.png)

test 성공!
