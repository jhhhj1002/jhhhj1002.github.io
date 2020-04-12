---
layout: post
title:  "[강의수강] 초보자를 위한 Lottery Dapp 개발 - 중"
excerpt: "인프런 강의 - Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 : Dapp설계방법 및 Lottery 규칙, Lottery Domain 및 Queue 설계, Lottery Bet 함수 구현, Lottery Bet 함수 테스트, 이더리움 GAS 계산"
date:   2020-04-12 13:38:36 +0530
categories: Ethereum Dapp Truffle
---

<br/>

<h3>< Dapp설계방법 및 Lottery 규칙 ></h3>  

#Dapp 서비스 설계
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
#Lottery 규칙  
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
<img src="/assets/imgs/Lottery&Dapp_29.png" width="70%" height="40%" >  

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









