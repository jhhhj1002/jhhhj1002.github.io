---
layout: post
title:  "[강의수강] 초보자를 위한 Lottery Dapp 개발 - 2"
excerpt: "인프런 강의 - Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 : Dapp설계방법 및 Lottery 규칙, Lottery Domain 및 Queue 설계, Lottery Bet 함수 구현, Lottery Bet 함수 테스트, 이더리움 GAS 계산"
date:   2020-04-12 13:38:36 +0530
categories: Ethereum Dapp Truffle
---

<br/>

<h3>< Dapp설계방법 및 Lottery 규칙 ></h3>  

<br/>
+ <h4>Dapp 서비스 설계</h4>
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
  
<br/>
+ <h4>Lottery 규칙</h4> 
1. +3 번째 블록해쉬의 첫 두글자 맞추기 '0xab...'  
  a. 유저가 던진 트랜잭션이 들어가는 블로 +3의 블록해쉬와 값을 비교  
2. 팟머니  
  a. 결과가 나왔을 때만 유저가 보내 돈을 팟머니에 쌓기 
  b. 여러명이 맞추었을 때는 가자 먼저 맞춘 사람이 팟머니를 가져간다.  
  c. 두 글자 중 하나만 맞추었을 때는 보내 돈을 돌려준다. 0.005ETH : 10 ** 15wei    
  d. 결과값을 검증 할 수 없을 때에는 보내 돈을 돌려준다.       


<br/>
* * *
<br/>
<h3>< Lottery Domain 및 Queue 설계 ></h3> 

<br/>
contracts 폴더의 Lottery.sol 파일에 코드 추가 ( 팟머니에 대한 코드 추가 )    
  ```  
   pragma solidity >=0.4.21 <0.7.0;

   contract Lottery {

     struct BetInfo {
        uint256 answerBlockNumber; // 맞추려는 정답 블록 
        address payable bettor; // 맞추었을때 돈을 보내주어야 할 better
        byte challenges; // 0xab 와 같은 1바이트 글자
    }

     uint256 private _pot; // 팟머니 저장해야 할 곳

     address public owner; // public 으로 선언하여 자동 getter 생성

     constructor() public {
        owner = msg.sender;
    }

    function getSomeValue() public pure returns (uint256 value){
        return 5;
    }

    function getPot() public view returns (uint256 pot) { // 팟머니 get 하는 함수 
        return _pot;
    }

  }
  ```
  
<br/>
Lottey.sol 을 테스트 하기위해 lottery.test.js 파일에 코드 추가  
  ```  
  it.only('getPot should return current pot', async () => { // 특정 케이스만 테스트 하기 위해 only 추가
        let pot = await lottery.getPot();
        assert.equal(pot, 0) // 처음에는 팟머니가 없는 상황이라 0
  })
  ```  

<br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_29.png" width="75%" height="45%" >  

<br/>
contracts 폴더의 Lottery.sol 파일에 코드 추가 ( 기본적인 Queue에 대한 코드 추가 )  
  ``` 
  pragma solidity >=0.4.21 <0.7.0;

  contract Lottery {

     struct BetInfo {
        uint256 answerBlockNumber; // 맞추려는 정답 블록
        address payable bettor; // 맞추었을때 돈을 보내주어야 할 better
        byte challenges; // 0xab 와 같은 1바이트 글자
     }

    uint256 private _tail;
    uint256 private _head;
    mapping (uint256 => BetInfo) private _bets; // 큐

    uint256 constant internal BLOCK_LIMIT = 256; // 블록해시로 확인할 수 있는 제한 256으로 고정 
    uint256 constant internal BET_BLOCK_INTERVAL = 3; // +3 번째블럭으로 고정 
    uint256 constant internal BET_AMOUNT = 5 * 10 ** 15; // 0.005ETH 베팅금액 고정
     uint256 private _pot; // 팟머니 저장해야 할 곳

     address public owner; // public 으로 선언하여 자동 getter 생성

     constructor() public {
        owner = msg.sender;
    }

    function getSomeValue() public pure returns (uint256 value){
        return 5;
    }

    function getPot() public view returns (uint256 pot) { // 팟머니 get 하는 함수 
        return _pot;
    }

    //Bet 베팅하는 function - 큐에 값을 저장

    //Distribute 검증하는 function - 결과값을 검증


     function getBetInfo(uint256 index) public view returns (uint256 answerBlockNumber, address bettor, byte challenges) {
        BetInfo memory b = _bets[index];
        answerBlockNumber = b.answerBlockNumber;
        bettor = b.bettor;
        challenges = b.challenges;
    }

    function pushBet(byte challenges) internal returns (bool) {
        BetInfo memory b;
        b.bettor = msg.sender; // 20 byte
        b.answerBlockNumber = block.number + BET_BLOCK_INTERVAL; // 32byte  20000 gas
        b.challenges = challenges; // byte // 20000 gas

        _bets[_tail] = b;
        _tail++; // 32byte 값 변화 // 20000 gas -> 5000 gas

        return true;
    }

    function popBet(uint256 index) internal returns (bool) {
        delete _bets[index];
        return true;
    }
  }
  ``` 

