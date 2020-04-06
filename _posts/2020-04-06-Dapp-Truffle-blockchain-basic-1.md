---

title : "블록체인 Dapp 개발에 트러플 활용하기 1부 - Mac OS 트러플 설치하기"
excerpt : "인프런(inflearn.com)의 블록체인 'Dapp 개발에 트러플 활용하기_기본편' 수강하며 정리한 포스팅. 트러플 설치부터 스마트 컨트랙트, 로컬에 배포하기, Rinkeby에 배포하기, 단위테스트, 트러플 리액트 박스 열어보기, 리액트 애플리케이션과 결합하기를 포함한다."

categories:
  - Truffle
  - Blockchain
  - React
  - DApp

tags:
  - Truffle
  - Blockchain
  - React
  - DApp

---

<br/>

블록체인 Dapp 개발에 트러플 활용하기 1부
-------------------

<br/>

### 트러플 개발환경

Mac OS를 기준으로 **트러플 설치**
**node** 와 **npm** 다운로드가 **완료된 상태** 가정하에 트러플 설치를 시작한다.

먼저, 터미널에서 **node** 버전을 확인 후 Truffle을 설치한다.

```
$ node -v
$ npm install -g truffle
$ truffle version
```

![node설치확인](/assets/node설치확인.png)
![트러플설치](/assets/트러플설치.png)
![트러플버전확인](/assets/트러플버전확인.png)

##### 트러플 설치 오류
- **checkPermissions** Missing write access to /usr/local/lib/node_modules
- **sudo** npm install -g truffle 명령으로 해결 [참고 블로그](https://blog.sonim1.com/125)
