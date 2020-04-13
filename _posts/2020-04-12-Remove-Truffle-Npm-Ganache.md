---

title : "Truffle, Npm, Nodejs, Ganache 삭제하기"
excerpt : "Rinkeby 배포 중 오류로 인해 모두 삭제 후 다시 시작하기"

categories:
  - Truffle
  - Blockchain
  - DApp
  - JavaScript

tags:
  - Truffle
  - Blockchain
  - DApp

---

<br/>

개발환경 클린하게 만들기
-------------------

<br/>

### Truffle 삭제하기

```
$ which truffle
$ rm -f 경로    //which truffle로 출력된 경로 사용
```

<br/>

* * *

<br/>

### Nodejs + npm 깔끔하게 삭제하기
[블로그](https://www.theteams.kr/teams/35/post/67342)에 자세히 나와있으니 명령어 한 번씩 싹 돌려주자
```
$ cd /usr/local/lib
$ sudo rm -rf node*
$ cd /usr/local/include
$ sudo rm -rf node*
```

<br/>

* * *

<br/>

### Ganache 삭제하기

응용프로그램에서 Ganache 삭제 처리

개발환경 갈아엎기 성공
