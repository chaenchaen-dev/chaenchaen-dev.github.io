---

title : "블록체인 Dapp 개발에 트러플 활용하기 2부 - 스마트 컨트랙트(Smart Contract) 작성"
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

블록체인 Dapp 개발에 트러플 활용하기 2부
-------------------

<br/>

### 스마트 컨트랙트(Smart Contract) 작성하기

강의에서는 스마트 컨트랙트를 작성할 프로그램으로 WebStorm을 사용했으나, 이 포스팅에서는 **IntelliJ CE** 버전으로 진행할 예정이다.

 **IntelliJ** 에서 솔리디티 언어를 작성하려면 **플러그인 다운로드 필수**
![sol_plugin](/assets/pic/0406/sol_plugin.png){: width="80%"}

[File] - [Open] - **dapp-example** 폴더 선택

![intelliJ_file_open](/assets/pic/0406/intelliJ_file_open.png){: width="90%"}

[contracts]에 HelloWorld 솔리디티 파일 만들기

![intelliJ_file_open](/assets/pic/0406/new_solidity_file.png){: width="90%"}

HelloWorld.sol **솔리디티 컴파일러 버전** 을 **0.5.8** 로 변경

![sol_version_check](/assets/pic/0406/sol_version_check.png){: width="90%"}
![sol_v](/assets/pic/0406/sol_v.png){: width="50%"}

**HelloWorld.sol** 작성
```
pragma solidity ^0.5.1;

contract HelloWorld {
    string public greeting;

    constructor(string memory _greeting) public {
        greeting = _greeting;
    }
    function setGreeting(string memory _greeting) public{
        greeting = _greeting;
    }
    function say() public view returns(string memory){    //출력
        return greeting;
    }
}

//terminal
$ truffle compile     //contracts에 있는 파일 모두 컴파일
```

<br/>

* * *

<br/>

### HelloWorld.sol 컴파일

**compile** -> **.json 파일 자동 생성**

![helloworld1](/assets/pic/0406/helloworld1.png){: width="90%"}
![helloworld2](/assets/pic/0406/helloworld2.png){: width="50%"}

**json** 파일에서 **중요** 부분은 **abi** , **bytecode** , **deployedBytecode**

![helloworld3](/assets/pic/0406/helloworld3.png){: width="90%"}

<br/>

***

<br/>
### **truffle_config.js** 에 **path 지정**

```
const path = require("path");
contracts_bulid_directory: path.join(__dirname, "client/src/contracts"),
```
![add_path](/assets/pic/0406/add_path.png){: width="90%"}
![helloworld4](/assets/pic/0406/helloworld4.png){: width="90%"}

다시 터미널에서 **compile** 하면 **지정한 위치** 에 컴파일 결과가 **저장** 되는 것을 확인할 수 있다.

```
$ truffle compile --all
```
![re_compile](/assets/pic/0406/re_compile.png){: width="50%"}
> path를 따로 지정하지 않고 default값인 경우에 저장 경로

![helloworld5](/assets/pic/0406/helloworld5.png){: width="50%"}
> path를 지정해줬을 경우의 파일 저장 경로

















<br/>
<br/>
<br/>
<br/>
<br/>
