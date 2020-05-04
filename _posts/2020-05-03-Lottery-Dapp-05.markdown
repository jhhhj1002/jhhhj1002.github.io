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
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

import Web3 from 'web3';

class App extends Component {

  async componentDidMount() {
    await this.initWeb3();
  }

  initWeb3 = async () => {
    if (window.ethereum) {
      console.log('Recent mode')
      this.web3 = new Web3(window.ethereum);
      try {
          // Request account access if needed
          await window.ethereum.enable();
          // Acccounts now exposed
          // this.web3.eth.sendTransaction({/* ... */});
      } catch (error) {
          // User denied account access...
          console.log(`User denied account access error : ${error}`)
      }
    }
    // Legacy dapp browsers...
    else if (window.web3) {
      console.log('legacy mode')
      this.web3 = new Web3(Web3.currentProvider);
      // Acccounts always exposed
      // web3.eth.sendTransaction({/* ... */});
    }
    // Non-dapp browsers...
    else {
      console.log('Non-Ethereum browser detected. You should consider trying MetaMask!');
    }
  }

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo"/>
          <p>
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <a 
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer">
            Learn React
          </a>
        </header>
      </div>
    );
  }
}
// index address challenge answer pot status answerBlockNumber
export default App;
```

+ 이더리움 메타마스크 연결 참고 : https://medium.com/metamask/https-medium-com-metamask-breaking-change-injecting-web3-7722797916a8  
-> initWeb3 에 코드 사용  

lottery-react-web으로 이동하여 yarn start  
<img src="/assets/imgs/Lottery&Dapp_69.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_70.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_71.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_72.png" width="75%" height="45%" >  

크롬 검사창 console에서 'Recent mode' 출려 확인 가능  
<img src="/assets/imgs/Lottery&Dapp_73.png" width="75%" height="45%" >  

src 폴더 아래 App.js 파일 수정하여 연결된 계정 확인   
```
async componentDidMount() {
  await this.initWeb3();
  console.log(this.web3);
  let accounts = await this.web3.eth.getAccounts();
  console.log(accounts);
}
```

크롬 검사창 console에서 web3객체와 메타마스크 주소 확인가능   
<img src="/assets/imgs/Lottery&Dapp_74.png" width="75%" height="45%" >  


-> ganache-cli 와 함께 사용하고 있으므로 ganache-cli와 연결

메타마스크르 로컬호스트 8545와 연결 -> 로컬호스트 8545로 ganache-cli와 연결  
<img src="/assets/imgs/Lottery&Dapp_75.png" width="75%" height="45%" >  

콘솔에 접근하여 Accounts 확인  
<img src="/assets/imgs/Lottery&Dapp_76.png" width="75%" height="45%" >  


메타마스크 로컬호스트 8545 계정을 10이더 전송  
<img src="/assets/imgs/Lottery&Dapp_77.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_78.png" width="75%" height="45%" > 
-> status = true 로 트랜잭션 성공 확인 가능  

10이더 전송 확인  
<img src="/assets/imgs/Lottery&Dapp_79.png" width="75%" height="45%" >  

**ganache-cli의 private key 를 Metamask에 import하여 사용할 수도 있음**.  


src 폴더 아래 App.js 파일 수정 ( balance 가지고오는 코드추가 )  
```
async componentDidMount() {
  await this.initWeb3();
  console.log(this.web3);
  let accounts = await this.web3.eth.getAccounts();
  console.log(accounts);
  let balance = await this.web3.eth.getBalance(accounts[0]);
  console.log(balance);
}
```

새로고침하여 콘솔에서 balance 10이더 확인 가능  
<img src="/assets/imgs/Lottery&Dapp_80.png" width="75%" height="45%" >  






