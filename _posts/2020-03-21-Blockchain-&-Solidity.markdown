---
layout: post
title:  "Blockchain & Solidity"
date:   2020-03-21 21:03:36 +0530
categories: Blockchain Solidity 
---

Smart Contract Dapp 에 들어가기 앞서 인프런에 있는 [블록체인과 솔리디티] 라는 강의를 수강했다.  
강의내용을 요약하자면 아래와 같다.

* 블록체인 : 암호화 (ex - 비트코인, 이더리움 )를 기반으로 가능한 많은 사람들이 접근 가능한 데이터 분산처리 기술  
* 이더이움 : 블록체인 플랫폼의 암호화폐
* 스마트 컨트랙트 : 블록체인상의 계약서
  + 참고로 이번 학기에 졸업프로젝트로 진행하는 Dapp의 정의는 블록체인을 기반으로 이더리움 플랫폼위에서 스마트 컨트랙트로 작동하는 탈중앙화( 분산 ) 어플리케이션 이다.    

* 솔리디티 : 이더리움 가상머신을 타겟으로, 스마트 컨트랙트를 만들기 위한 언어   
  + Remix : 웹 기반의 솔리디티 에디터  

* 가스 : 수수료 = 가스프라이스 * 가스 사용량   
  + 스마트 컨트랙트에서 기능을 사용시 마다 가스 사용
  

* * *

__라이브코딩으로 블록체인 투표앱 만들기__

Remix : <http://remix.ethereum.org/>

```solidity
pragma solidity 0.4.24;

contract vote{
    
}

```







[블록체인과 솔리디티]: https://www.inflearn.com/course/블록체인-blockchain/dashboard
