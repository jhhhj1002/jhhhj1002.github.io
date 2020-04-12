---
layout: post
title:  "[강의수강] 초보자를 위한 Lottery Dapp 개발 - 중"
excerpt: "인프런 강의 - Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 : Dapp설계방법 및 Lottery 규칙, Lottery Domain 및 Queue 설계, Lottery Bet 함수 구현, Lottery Bet 함수 테스트, 이더리움 GAS 계산"
date:   2020-04-12 13:38:36 +0530
categories: Ethereum Dapp Truffle
---

<br/>

<h3>< Dapp설계방법 및 Lottery 규칙 ></h3>  

Dapp 서비스 설계
1. 지갑 관리 : 돈 관리  
2. 아키텍쳐  
  a. Smart Contract - front  
  b. Smart Contract - Server - front  
3. Code
  a. 코드를 실행하는데 돈이든다. 
  b. 권한 관리  
  c. 비지니스 로직 업데이트  
  d. 데이터 마이그레이션     
4. 운영  
  a. Public  
  b. Private    
  

Lottery 규칙  
1. +3 번째 블록해쉬의 첫 두글자 맞추기 '0xab...'
  a. 유저가 던진 트랜잭션이 들어가는 블로 +3의 블록해쉬와 값을 비교  
2. 팟머니  
  a. 결과가 나왔을 때만 유저가 보내 돈을 팟머니에 쌓기 
  b. 여러명이 맞추었을 때는 가자 먼저 맞춘 사람이 팟머니를 가져간다.  
  c. 두 글자 중 하나만 맞추었을 때는 보내 돈을 돌려준다.  
  d. 결과값을 검증 할 수 없을 때에는 보내 돈을 돌려준다.       











