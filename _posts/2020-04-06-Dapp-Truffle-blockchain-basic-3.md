---

title : "블록체인 Dapp 개발에 트러플 활용하기 3부 - Ganache에 배포하기"
excerpt : "인프런(inflearn.com)의 블록체인 'Dapp 개발에 트러플 활용하기_기본편' 수강하며 정리한 포스팅. 트러플 설치부터 스마트 컨트랙트, 로컬에 배포하기, Rinkeby에 배포하기, 단위테스트, 트러플 리액트 박스 열어보기, 리액트 애플리케이션과 결합하기를 포함한다."

categories:
  - Truffle
  - Blockchain
  - Ganache
  - DApp
  - JavaScript

tags:
  - Truffle
  - Blockchain
  - Ganache
  - DApp

---

<br/>

블록체인 Dapp 개발에 트러플 활용하기 3부
-------------------

<br/>

### Ganache 설치 (Mac OS)

로컬 이더리움 [Ganache](https://github.com/trufflesuite/ganache/releases)를 본인의 OS에 맞게 **다운** 받는다.

![ganache_download](/assets/pic/0406/ganache_download.png){: width="90%"}

**Ganache** 실행

![ganache1](/assets/pic/0406/ganache1.png){: width="80%"}

[ **QUICKSTART** ] 클릭

![ganache2](/assets/pic/0406/ganache2.png){: width="80%"}

[server] 에 **automine** 이 활성화 되어 있을 경우에 **자동** 으로 **블록체인이 생성** 된다.

<br/>

* * *

<br/>


### JavaScript 배포 스크립트 작성

배포할 스크립트 **2_deploy_hello.js 파일** 을 **migrations 폴더** 에 생성한다.
이때, .js 파일의 이름은 **순서** 를 따라서 만들어야 한다.

![script1](/assets/pic/0406/script1.png)

**2_deploy_hello.js** 작성

```
const helloWorld = artifacts.require("HelloWorld");   // HelloWorld 컨트랙트 이름 (배포되어야 할 컨트랙트의 이름을 써준다.)

module.exports = function(deployer) {
    deployer.deploy(helloWorld, "Hello, World!");   //생성자에 파라미터 추가
}
```
![script6](/assets/pic/0406/script6.png){: width="90%"}

- 배포
  - 개발서버에 해당 **테스트넷**(Testnet), **메인넷**(Mainnet)
  - 운영서버
  - 로컬 **가나슈**(Ganache)

배포를 어디에 해줄 것인지 **target** 은 **truffle_config.js** 에서 **networks:** 에서 **설정** 해준다.

networks에서 **development 주석 해제** 하고 가나슈 정보와 **일치** 하게 수정한다.

![script4](/assets/pic/0406/script4.png)
![script5](/assets/pic/0406/script5_v76ndgs8s.png){: width="40%"}
> contract address 즉, 스마트 컨트랙트 주소가 부여

<br/>

* * *

<br/>

### 스마트 컨트랙트 배포하기

터미널에서 **배포 명령어** 입력
```
> truffle migrate --network development   //development 자리에 배포하려는 타겟의 이름 작성
> truffle migrate --reset     //처음부터 새로 배포하는 명령어
```

![result2](/assets/result2.png){: width="50%"} ![result1](/assets/result1.png){: width="50%"} 

<br/>

로컬 **가나슈**(Ganache)에 **배포 성공**

<br/>
<br/>
<br/>
<br/>
<br/>