<br/>
* * *
<br/>
<h3>< Lottery Bet 함수 구현 ></h3>  

<br/>
contracts 폴더의 Lottery.sol 파일에 코드 추가 ( Bet 함수 추가 )  
  ``` 
  event BET(uint256 index, address indexed bettor, uint256 amount, byte challenges, uint256 answerBlockNumber);
  
  //Bet 베팅하는 function - 큐에 값을 저장
  /**
   * @dev 베팅을 한다. 유저는 0.005 ETH를 보내야 하고, 베팅용 1 byte 글자를 보낸다.
   * 큐에 저장된 베팅 정보는 이후 distribute 함수에서 해결된다.
   * @param challenges 유저가 베팅하는 글자
   * @return 함수가 잘 수행되었는지 확인해는 bool 값
   */
  function bet(byte challenges) public payable returns (bool result) { // 돈을 보내기 위해 payable 사용
      // Check the proper ether is sent - 돈이 제대로 왔는지 확인
      require(msg.value == BET_AMOUNT, "Not enough ETH");

      // Push bet to the queue
      require(pushBet(challenges), "Fail to add a new Bet Info");

      // Emit event
      emit BET(_tail - 1, msg.sender, msg.value, challenges, block.number + BET_BLOCK_INTERVAL);

      return true;
  }
  ``` 

<br/>
test 폴더의 lottery.test.js 파일 코드 수정 ( Basic test 제거 )  
  ``` 
  const Lottery = artifacts.require("Lottery");

  contract('Lottery', function([deployer, user1, user2]){ // 각각의 파라미터에는 10개의 주소중 순서대로 들어감
      let lottery;
      beforeEach(async () => {
          console.log('Before each')    
          lottery = await Lottery.new();  // 컨트랙트 배포 
      })

      it.only('getPot should return current pot', async () => { // 특정 케이스만 테스트 하기 위해 only 추가
          let pot = await lottery.getPot();
          assert.equal(pot, 0) // 처음에는 팟머니가 없는 상황이라 0
      })

  });
  
 ```  

<br/>
컴파일  
<img src="/assets/imgs/Lottery&Dapp_30.png" width="70%" height="40%" >

<br/>
* * *
<br/>
<h3>< Lottery Bet 함수 테스트 ></h3>  

<br/>
test 폴더의 lottery.test.js 파일 코드 추가    
  ```
  describe('Bet', function () {
        it.only('should fail when the bet money is not 0.005 ETH', async () => { //돈이 적절히 들어왔는지 확인
            // Fail transaction
            await lottery.bet('0xab', {from : user1, value:4000000000000000}); // 0.004 ETH
            // transaction object {chainId, value, to, from, gas(Limit), gasPrice}
        })
        it('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
            // bet

            // check contract balance

            // check bet info

            // check log

        })
    })
  ```

<br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )    
<img src="/assets/imgs/Lottery&Dapp_31.png" width="75%" height="45%" >   

+ 0.005ETH가 아니라서, Lottery.sol파일의 "Not enough ETH" 메세지 require문 출력  


<br/>
정상 트랜잭션 테스트 : test 폴더의 lottery.test.js 파일 코드 수정 ( 0.004ETH -> 0.005ETH ) 
  ```
  it.only('should fail when the bet money is not 0.005 ETH', async () => { //돈이 적절히 들어왔는지 확인
       // Fail transaction
       await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.004 ETH
       // transaction object {chainId, value, to, from, gas(Limit), gasPrice}
  })
  ```

