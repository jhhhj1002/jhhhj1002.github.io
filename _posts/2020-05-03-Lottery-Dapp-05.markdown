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

트러플 콘솔에 접근하여 Accounts 확인  
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

새로고침하여 Chrome 콘솔에서 balance 10이더 확인 가능  
<img src="/assets/imgs/Lottery&Dapp_80.png" width="75%" height="45%" >  


<br/>
* * *
<br/>
<h3>< web3js - send&call ></h3> 
  
<br/>
스마트 컨트랙트 재배포 ( truffle migrate --reset 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_81.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_82.png" width="75%" height="45%" >  

src 폴더 아래 App.js 파일 수정  
```
let lotteryAddress = '0x9Cf1A3C561D45b3f689cC2e109cc21bb04644948'; // 배포된 deploy_smart_contract.js의 contract address 사용 
let lotteryABI = [ { "inputs": [], "payable": false, "stateMutability": "nonpayable", "type": "constructor" }, { "anonymous": false, "inputs": [ { "indexed": false, "internalType": "uint256", "name": "index", "type": "uint256" }, { "indexed": true, "internalType": "address", "name": "bettor", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "amount", "type": "uint256" }, { "indexed": false, "internalType": "bytes1", "name": "challenges", "type": "bytes1" }, { "indexed": false, "internalType": "uint256", "name": "answerBlockNumber", "type": "uint256" } ], "name": "BET", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": false, "internalType": "uint256", "name": "index", "type": "uint256" }, { "indexed": false, "internalType": "address", "name": "bettor", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "amount", "type": "uint256" }, { "indexed": false, "internalType": "bytes1", "name": "challenges", "type": "bytes1" }, { "indexed": false, "internalType": "bytes1", "name": "answer", "type": "bytes1" }, { "indexed": false, "internalType": "uint256", "name": "answerBlockNumber", "type": "uint256" } ], "name": "DRAW", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": false, "internalType": "uint256", "name": "index", "type": "uint256" }, { "indexed": false, "internalType": "address", "name": "bettor", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "amount", "type": "uint256" }, { "indexed": false, "internalType": "bytes1", "name": "challenges", "type": "bytes1" }, { "indexed": false, "internalType": "bytes1", "name": "answer", "type": "bytes1" }, { "indexed": false, "internalType": "uint256", "name": "answerBlockNumber", "type": "uint256" } ], "name": "FAIL", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": false, "internalType": "uint256", "name": "index", "type": "uint256" }, { "indexed": false, "internalType": "address", "name": "bettor", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "amount", "type": "uint256" }, { "indexed": false, "internalType": "bytes1", "name": "challenges", "type": "bytes1" }, { "indexed": false, "internalType": "uint256", "name": "answerBlockNumber", "type": "uint256" } ], "name": "REFUND", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": false, "internalType": "uint256", "name": "index", "type": "uint256" }, { "indexed": false, "internalType": "address", "name": "bettor", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "amount", "type": "uint256" }, { "indexed": false, "internalType": "bytes1", "name": "challenges", "type": "bytes1" }, { "indexed": false, "internalType": "bytes1", "name": "answer", "type": "bytes1" }, { "indexed": false, "internalType": "uint256", "name": "answerBlockNumber", "type": "uint256" } ], "name": "WIN", "type": "event" }, { "constant": true, "inputs": [], "name": "answerForTest", "outputs": [ { "internalType": "bytes32", "name": "", "type": "bytes32" } ], "payable": false, "stateMutability": "view", "type": "function" }, { "constant": true, "inputs": [], "name": "owner", "outputs": [ { "internalType": "address payable", "name": "", "type": "address" } ], "payable": false, "stateMutability": "view", "type": "function" }, { "constant": true, "inputs": [], "name": "getPot", "outputs": [ { "internalType": "uint256", "name": "pot", "type": "uint256" } ], "payable": false, "stateMutability": "view", "type": "function" }, { "constant": false, "inputs": [ { "internalType": "bytes1", "name": "challenges", "type": "bytes1" } ], "name": "betAndDistribute", "outputs": [ { "internalType": "bool", "name": "result", "type": "bool" } ], "payable": true, "stateMutability": "payable", "type": "function" }, { "constant": false, "inputs": [ { "internalType": "bytes1", "name": "challenges", "type": "bytes1" } ], "name": "bet", "outputs": [ { "internalType": "bool", "name": "result", "type": "bool" } ], "payable": true, "stateMutability": "payable", "type": "function" }, { "constant": false, "inputs": [ { "internalType": "bytes32", "name": "answer", "type": "bytes32" } ], "name": "setAnswerForTest", "outputs": [ { "internalType": "bool", "name": "result", "type": "bool" } ], "payable": false, "stateMutability": "nonpayable", "type": "function" }, { "constant": false, "inputs": [], "name": "distribute", "outputs": [], "payable": false, "stateMutability": "nonpayable", "type": "function" }, { "constant": true, "inputs": [ { "internalType": "bytes1", "name": "challenges", "type": "bytes1" }, { "internalType": "bytes32", "name": "answer", "type": "bytes32" } ], "name": "isMatch", "outputs": [ { "internalType": "enum Lottery.BettingResult", "name": "", "type": "uint8" } ], "payable": false, "stateMutability": "pure", "type": "function" }, { "constant": true, "inputs": [ { "internalType": "uint256", "name": "index", "type": "uint256" } ], "name": "getBetInfo", "outputs": [ { "internalType": "uint256", "name": "answerBlockNumber", "type": "uint256" }, { "internalType": "address", "name": "bettor", "type": "address" }, { "internalType": "bytes1", "name": "challenges", "type": "bytes1" } ], "payable": false, "stateMutability": "view", "type": "function" }];
// abi필요 -> build 폴더 아래 Lottery.json 의 abi 배열 linebrak하여 복붙

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

    let accounts = await this.web3.eth.getAccounts();
    this.account = accounts[0];

    this.lotteryContract = new this.web3.eth.Contract(lotteryABI, lotteryAddress); // C 대문자 주의 
    
    let pot = await this.lotteryContract.methods.getPot().call(); // 팟머니 불러옴 - 컨트랙스에 있는 method 중에 getPot을 호출 
    console.log(pot);
    
    let owner = await this.lotteryContract.methods.owner().call(); // owner 불러옴 - 컨트랙스에 있는 method 중에 owner를 호출 
    console.log(owner);
  }
```

+ ABI line brak 사이트 : https://www.textfixer.com/tools/remove-line-breaks.php

+ call : 스마트 컨트랙트의 상태를 변화시키지 않는, 블럭체인의 실제 트랜잭션으로 만들어 지지 않는, 값만 읽어오는 것들  
+ send, invoke : 스마트 컨트랙트에 있느 값을 변화시키거나, 블럭체인의 새로운 트랜잭션을 통해 상호작용을 하는 것들 


새로고침하여 Chrome 콘솔에서 확인  
<img src="/assets/imgs/Lottery&Dapp_83.png" width="75%" height="45%" >  
-> 아직 배팅한 사람이 없으니 pot은 0, ganache-cli의 0번째 account 확인 가능  

트러플 콘솔에 들어가서 콘솔에서도 상호작용 할수있게 준비  
<img src="/assets/imgs/Lottery&Dapp_84.png" width="75%" height="45%" >  
lt 입력시 출력되는 뒷부분 너무 길어서 캡쳐 생략  
-> 변수 lt를 사용하여 상호작용 가능  


Accounts 가지고 와서, 첫번째 계정에 대하여 betAndDistribute 함수 사용  
<img src="/assets/imgs/Lottery&Dapp_85.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_86.png" width="75%" height="45%" >  


betAndDistribute 함수 여러번 호출 -> event : 'FAIL' 확인 ( FAIL이 되어야 팟머니에 들어감 )  
<img src="/assets/imgs/Lottery&Dapp_87.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_88.png" width="75%" height="45%" >  

새로고침하여 Chrome 콘솔에서 팟머니 증가 확인  
<img src="/assets/imgs/Lottery&Dapp_89.png" width="75%" height="45%" >  

src 폴더 아래 App.js 파일 수정 ( 배팅 트랜잭션 추가 )  
```
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

    let accounts = await this.web3.eth.getAccounts();
    this.account = accounts[0];

    this.lotteryContract = new this.web3.eth.Contract(lotteryABI, lotteryAddress) // C 대문자 주의 

    let pot = await this.lotteryContract.methods.getPot().call() // 팟머니 불러옴 - 컨트랙스에 있는 method 중에 getPot을 호출 
    console.log(pot)

    let owner = await this.lotteryContract.methods.owner().call() // owner 불러옴 - 컨트랙스에 있는 method 중에 owner를 호출 
    console.log(owner)

    this.lotteryContract.methods.betAndDistribute('0xcd').send({from:this.account, value:5000000000000000,gas:300000}) // send 사용
}
```

Chrome 새로고침 후, 계약 승인 선택시 트랜잭션 발생  
<img src="/assets/imgs/Lottery&Dapp_90.png" width="75%" height="45%" >  
<img src="/assets/imgs/Lottery&Dapp_91.png" width="75%" height="45%" >  

트러플 콘솔창에서 인덱스 확인 ( getBetInfo(index) 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_92.png" width="75%" height="45%" >   
-> 5번에 '0xcd' 들어있음 확인  

더 진행되지 않은 값들은 '0x00'으로 초기화 되어있음  
<img src="/assets/imgs/Lottery&Dapp_93.png" width="75%" height="45%" >   


src 폴더 아래 App.js 파일 수정 ( bet 함수 추가 )  
```
bet = async () => {
   // nonce - 특정 address 가 몇개의 트랜잭션들을 만들었는지 알수 있음 -> 트랜잭션 replay방지, 외부의 유저가 마음대로 사용하지 못하게 하는 기능o

   let nonce = await this.web3.eth.getTransactionCount(this.account);
   this.lotteryContract.methods.betAndDistribute(challenges).send({from:this.account, value:5000000000000000, gas:300000, nonce:nonce})
}
```









