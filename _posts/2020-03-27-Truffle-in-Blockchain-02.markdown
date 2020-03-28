---
layout: post
title:  "[강의수강] 블록체인 Dapp 개발에 트러플 활용하기_기본편 - 중"
date:   2020-03-27 18:38:36 +0530
categories: Blockchain Dapp Truffle
---

< 로컬 이더리움( Ganache )에 배포하기 >

Ganache 설치

<img src="/assets/imgs/Blockchain&Truffle_11.png" width="80%" height="45%" >

Quick start 선택  
  RPC Server : HTTP://127.0.0.1:7545  
  NETWORK ID : 5777  

<img src="/assets/imgs/Blockchain&Truffle_12.png" width="80%" height="45%" >

+ 테스트넷(Testnet), 매인넷(Mainnet), 로컬 에 컴파일된 SmartContract 배포 가능

이번 실습에서는 로컬 (Ganache)에 배포 예정
배포를 위해서는 migrations 폴더 아래에 JavaScript 코드 작성 필요

배포 Script 작성 ( ex - 파일명 : 2_deploy_hello.js )

```javascript

const helloworld = artifacts.require("HelloWorld");

module.exports = function(deployer){
    deployer.deploy(helloworld,"Hello,World!");
};

```