<br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )    
<img src="/assets/imgs/Lottery&Dapp_32.png" width="75%" height="45%" >  
-> 테스트 통과  

<br/>
Fail 났을 때 어떻게 Catch 할 것 인가 -> Helper tool 사용 -> open zeppelin의 스마트 컨트랙트 툴 사용  ( 오픈소스 컨트랙트 라이브러리 모음 )  
+ 이번 실습에서는 openzeppelin-test-helpers/shouldFail.js 파일의 코드 참고  

<br/>
test폴더 아래에 assertRevert.js 파일 생성 후 코드작성  
  ```
  module.exports = async (promise) => {
      try {
          await promise;
          assert.fail('Expected revert not received');
      } catch (error) {
          const revertFound = error.message.search('revert') >= 0;
          assert(revertFound, `Expected "revert", got ${error} instead`);
      }
  }
  ```

<br/>
test 폴더의 lottery.test.js 파일에 assertRevert 추가  
  ```
  const assertRevert = require('./asserRevert');
  
  it.only('should fail when the bet money is not 0.005 ETH', async () => { //돈이 적절히 들어왔는지 확인
       // Fail transaction
       await assertRevert(lottery.bet('0xab', {from : user1, value:4000000000000000})); // 0.004 ETH
       // transaction object {chainId, value, to, from, gas(Limit), gasPrice}
  })
  ```
+ assertRevert function안에서 트랜잭션이 Fail시 던지는 에러를 try catch문으로 'revert' 글자가 들어있는지 확인 ( 글자가 있으면 Fail Catch )  


<br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )    
<img src="/assets/imgs/Lottery&Dapp_33.png" width="75%" height="45%" >  
-> Fail난 트랜잭션이 잘 Catch 된것 확인 가능  

<br/>
test 폴더의 lottery.test.js 파일 코드 수정 ( 트랜잭션 receipt를 받아 찍어봄 )    
  ```
  it.only('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
       // bet
       let receipt = await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.005 ETH
       console.log(receipt);
       
       // check contract balance

       // check bet info

       // check log
    
  })
  ```

<br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )    
<img src="/assets/imgs/Lottery&Dapp_34.png" width="75%" height="45%" >  
-> 트랜잭션 receipt이 출력됨  


<br/>
test 폴더의 lottery.test.js 파일 코드 수정 ( 팟머니를 받아 찍어봄 )    
  ```
  it.only('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
       // bet
       let receipt = await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.005 ETH
       //console.log(receipt);
       
       let pot = await lottery.getPot();
       assert.equal(pot, 0);
       
       // check contract balance

       // check bet info

       // check log
    
  })
  ```

<br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )    
<img src="/assets/imgs/Lottery&Dapp_35.png" width="75%" height="45%" >  
-> 테스트 통과  


<br/>
test 폴더의 lottery.test.js 파일 코드 수정 ( 컨트랙트 Balance 확인 )  
  ```
  it.only('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
       // bet
       let receipt = await lottery.bet('0xab', {from : user1, value:5000000000000000}); // 0.005 ETH
       //console.log(receipt);

       let pot = await lottery.getPot();
       assert.equal(pot, 0);

       // check contract balance == 0.005
       let contractBalance = await web3.eth.getBalance(lottery.address);
       assert.equal(contractBalance, 5000000000000000)
       
       // check bet info

       // check log

    })
  ```

<br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )    
<img src="/assets/imgs/Lottery&Dapp_36.png" width="75%" height="45%" >  
-> 테스트 통과  

<br/>
test 폴더의 lottery.test.js 파일 코드 수정 ( betAmount 변수추가 (0.005ETH), betInfo 확인 )  
  ```
  it.only('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
       // bet
       let receipt = await lottery.bet('0xab', {from : user1, value:betAmount}); // 0.005 ETH
       //console.log(receipt);

       let pot = await lottery.getPot();
       assert.equal(pot, 0);

       // check contract balance == 0.005
       let contractBalance = await web3.eth.getBalance(lottery.address);
       assert.equal(contractBalance, betAmount)

       // check bet info // 베팅 info가 제대로 들어왔는지
       let currentBlockNumber = await web3.eth.getBlockNumber();

       let bet = await lottery.getBetInfo(0);
       assert.equal(bet.answerBlockNumber, currentBlockNumber + bet_block_interval); // answerBlockNumber 확인
       assert.equal(bet.bettor, user1); // bettor 확인
       assert.equal(bet.challenges, '0xab') // challenges 확인

       // check log

  })
  ```
  
