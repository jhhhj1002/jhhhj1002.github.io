---
layout: post
title:  "About Ethereum"
excerpt: "이더리움에 관한 기억하고 싶은 사소한 내용"
date:   2020-03-22 21:03:36 +0530
categories: Solidity Ethereum
---

<h3>< Development Comparision - Java Web vs Etherum Contract ></h3>

<img src="/assets/imgs/JavaVsEthereum.png" width="80%" height="65%" >

<br/>
<h3>< About Solidity ></h3>

> msg.sender : 메세지를 보낸 주소  
> msg.value : 메세지를 보낸 값

> storage : 전역변수  
> memory : 로컬변수

> external : 다른 컨트랙트나 트랜잭션에 의해서만 호출 가능 ex - f() x , this.f() O  
> public : getter 함수 자동 생성  
> internal : 내부적으로만 사용가능 , this 접근 불가  
> private : internal과 비슷하되 상속된 컨트랙트에서 접근 불가  
