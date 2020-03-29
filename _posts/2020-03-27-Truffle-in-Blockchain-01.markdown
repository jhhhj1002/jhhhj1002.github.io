---
layout: post
title:  "[강의수강] 블록체인 Dapp 개발에 트러플 활용하기_기본편 - 상"
excerpt: "인프런 강의 - 블록체인 Dapp 개발에 트러플 활용하기 : 트러플 설치하기, 스마트 컨트랙트 Hello,World!"
date:   2020-03-27 17:21:36 +0530
categories: Blockchain Dapp Truffle
---

<br/> 
<h3>< 트러플 설치하기 ></h3> 

Node 버전 확인  
<img src="/assets/imgs/Blockchain&Truffle_01.png" width="50%" height="35%" >

<br/>
Truffle 설치  
<img src="/assets/imgs/Blockchain&Truffle_02.png" width="100%" height="60%" >

<br/>
Truffle, Solodity, Node, Web3.js 버전 확인 => Truffle 버전 : 5.1.18 , Solidity 버전 : 0.5.16  
<img src="/assets/imgs/Blockchain&Truffle_03.png" width="55%" height="35%" >

<br/>
dapp-example 폴더 생성 후 이동  
<img src="/assets/imgs/Blockchain&Truffle_04.png" width="60%" height="40%" >

<br/>
소스폴더 생성 : truffle init ( npm init 과 같은 역할 )  
<img src="/assets/imgs/Blockchain&Truffle_05.png" width="60%" height="40%" >

<br/>
4가지 폴더가 생성됨  
<img src="/assets/imgs/Blockchain&Truffle_06.png" width="85%" height="40%" >

<br/>
contracts 폴더 아래에 스마트 컨트랙트 작성  
<img src="/assets/imgs/Blockchain&Truffle_07.png" width="60%" height="40%" >


<br/>
* * *
<br/>
<h3>< 스마트 컨트랙트 Hello,World! ></h3>

인텔리제이로 이전에 만든 dapp-example 폴더 open 하여 코드 작업 ( Solidity Plugin 설치 필수 )  

<br/>
contracts 폴더 아래에 solidity 컨트랙트 작성

<br/> 
자신의 solidity 버전에 따라서 solodity가 컴파일됨 ( truffle version시 나오는 solidity 버전 참고 )  
만약, 자신의 버전보다 아래버전의 solidity 사용시 truffle-config.js 파일의 compilers:{ solc:{ 의 version 수정 후 주석 삭제 하여 사용   
<img src="/assets/imgs/Blockchain&Truffle_08.png" width="80%" height="45%" >

<br/> 
현재 solidity 버전 : 0.5.16 -> truffle-config.js 파일 수정 후 solidity 버전 : 0.5.8
<img src="/assets/imgs/Blockchain&Truffle_09.png" width="60%" height="40%" >
      
<br/>
HelloWorld.sol 스마트 컨트랙트 작성 ( solidity 버전 : 0.5.8 )
```
pragma solidity ^0.5.8;  

contract HelloWorld { //컨트랙트 이름 : HelloWorld

    string public greeting; //상태변수

    constructor(string memory _greeting) public { //생성자
        greeting = _greeting;
    }

    function setGreeting(string memory _greeting) public { //메소드 - set
        greeting = _greeting;
    }

    function say() public view returns (string memory){ //메소드 - 출력
        return greeting;
    }

}

```

<br/>
컴파일 ( truffle compile / compile --all )  
 <img src="/assets/imgs/Blockchain&Truffle_10.png" width="65%" height="45%" >   

<br/>
-> 컴파일 결과 build 아래 contracts 아래 컴파일된 결과 json파일로 생성   
+ json 파일의 abi, deployedBytecode, bytecode 부분이 중요한 부분
 