<br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )    
<img src="/assets/imgs/Lottery&Dapp_37.png" width="75%" height="45%" >  
-> 테스트 통과    
  
  
<br/>
receipt 찍었을 때 logs 의 event에 'BET'이 있는지 확인 해야함  ->  test폴더 아래에 expectEvent.js 파일 생성 후 코드작성  
  ```
  const assert = require('chai').assert;

  const inLogs = async (logs, eventName) => {
      const event = logs.find(e => e.event === eventName);
      assert.exists(event);
  }

  module.exports = {
      inLogs
  }
  ```

<br/>
expectEvent.js 파일 에서 chai를 사용하기 위해 설치 ( npm install chai 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_38.png" width="75%" height="45%" >  

<br/>
test 폴더의 lottery.test.js 파일 코드 수정
  ```
  it.only('should put the bet to the bet queue with 1 bet', async () => { // 값이 적절히 들어왔는지 확인
       // bet
       let receipt = await lottery.bet('0xab', {from : user1, value:betAmount}); // 0.005 ETH
       //console.log(receipt);

       let pot = await lottery.getPot();
       assert.equal(pot, 0);

       // check contract balance == 0.005
       let contractBalance = await web3.eth.getBalance(lottery.address);
       assert.equal(contractBalance, betAmount)

       // check bet info // 베팅 info가 제대로 들어왔는지
       let currentBlockNumber = await web3.eth.getBlockNumber();

       let bet = await lottery.getBetInfo(0);
       assert.equal(bet.answerBlockNumber, currentBlockNumber + bet_block_interval); // answerBlockNumber 확인
       assert.equal(bet.bettor, user1); // bettor 확인
       assert.equal(bet.challenges, '0xab') // challenges 확인

       // check log
       await expectEvent.inLogs(receipt.logs, 'BET')
  })
  ```
  
 <br/>
테스트 파일 실행 ( truffle test test/lottery.test.js 명령어 사용 )    
< img src="/assets/imgs/Lottery&Dapp_39.png" width="75%" height="45%" >  
-> 테스트 통과  
  
  
<br/>
* * *
<br/>
<h3>< 이더리움 GAS 계산 ></h3>  

<br/>  
+ <h4>Ethereum 수수료</h4>
1. gas ( gas Limit )  
2. gas Price  
3. ETH
  a. 1ETH = 10 ** 18 wei   
4. 수수료 = gas * gas Price  
  ex ) 21000 * ( 1gwei = 10 ** 9 wei ) = 21000000000000 wei = 0.000021 ETH  
  
+ <h4>GAS 계산</h4>
1. 32bytes 새로 저장 = 20000 gas 사용   
  + block의 gas Limit 은 block안에 있는 모든 트랜잭션들이 사용할 수 있는 gas 제한 하는것 ( block Limit 이상의 연산 불가 )
2. 32bytes 기존 변수에 있는 값을 바꿀 때 = 5000 gas 사용
  a. 기존변수를 초기화해서 사용하지 않을 때 -> 10000 gas return  
  
<br/>
스마트 컨트랙트 새로 배포 ( truffle migrate -- reset 명령어 사용 )      
< img src="/assets/imgs/Lottery&Dapp_40.png" width="75%" height="45%" >  
< img src="/assets/imgs/Lottery&Dapp_41.png" width="75%" height="45%" >  

<br/>
truffle 콘솔 사용 하여 bet 함수 테스트 ( truffle console 명령어 사용 )      
< img src="/assets/imgs/Lottery&Dapp_42.png" width="75%" height="45%" >  
< img src="/assets/imgs/Lottery&Dapp_43.png" width="75%" height="45%" >  
-> bet function : 89338 gas 사용  


<br/>
truffle 콘솔 사용 하여 bet 함수 다시 테스트  
< img src="/assets/imgs/Lottery&Dapp_44.png" width="75%" height="45%" > 
-> bet function : 74338 gas 사용  

=>> bet function : 89338 -> 74338 ==>> 기존에 있는 값을 바꿈으로 15000 gas 소모  

