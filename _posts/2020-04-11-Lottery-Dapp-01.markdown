---
layout: post
title:  "[강의수강] 초보자를 위한 Lottery Dapp 개발 - 상"
excerpt: "인프런 강의 - Ethereum 실전! 초보자를 위한 Lottery Dapp 개발: Dapp 개발환경 세팅, Truffle Project 설정 및 구조 설명"
date:   2020-04-11 13:38:36 +0530
categories: Ethereum Dapp Truffle
---

<br/>

<h3>< Dapp 개발환경 세팅 ></h3>
기본적으로 필요한 것  
1. node.js -> 강의에서는 8버전 사용 ( 8버전 이상이면 상관 없음 ), 현재 내 컴퓨터의 버전은 12.16.1  
  <img src="/assets/imgs/Lottery&Dapp_01.png" width="65%" height="35%" >  
2. vscode ( Visual Studio Code )   
  [Visual Studio Code 홈페이지] 에서 설치
3. Truffle -> 강의에서는 5.0.2버전 사용, 현재 내 컴퓨터의 버전은 5.1.18  
  <img src="/assets/imgs/Lottery&Dapp_02.png" width="65%" height="35%" >  
4. ganache-cli  
  <img src="/assets/imgs/Lottery&Dapp_03.png" width="65%" height="35%" >  
  ganache-cli 명령어로 accounts 와 private keys 확인 가능  
  <img src="/assets/imgs/Lottery&Dapp_04.png" width="65%" height="35%" >  
5. vscode - solidity extension  
  vscode 에서 solidity 확장자 설치  
  <img src="/assets/imgs/Lottery&Dapp_05.png" width="65%" height="35%" >  
6. metamask  
  [metamask 홈페이지] 에서 설치

<h3>< Truffle Project 세팅 - Truffle Project 설정 및 구조 설명 ></h3>
  
  설치 했었던 Truffle, Solidity, Node 버전 확인  
  <img src="/assets/imgs/Lottery&Dapp_06.png" width="65%" height="35%" >  
  
  프로젝트 폴더 생성 lecture-tutorial 후 smartcontract를 사용할 폴더 lottery-smart-contract 생성  
  <img src="/assets/imgs/Lottery&Dapp_07.png" width="65%" height="35%" >  
  
  lottery-smart-contract 폴더로 이동하여 truffle init 명령어 실행 -> truffle 프로젝트의 기본세팅  
  <img src="/assets/imgs/Lottery&Dapp_08.png" width="65%" height="35%" >  
  
  새로생긴 폴더들 확인 ( ls-al 명령어 사용 )  
   <img src="/assets/imgs/Lottery&Dapp_09.png" width="65%" height="35%" >  
  
  

[Visual Studio Code 홈페이지]:   https://code.visualstudio.com
[metamask 홈페이지]:   https://metamask.io
