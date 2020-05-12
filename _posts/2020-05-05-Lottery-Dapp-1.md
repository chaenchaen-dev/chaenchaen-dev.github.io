---

title : "Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 1부 - 개발환경 세팅하기 (20.05.12 수정)"
excerpt : "인프런(inflearn.com)의 블록체인 'Ethereum 실전! 초보자를 위한 Lottery Dapp 개발' 강의를 수강하며 정리한 포스팅. Truffle project 세팅부터 Lottery 개발, UI 까지의 과정을 포함한다."

categories:
  - Ethereum
  - DApp
  - Truffle

tags:
  - DApp

---

<br/>

Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 1부
-------------------

<br/>

### 개발환경 세팅하기

설치가 필요한 목록 체크

1. node.js (8버전 이상이면 문제 없음)
- 홈페이지에서 직접 다운로드, 또는 **homebrew**(추천) 를 통한 다운로드

```
~ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
오류추가
```
~ brew install nvm
```
brew로 nvm 설치했을 때, nvm -v에서 nvm을 찾을 수 없다고 하는 문제
![nvmerror](/assets/pic/200512/nvmerror.png)
```
~ mkdir ~/.nvm    //디렉토리 만들고
~ export NVM_DIR= ~~~~~   //어쩌고 저쩌고 나오는거 모두 복사
~ vi ~./zshrc   //vi 편집자로 복사한 내용 붙여넣기 bash 쓰는 사람은 bash_profile 에 추가
~ . ~/.zshrc    //편집한 내용 활성화
~ nvm -v
```
![nvm_success](/assets/pic/200512/nvm_success.png){: width="70%"}


2. vscode (Visual Studio Code)
- [홈페이지](https://code.visualstudio.com/)에서 다운로드

3. truffle
- 블로그의 블록체인 Dapp 개발에 트러플 활용하기 1부 - [Mac OS 트러플 설치하기](https://chaenchaen-dev.github.io/truffle/blockchain/react/dapp/Dapp-Truffle-blockchain-basic-1/) 포스팅 참고!

4. ganache-cil
- 바로 블록체인을 사용할 수 있게 해주는 프로그램으로 터미널에서 아래의 명령어로 설치한다.
```
npm -g install ganache-cil
```

5. vscode - solidity extension
- vscode에서 solidity 확장자 설치

<br/>

<br/>

<br/>
