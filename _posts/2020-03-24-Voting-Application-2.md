---

title : "투표 DApp 구현하기 2부 - 스마트 컨트랙트, DApp 만들기"
excerpt : "인프런(inflearn.com)의 이더리움(Ethereum) & 솔리디티(Solidity) 기반의 투표 DApp 구현하기를 수강하며 정리한 포스팅. 컨트랙트를 작성하고, 블록체인에 배포 그리고 상호작용까지"

categories:
  - Ethereum
  - Blockchain
  - Solidity
  - DApp

tags:
  - Ethereum

---

<br/>

투표 DApp 구현하기
-------------------

<br/>

### 솔리디티로 첫 컨트랙트 작성

컨트랙트에 필요한 **3가지 기능**
```
각 후보자들을 초기화한다.
후보자에 대해 투표할 수 있다.
각 후보자에 대한 득표수를 확인할 수 있어야 한다.
```

<br/>

[리믹스](remix.ethereum.org)에 접속한 다음, 미리 적혀있던 예제코드는 무시하고 새 컨트랙트 파일을 만든다.
솔리디티 컨트랙트 파일의 확장자는 **.sol**

컴파일러 **버전은 0.4.23** 으로 해주고, **Auto compile에 체크** 해준다.


컨트랙트에서 첫 번째로 써야되는 코드는 어떤 컴파일러 버전을 이 컨트랙트에 적용할 것인가이다. 만약, 다른 버전 컴파일러를 쓴다면 컴파일이 되지 않는다.


<br/>

* * *

<br/>


### 3가지 기능을 포함한 컨트랙트 작성

컴파일러 버전 오류로 인해 약간의 **수정** 이 있음!!
```
pragma solidity ^0.6.4;

contract Voting{

    bytes32[] public candidateList;
    constructor(bytes32[] memory candidateNames) public {
        candidateList = candidateNames;
    }

    mapping (bytes32 => uint8) public votesReceived;

    function voteForCandidate(bytes32 candidate) public {
        require(validCandidate(candidate));
        votesReceived[candidate] += 1;
    }
    function totalVotesFor(bytes32 candidate) view public returns(uint8){
        require(validCandidate(candidate));
        return votesReceived[candidate];
    }
    function validCandidate(bytes32 candidate) view public returns (bool){
        for(uint i=0; i < candidateList.length; i++){
            if(candidateList[i] == candidate){
                return true;
            }
        }
        return false;
    }

}
```

![voting-2](/assets/voting-2_rhirt0hhe.png)

<br/>

### 컨트랙트 컴파일하기

작성한 컨트랙트 코드를 복사한 뒤, **터미널** (terminal)을 실행
terminal로 프로젝트 폴더 아래에 **Voting.sol 파일을 생성** 한다.
```
$ vi Voting.sol
```

![terminal-1](/assets/terminal-1.png)

**esc + :wq** 입력 (저장하고 vim 나가기)

터미널로 돌아와 node.js를 **chapter1 디렉토리에서 실행** 한다.
```
$ node_modules/.bin/solcjs --bin --abi Voting.sol
```

<br/>

* * *

<br/>

### 블록체인에 배포하는 단계

<br/>

ABI 정의를 전달해서 VotingContract라는 객체를 생성한다.
```
$ node //노드에 진입해서
> Web3 = require('web3')
> web3 = new Web3("http://localhost:8545") //.json 형식의 파일이 나옴
> bytecode = fs.readFileSync('./Voting_sol_Voting.bin').toString()
```

여기까지 ABI정의 완료!

```
> abi = fs.readFileSync('./Voting_sol_Voting.abi').toString()  //abi 정의를 전달
```

![bytecode](/assets/bytecode.png)

블록체인에 배포하는 건 **바이트코드**!!
이 바이트코드는 Voting 컨트랙트를 컴파일한 결과물이다.

```
> abi = JSON.parse(fs.readFileSync('./Voting_sol_Voting.abi').toString())
> deployedContract = new web3.eth.contract(abi)  //강의에는 eth.Contract라고 되어있음 -> 오류발생
```
Web3 eth contract is not a constructor **오류** 참고 [페이지](https://ethereum.stackexchange.com/questions/72689/uncaught-typeerror-web3-eth-contract-is-not-a-constructor)
