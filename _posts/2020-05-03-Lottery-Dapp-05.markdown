---
layout: post
title:  "[강의수강] 초보자를 위한 Lottery Dapp 개발 - 4"
excerpt: "인프런 강의 - Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 : React js 환경설정, web3js - send&call, 
web3js - filter, Dapp 데이터 관리"
date:   2020-05-03 14:50:36 +0530
categories: Ethereum Dapp Truffle
---


<br/>

<h3>< React js 환경설정 ></h3>  

이번 실습에서 할 일 : 지금까지 만든 Lottery Smart Contract 를 React 에 연결하여 Front에 붙임  
**메타마스크 설치 필수**

-------------------------------------------------------------------------------------------//npm  
Create React app 설치 ( npm -g install create-react-app 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_54.png" width="75%" height="45%" >  

lottery-react-web 이라는 이름의 react app 생성 ( create-react-app lottery-react-web 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_55.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_56.png" width="75%" height="45%" >  

lottery-react-web 이 생성된것 확인  
<img src="/assets/imgs/Lottery&Dapp_57.png" width="75%" height="45%" >  

lottery-react-web으로 이동하여 yarn start  
<img src="/assets/imgs/Lottery&Dapp_58.png" width="75%" height="45%" >  

리액트 기본페이지 출력됨  
<img src="/assets/imgs/Lottery&Dapp_59.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_60.png" width="75%" height="45%" >  
-------------------------------------------------------------------------------------------//npm  

-------------------------------------------------------------------------------------------//yarn  
Create React app 설치 ( yarn global add create-react-app 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_61.png" width="75%" height="45%" > 

lottery-react-web 이라는 이름의 react app 생성 ( create-react-app lottery-react-web 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_62.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_63.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_64.png" width="75%" height="45%" >  

lottery-react-web 이 생성된것 확인  
<img src="/assets/imgs/Lottery&Dapp_57.png" width="75%" height="45%" >  

lottery-react-web으로 이동하여 yarn start  
<img src="/assets/imgs/Lottery&Dapp_65.png" width="75%" height="45%" >  

리액트 기본페이지 출력됨  
<img src="/assets/imgs/Lottery&Dapp_66.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_60.png" width="75%" height="45%" >  

web3 설치 ( yarn add web3 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_67.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_68.png" width="75%" height="45%" >  

**본 강의에서는 클래스형 컴포넌트 방식사용, 본인은 함수형 컴포넌트 방식 사용**

src 폴더 아래 App.js 파일 수정  
```
import Web3 from 'web3';
```









