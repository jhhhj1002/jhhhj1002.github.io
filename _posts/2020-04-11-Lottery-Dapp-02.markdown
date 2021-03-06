---
layout: post
title:  "[강의수강] 초보자를 위한 Lottery Dapp 개발 - 1"
excerpt: "인프런 강의 - Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 : Truffle Project 설정 및 구조 설명, Truffle을 활용한 테스트"
date:   2020-04-11 13:38:36 +0530
categories: Ethereum Dapp Truffle
---

<br/>

<h3>< Truffle Project 세팅 - Truffle Project 설정 및 구조 설명 ></h3>  
  
  설치 했었던 Truffle, Solidity, Node 버전 확인  
  <img src="/assets/imgs/Lottery&Dapp_06.png" width="45%" height="25%" >  
  
  <br/>
  프로젝트 폴더 생성 lecture-tutorial 후 smartcontract를 사용할 폴더 lottery-smart-contract 생성  
  <img src="/assets/imgs/Lottery&Dapp_07.png" width="65%" height="35%" >  
  
  <br/>
  lottery-smart-contract 폴더로 이동하여 truffle init 명령어 실행 -> truffle 프로젝트의 기본세팅  
  <img src="/assets/imgs/Lottery&Dapp_08.png" width="65%" height="35%" >  
  
  <br/>
  새로생긴 폴더들 확인 ( ls-al 명령어 사용 )  
  <img src="/assets/imgs/Lottery&Dapp_09.png" width="65%" height="35%" >  
  
  <br/>
  Visual Studio Code 로 lottery-smart-contract 프로젝트 오픈  
      -> Migrations.sol, 1_initial_migration.js 파일 생성 확인  
  <img src="/assets/imgs/Lottery&Dapp_10.png" width="65%" height="35%" >  
  
  + contracts 폴더 : 스마트 컨트랙트 관련 폴더  
  + migrations 폴더 : 배포 관련 스크립트 폴더  
  + test 폴더 : 단위테스트 관련 폴더  
  
  <br/>
  contracts 폴더 안에 Lottery.sol 파일 생성 후 컨트랙트 기본구조 작성  
  ```
   pragma solidity >=0.4.21 <0.7.0;
   
   contract Lottery {

   }
  ```
  
  <br/>
  컴파일  
  <img src="/assets/imgs/Lottery&Dapp_11.png" width="70%" height="40%" >  
  
  <br/>
  컴파일시 build 폴더 생성 -> 컴파일 결과 json 파일로 생성    
  <img src="/assets/imgs/Lottery&Dapp_12.png" width="30%" height="15%" >  
  + json 파일의 bytecode : 실제 blockchain 에 배포될 때 사용되는 bytecode
  
  <br/>
  migrations 폴더에 2_deploy_smart_contract.js 파일 생성 후 배포 코드 작성  
  ```
   const Lottery = artifacts.require("Lottery"); // artifacts.require 가 build 폴더 안에있는 Lottery 데이터 가지고옴

   module.exports = function(deployer) {
     deployer.deploy(Lottery); // Lottery 의 bytecode를 가지고 와 deployer 배포 (deploy) 해줌
   };
  ```
  
  <br/>
  배포하기 위해 사용할 blockchain 네트워크 ( ganache-cli -d -m tutorial 명령어 사용 - 단어는 tutorial 로 설정 )  
  <img src="/assets/imgs/Lottery&Dapp_13.png" width="65%" height="35%" >  
  
  <br/>
  truffle-config.js 파일 에서 배포할 networks 설정 ( networks의 development 부분 수정 )  
  
  ```  
   development: {
     host: "127.0.0.1",     // Localhost (default: none)
     port: 8545,            // Standard Ethereum port (default: none)
     network_id: "*",       // Any network (default: none)
    },   
  ```
   + 로컬에서 테스트 예정 이라 주석만 제거  
   
  <br/>
  스마트 컨트랙트 배포 ( truffle migrate 명령어 사용 )  
  <img src="/assets/imgs/Lottery&Dapp_14.png" width="65%" height="35%" >  
  
  + 스마트 컨트랙트 재배포 시에는 truffle migrate --reset 명령어 사용  
  
  <br/>
  * * *
  <br/>
  <h3>< Truffle Project 세팅 - Truffle을 활용한 스마트 컨트랙트 상호작용 ></h3>  
  
  <br/>
  Lottey.sol 파일 작성  
   ``` 
   pragma solidity >=0.4.21 <0.7.0;

   contract Lottery {

      address public owner; // public 으로 선언하여 자동 getter 생성

       constructor() public {
          owner = msg.sender;
      }

      function getSomeValue() public pure returns (uint256 value){
          return 5;
      }

  }
  ```
  
  <br/>
   스마트 컨트랙트 재배포  
   <img src="/assets/imgs/Lottery&Dapp_15.png" width="65%" height="35%" >  
       => 처음 배포시 Lottery의 gas used는 66838 이었는데 재배포시 gas used는 130157 로 증가  
          코드를 작성한 만큼 블록체인에 저장해야 하기 때문에 많은 수수료가 사용됨  
  
  <br/>
  이전에 켜둔 ganache-cli 에서 배포 트랜잭션들 기록 -> 트랜잭션들 확인 가능  
  <img src="/assets/imgs/Lottery&Dapp_16.png" width="65%" height="35%" >  
  <img src="/assets/imgs/Lottery&Dapp_17.png" width="65%" height="35%" >  
  
  <br/>
  블록체인에 접근 ( truffle console 명령어 사용 )  
  <img src="/assets/imgs/Lottery&Dapp_18.png" width="65%" height="35%" >  
  
  <br/>
  web3 에 접근 해서 사용 가능  
  <img src="/assets/imgs/Lottery&Dapp_19.png" width="35%" height="25%" >  
  
  <br/>
  받은 주소 10개 ( ganache-cli 의 Available Accounts ) 확인 가능 ( getAccounts( ) 명령어 사용 )  
  <img src="/assets/imgs/Lottery&Dapp_20.png" width="40%" height="20%" >  

  <br/>
  계정의 이더 확인 가능 ( getBalance('Account Address') 명령어 사용 )  
  <img src="/assets/imgs/Lottery&Dapp_21.png" width="65%" height="35%" >  
  + 원래 100 이더였는데 스마트 컨트랙트 배포하는데 이더 사용  

  <br/>
  Lottery address 확인 가능 ( Lottery.json 파일의 networks의 address 와 동일 한 주소 확인 가능 )  
  <img src="/assets/imgs/Lottery&Dapp_22.png" width="45%" height="25%" >  

  <br/>
  lt 변수안에 배포된 Lottery의 instance 넣음  
  <img src="/assets/imgs/Lottery&Dapp_23.png" width="65%" height="35%" >  

  <br/>
  lt 에서 사용할 수 있는 여러가지 함수들을 abi 형태로 확인 가능  
  <img src="/assets/imgs/Lottery&Dapp_24.png" width="30%" height="15%" >  

  <br/>
  owner의 getter 함수 사용  
  <img src="/assets/imgs/Lottery&Dapp_25.png" width="45%" height="20%" >  
      -> eth.getAccounts()시 0번째 주소와 동일 ( truffle은 자신이 가진 주소중 0번째 주소를 deployer로 사용 )  
  
  <br/>
  Lottery.sol 파일 에 만든 getSomeValue() 함수 확인 -> 리턴 5확인 가능  
  <img src="/assets/imgs/Lottery&Dapp_26.png" width="65%" height="35%" >  
  
  <br/>
  * * *
  <br/>
  <h3>< Truffle Project 세팅 - Truffle을 활용한 테스트 ></h3>  
  
  <br/>
  test 폴더 안에 lottery.test.js 파일 생성 후 코드작성  
  ```   
   const Lottery = artifacts.require("Lottery");

   contract('Lottery', function([deployer, user1, user2]){ // 각각의 파라미터에는 10개의 주소중 순서대로 들어감

        beforeEach(async () => {
            console.log('Before each')      
        })

        it('Basic test', async () => {
            console.log('Basic test')
        })

    }); 
  ```
  
  + 테스트 하는 방법 1 : truffle test 명령어 이용 - test 폴더 안의 모드 파일 실행
  + 테스트 하는 방법 2 : truffle test test/lottery.test.js - 테스트 할 파일 직접 지정 ( 단일 테스트 )  
  
  
  <br/>
  테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )  
  <img src="/assets/imgs/Lottery&Dapp_27.png" width="65%" height="35%" >  
      -> lottery.test.js 파일의 console 로 찍은 부부 확인 가능  
    
  <br/>
  테스트 파일 수정 후 테스트 파일 재실행( truffle test test/lottery.test.js 명령어 사용 )  
  ```
   const Lottery = artifacts.require("Lottery");

    contract('Lottery', function([deployer, user1, user2]){ // 각각의 파라미터에는 10개의 주소중 순서대로 들어감
        let lottery;
        beforeEach(async () => {
            console.log('Before each')    
            lottery = await Lottery.new();  // 컨트랙트 배포 
        })

        it('Basic test', async () => {
            console.log('Basic test')
            let owner = await lottery.owner();
            let value = await lottery.getSomeValue();

            console.log('owner : ' + owner);
            console.log('value : ' + value);
            assert.equal(value, 5) // value 값 5와 같은지 확인
        })

    });
  ```
  <img src="/assets/imgs/Lottery&Dapp_28.png" width="70%" height="35%" >  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
 
