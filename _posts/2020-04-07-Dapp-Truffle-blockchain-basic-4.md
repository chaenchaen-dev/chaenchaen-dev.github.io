---

title : "블록체인 Dapp 개발에 트러플 활용하기 4부 - Rinkeby에 배포하기"
excerpt : "인프런(inflearn.com)의 블록체인 'Dapp 개발에 트러플 활용하기_기본편' 수강하며 정리한 포스팅. 트러플 설치부터 스마트 컨트랙트, 로컬에 배포하기, Rinkeby에 배포하기, 단위테스트, 트러플 리액트 박스 열어보기, 리액트 애플리케이션과 결합하기를 포함한다."

categories:
  - Truffle
  - Blockchain
  - Rinkeby
  - DApp
  - JavaScript

tags:
  - Truffle
  - Blockchain
  - Rinkeby
  - DApp

---

<br/>

블록체인 Dapp 개발에 트러플 활용하기 4부
-------------------

<br/>

### Rinkeby에 배포하기

이더리움 네트워크에 접근할 수 있도록 도와주는 [Infura](https://infura.io/) 서비스를 통해서 배포를 할 수 있다.

- [Infura](https://infura.io/) 접속
- 회원가입 후 로그인
- create new project
- **api 키** 복사

![r2](/assets/pic/200407/r2.png)

3부의 **truffle-config.js 파일** 을 이어서 ropsten -> **rinkeby** 로 **수정** 하고, **api 키** 도 붙여넣기

![r3](/assets/pic/200407/r3.png){: width="70%"}

**HDWalletProvider** 주석 **제거**

![r4](/assets/pic/200407/r4.png){: width="70%"}

# Rinkeby 배포하려는 도중 대량의 오류 발생

![gyp1](/assets/pic/200407/gyp1.png)
```
$ sudo npm install node-gyp -g      //해결
```

나머지 오류 등등 
![err1](/assets/err1.png)
![err2](/assets/err2.png)




<br/>
<br/>
<br/>
<br/>
<br/>
