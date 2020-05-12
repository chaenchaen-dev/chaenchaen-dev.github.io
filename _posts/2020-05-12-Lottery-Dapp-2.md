---

title : "Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 2부 - Truffle Project 세팅하기"
excerpt : "인프런(inflearn.com)의 블록체인 'Ethereum 실전! 초보자를 위한 Lottery Dapp 개발' 강의를 수강하며 정리한 포스팅. Truffle project 세팅부터 Lottery 개발, UI 까지의 과정을 포함한다."

categories:
  - Ethereum
  - DApp
  - Truffle
  - Solidity

tags:
  - DApp
  - Truffle
  - Solidity

---

<br/>

Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 2부
-------------------

<br/>

### Truffle Project 세팅하기

```
~ truffle init    //작업하려는 폴더에서 init 명령 실행
~ ls -al    //새로 생긴 파일 확인
```
![truffle_init](/assets/pic/200512/truffle_init.png)

</br>

***

</br>

#### Visual Studio에서 project 열기

1. contracts 에 Lottery.sol 파일 추가
- Lottery.sol에 코드 추가
  ```
  pragma solidity >=0.4.21 <0.7.0;
  contract Lottery{
  }
  ```
2. migratios 폴더에 1_initial_migration.js 파일 복사해서 붙여넣기 후 파일명 2_deploy_smart_contract.js로 변경

3. control + \` 단축키를 이용해 visual studio에서 terminal 열기
4. 터미널에 truffle compile 입력
5. truffle-config.js 파일 수정
- development 주석 제거 (로컬에 배포하기 위해서)
6. truffle migrate
- 재시도 했을 경우 truffle migrate --reset

</br>

***

</br>

#### 배포완료 결과 화면

![deploy_result](/assets/pic/200512/deploy_result.png)

</br>
</br>
</br>
