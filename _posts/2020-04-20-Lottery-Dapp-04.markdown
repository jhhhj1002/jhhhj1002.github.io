---
layout: post
title:  "[강의수강] 초보자를 위한 Lottery Dapp 개발 - 3"
excerpt: "인프런 강의 - Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 : Lottery Distribute 함수 설계, Lottery isMatch 함수 구현 및 테스트, Lottery Distribute 함수 구현, Lottery Distribute 함수 테스트, Lottery 컨트랙트 리뷰"
date:   2020-04-20 13:38:36 +0530
categories: Ethereum Dapp Truffle
---


<br/>

<h3>< Lottery Distribute 함수 설계 ></h3>  
  
<br/>
Lottey.sol 파일에 distribute 함수 추가  
    
```
/**
 * @dev 베팅 결과값을 확인 하고 팟머니를 분배한다.
 * 정답 실패 : 팟머니 축척, 정답 맞춤 : 팟머니 획득, 한글자 맞춤 or 정답 확인 불가 : 베팅 금액만 획득
*/
function distribute() public {
  // head 3 4 5 6 7 8 9 10 11 12 tail // 큐 - 새로운 정보는 tail방향 부터 추가
  uint256 cur; // head 부터 tail 방향으로 도는 루프

  BetInfo memory b;
  BlockStatus currentBlockStatus; // 현재 BlockStatus

  for(cur=_head;cur<_tail;cur++) {
      b = _bets[cur]; // 베팅 info 불러옴
      currentBlockStatus = getBlockStatus(b.answerBlockNumber);
      // Checkable : block.number > AnswerBlockNumber && block.number  <  BLOCK_LIMIT + AnswerBlockNumber 1 
      // 정답을 체크할 수 있는 상태
      if(currentBlockStatus == BlockStatus.Checkable) {
          // if win, bettor gets pot
              
          // if fail, bettor's money goes pot
                
          // if draw, refund bettor's money 

       }

       // Not Revealed : block.number <= AnswerBlockNumber 2 // 블락이 마이닝 되지 않은 상태
       if(currentBlockStatus == BlockStatus.NotRevealed) {
           break;
       }

       // Block Limit Passed : block.number >= AnswerBlockNumber + BLOCK_LIMIT 3 // 블락이 제한이 지났을 때
       if(currentBlockStatus == BlockStatus.BlockLimitPassed) {
           // refund
           // emit refund
       }

       popBet(cur);
   }
}
```  

<br/>
Lottey.sol 파일에 getBlockStatus 함수 추가  
```
enum BlockStatus {Checkable, NotRevealed, BlockLimitPassed}

function getBlockStatus(uint256 answerBlockNumber) internal view returns (BlockStatus) { // BlockStatus 리턴
    if(block.number > answerBlockNumber && block.number  <  BLOCK_LIMIT + answerBlockNumber) {
        return BlockStatus.Checkable;
    }

    if(block.number <= answerBlockNumber) {
        return BlockStatus.NotRevealed;
    }

    if(block.number >= answerBlockNumber + BLOCK_LIMIT) {
         return BlockStatus.BlockLimitPassed;
    }

    return BlockStatus.BlockLimitPassed;
  }  
``` 
  


<br/>
* * *
<br/>
<h3>< Lottery isMatch 함수 구현 및 테스트 ></h3> 
  
<br/>
Lottey.sol 파일에 isMatch 함수 추가     
```
enum BettingResult {Fail, Win, Draw}

/**
* @dev 베팅글자와 정답을 확인한다.
* @param challenges 베팅 글자
* @param answer 블락해쉬
* @return 정답결과
*/
function isMatch(byte challenges, bytes32 answer) public pure returns (BettingResult) {
    // challenges 0xab
    // answer 0xab......ff 32 bytes

    byte c1 = challenges;
    byte c2 = challenges;

    byte a1 = answer[0];
    byte a2 = answer[0];

    // Get first number
    c1 = c1 >> 4; // 0xab -> 0x0a
    c1 = c1 << 4; // 0x0a -> 0xa0

    a1 = a1 >> 4;
    a1 = a1 << 4;

    // Get Second number
    c2 = c2 << 4; // 0xab -> 0xb0
    c2 = c2 >> 4; // 0xb0 -> 0x0b

    a2 = a2 << 4;
    a2 = a2 >> 4;

    if(a1 == c1 && a2 == c2) {
        return BettingResult.Win;
    }

    if(a1 == c1 || a2 == c2) {
        return BettingResult.Draw;
    }

    return BettingResult.Fail;

}
``` 

<br/>
컴파일 ( truffle compile 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_45.png" width="75%" height="45%" >  

<br/>
truffle console로 blockHash 가지고옴 ( 아무 hash 가지고 와도 상관없음 )  
<img src="/assets/imgs/Lottery&Dapp_46.png" width="75%" height="45%" >  
-> blockHash : '0x60be4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86'  

<br/>
test 폴더 아래의 lottery.test.js 파일에 isMatch 함수 테스트 코드 추가 ( 위에서 가져온 blockHash 사용, 맨 앞만 ab로 바꿔서 사용 )    
```
describe.only('isMatch', function () {
    it('should be BettingResult.Win when two characters match', async () => {
        let blockHash = '0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86'
        let matchingResult = await lottery.isMatch('0xab', blockHash);
        assert.equal(matchingResult, 1);
     })
})
```

<br/>
lottery.test.js 파일 테스트 ( truffle test test/lottery.test.js 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_47.png" width="75%" height="45%" >  

<br/>
test 폴더 아래의 lottery.test.js 파일에 isMatch 함수 테스트 코드 추가 ( Fail, Draw 경우 추가 )  
```
describe.only('isMatch', function () {
    let blockHash = '0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86'
    it('should be BettingResult.Win when two characters match', async () => {            
        let matchingResult = await lottery.isMatch('0xab', blockHash);
        assert.equal(matchingResult, 1);
    })

    it('should be BettingResult.Fail when two characters match', async () => {
        let matchingResult = await lottery.isMatch('0xcd', blockHash);
        assert.equal(matchingResult, 0);
    })

    it('should be BettingResult.Draw when two characters match', async () => {
        let matchingResult = await lottery.isMatch('0xaf', blockHash);
        assert.equal(matchingResult, 2);

        matchingResult = await lottery.isMatch('0xfb', blockHash);
        assert.equal(matchingResult, 2);
    })
})
```

<br/>
lottery.test.js 파일 다시 테스트 ( truffle test test/lottery.test.js 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_48.png" width="75%" height="45%" >  
=>> isMatch 함수에 대한 테스트 완료  



<br/>
* * *
<br/>
<h3>< Lottery Distribute 함수 구현 ></h3> 

<br/>
스마트 컨트랙트 안에서 이더를 전송하는 3가지 방법  
**매우 조심해서 사용**    
1. call : 이더전송 + 다른 스마트컨트랙트의 특정 함수 호출 -> 함수호출시 같이 이더 전송 가능  
-> 외부의 스마트 컨트랙트 함부로 호출시 매우 위험  
2. send : 돈을 보내도 False 값 리턴   
3. transfer : 가장 많이 사용, 이더 던지기 실패시 스마트 컨트랙트 트랜잭션 자체 Fail시킴 -> 가장 안전     

  
<br/>
Lottey.sol 파일 수정  
( transferAfterPayingFee 함수, setAnswerForTest 함수, getAnswerBlockHash 함수, event들 추가, Distribute 함수 완성 )  
```
event WIN(uint256 index, address bettor, uint256 amount, byte challenges, byte answer, uint256 answerBlockNumber);
event FAIL(uint256 index, address bettor, uint256 amount, byte challenges, byte answer, uint256 answerBlockNumber);
event DRAW(uint256 index, address bettor, uint256 amount, byte challenges, byte answer, uint256 answerBlockNumber);
event REFUND(uint256 index, address bettor, uint256 amount, byte challenges, uint256 answerBlockNumber);
    
address payable public owner; // -> 기존에 있던 변수에 payable 추가 
bytes32 public answerForTest;
bool private mode = false; // false : use answer for test ( 테스트모드 ), true : use real block hash ( real모드 )
    // 랜덤값 생성시에는 이에대한 테스트가 어려움 -> 임시정답 세팅 후 모드를 바꿔주며 ( ex - 테스트형 모드, 배포형 모드 ) 테스트

// 수수료 떼가는 함수 - 일정량의 수수료 스마트컨트랙트 owner에게 줌
function transferAfterPayingFee(address payable addr, uint256 amount) internal returns (uint256) { 
        
    // uint256 fee = amount / 100; //1퍼센트를 수수료로 
    uint256 fee = 0; // 테스트시에는 0으로 지정하여 테스트 
    uint256 amountWithoutFee = amount - fee;
    
    // transfer to addr
    addr.transfer(amountWithoutFee);
    
    // transfer to owner
    owner.transfer(fee);

     return amountWithoutFee;
}

function setAnswerForTest(bytes32 answer) public returns (bool result) {
    require(msg.sender == owner, "Only owner can set the answer for test mode");
    answerForTest = answer;
    return true;
}
    
function getAnswerBlockHash(uint256 answerBlockNumber) internal view returns (bytes32 answer) {
    return mode ? blockhash(answerBlockNumber) : answerForTest;
}

/**
* @dev 베팅 결과값을 확인 하고 팟머니를 분배한다.
* 정답 실패 : 팟머니 축척, 정답 맞춤 : 팟머니 획득, 한글자 맞춤 or 정답 확인 불가 : 베팅 금액만 획득
*/
function distribute() public {
    // head 3 4 5 6 7 8 9 10 11 12 tail // 큐 - 새로운 정보는 tail방향 부터 추가
    uint256 cur; // head 부터 tail 방향으로 도는 루프
    uint256 transferAmount;

    BetInfo memory b;
    BlockStatus currentBlockStatus; // 현재 BlockStatus
    BettingResult currentBettingResult;

    for(cur=_head;cur<_tail;cur++) {
        b = _bets[cur]; // 베팅 info 불러옴
        currentBlockStatus = getBlockStatus(b.answerBlockNumber);
        // Checkable : block.number > AnswerBlockNumber && block.number  <  BLOCK_LIMIT + AnswerBlockNumber 1 
        // 정답을 체크할 수 있는 상태
        if(currentBlockStatus == BlockStatus.Checkable) {
            bytes32 answerBlockHash = getAnswerBlockHash(b.answerBlockNumber);
            currentBettingResult = isMatch(b.challenges, answerBlockHash);
            // if win, bettor gets pot
            if(currentBettingResult == BettingResult.Win) {
                // transfer pot //팟머니 옮겨줌
                transferAmount = transferAfterPayingFee(b.bettor, _pot + BET_AMOUNT);
                // pot = 0 
                _pot = 0;
                 // emit WIN
                 emit WIN(cur, b.bettor, transferAmount, b.challenges, answerBlockHash[0], b.answerBlockNumber);
            }
            // if fail, bettor's money goes pot
            if(currentBettingResult == BettingResult.Fail) {
                // pot = pot + BET_AMOUNT // 팟머니에 베팅한 amount 만큼 더해줌
                _pot += BET_AMOUNT;
                // emit FAIL
                emit FAIL(cur, b.bettor, 0, b.challenges, answerBlockHash[0], b.answerBlockNumber);
            }                
            // if draw, refund bettor's money 
            if(currentBettingResult == BettingResult.Draw) {
                // transfer only BET_AMOUNT // 베팅한 돈만큼만 돌려줌 
                transferAmount = transferAfterPayingFee(b.bettor, BET_AMOUNT);
                // emit DRAW
                emit DRAW(cur, b.bettor, transferAmount, b.challenges, answerBlockHash[0], b.answerBlockNumber);
            }
         }
         // Not Revealed : block.number <= AnswerBlockNumber 2 // 블락이 마이닝 되지 않은 상태
         if(currentBlockStatus == BlockStatus.NotRevealed) {
             break;
         }
         // Block Limit Passed : block.number >= AnswerBlockNumber + BLOCK_LIMIT 3 // 블락이 제한이 지났을 때
         if(currentBlockStatus == BlockStatus.BlockLimitPassed) {
             // refund
             transferAmount = transferAfterPayingFee(b.bettor, BET_AMOUNT);
             // emit refund
             emit REFUND(cur, b.bettor, transferAmount, b.challenges, b.answerBlockNumber);
        }

        popBet(cur);
    }
    _head = cur;
}
```
  
<br/>
컴파일 ( truffle compile 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_49.png" width="75%" height="45%" >  
  

<br/>
* * *
<br/>
<h3>< Lottery Distribute 함수 테스트 ></h3>  
  
<br/>
Lottey.sol 파일에 betAndDistribute 함수 추가     
```  
/**
* @dev 베팅과 정답 체크를 한다. 유저는 0.005 ETH를 보내야 하고, 베팅용 1 byte 글자를 보낸다.
* 큐에 저장된 베팅 정보는 이후 distribute 함수에서 해결된다.
* @param challenges 유저가 베팅하는 글자
* @return 함수가 잘 수행되었는지 확인해는 bool 값
*/
function betAndDistribute(byte challenges) public payable returns (bool result) {
   bet(challenges);

   distribute();

   return true;
}
``` 

<br/>
test 폴더 아래의 lottery.test.js 파일에 distribute 함수 테스트 코드 추가  
```
describe('Distribute', function () {
    describe('When the answer is checkable', function () { // 정답 확인할 수 있는 상황 
        it.only('should give the user the pot when the answer matches', async () => { // 두 글자 다 맞았을 때
            await lottery.setAnswerForTest('0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86', {from:deployer})

            // pot 의 변화량 확인

            // user(winner)의 밸런스를 확인

        })

        it('should give the user the amount he or she bet when a single character matches', async () => { // 한 글자 다 맞았을 때
        
        })

        it('should get the eth of user when the answer does not match at all', async () => { // 다 틀렸을 때
        
        })

    }) 
    describe('When the answer is not revealed(Not Mined)', function () { // 정답 확인할 수 없을 때 - 채굴되지 않은 경우 

    })
        
    describe('When the answer is not revealed(Block limit is passed)', function () { // 정답 확인할 수 없을 때 - block limit이 지난경우  

    })
}) 
```

<br/>
lottery.test.js 파일 테스트 ( truffle test test/lottery.test.js 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_50.png" width="75%" height="45%" >  

<br/>
test 폴더 아래의 lottery.test.js 파일에 distribute 함수 중 첫번째 it 코드 작성    
```
it.only('should give the user the pot when the answer matches', async () => { // 두 글자 다 맞았을 때
    await lottery.setAnswerForTest('0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86', {from:deployer})
                
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 1 -> 4
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 2 -> 5
    await lottery.betAndDistribute('0xab', {from:user1, value:betAmount}) // 3 -> 6
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 4 -> 7
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 5 -> 8
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 6 -> 9
                
    let potBefore = await lottery.getPot(); //  == 0.01 ETH
    let user1BalanceBefore = await web3.eth.getBalance(user1);
                
    let receipt7 = await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 7 -> 10 // user1에게 pot이 간다

    let potAfter = await lottery.getPot(); // == 0
    let user1BalanceAfter = await web3.eth.getBalance(user1); // == before + 0.015 ETH
                
    // pot 의 변화량 확인
    assert.equal(potBefore.toString(), new web3.utils.BN('10000000000000000').toString());
    assert.equal(potAfter.toString(), new web3.utils.BN('0').toString());

    // user(winner)의 밸런스를 확인
    user1BalanceBefore = new web3.utils.BN(user1BalanceBefore);
    assert.equal(user1BalanceBefore.add(potBefore).add(betAmountBN).toString(), new web3.utils.BN(user1BalanceAfter).toString())

})
```

<br/>
lottery.test.js 파일 테스트 ( truffle test test/lottery.test.js 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_51.png" width="75%" height="45%" >  


<br/>
test 폴더 아래의 lottery.test.js 파일에 distribute 함수 중 두번째 it 코드 작성    
```
it.only('should give the user the amount he or she bet when a single character matches', async () => { // 한 글자 다 맞았을 때
    await lottery.setAnswerForTest('0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86', {from:deployer})
                
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 1 -> 4
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 2 -> 5
    await lottery.betAndDistribute('0xaf', {from:user1, value:betAmount}) // 3 -> 6
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 4 -> 7
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 5 -> 8
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 6 -> 9
                
    let potBefore = await lottery.getPot(); //  == 0.01 ETH
    let user1BalanceBefore = await web3.eth.getBalance(user1);
                
    let receipt7 = await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 7 -> 10 // user1에게 pot이 간다

    let potAfter = await lottery.getPot(); // == 0.01 ETH
    let user1BalanceAfter = await web3.eth.getBalance(user1); // == before + 0.005 ETH
                
    // pot 의 변화량 확인
    assert.equal(potBefore.toString(), potAfter.toString());

    // user(winner)의 밸런스를 확인
    user1BalanceBefore = new web3.utils.BN(user1BalanceBefore);
    assert.equal(user1BalanceBefore.add(betAmountBN).toString(), new web3.utils.BN(user1BalanceAfter).toString())
})
```

<br/>
lottery.test.js 파일 테스트 ( truffle test test/lottery.test.js 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_52.png" width="75%" height="45%" > 


<br/>
test 폴더 아래의 lottery.test.js 파일에 distribute 함수 중 세번째 it 코드 작성    
```
it.only('should get the eth of user when the answer does not match at all', async () => { // 다 틀렸을 때
    await lottery.setAnswerForTest('0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86', {from:deployer})
                
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 1 -> 4
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 2 -> 5
    await lottery.betAndDistribute('0xef', {from:user1, value:betAmount}) // 3 -> 6
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 4 -> 7
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 5 -> 8
    await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 6 -> 9
                
    let potBefore = await lottery.getPot(); //  == 0.01 ETH
    let user1BalanceBefore = await web3.eth.getBalance(user1);
                
    let receipt7 = await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 7 -> 10 // user1에게 pot이 간다

    let potAfter = await lottery.getPot(); // == 0.015 ETH
    let user1BalanceAfter = await web3.eth.getBalance(user1); // == before
                
    // pot 의 변화량 확인
    assert.equal(potBefore.add(betAmountBN).toString(), potAfter.toString());

    // user(winner)의 밸런스를 확인
    user1BalanceBefore = new web3.utils.BN(user1BalanceBefore);
    assert.equal(user1BalanceBefore.toString(), new web3.utils.BN(user1BalanceAfter).toString())
})
```

<br/>
lottery.test.js 파일 테스트 ( truffle test test/lottery.test.js 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_53.png" width="75%" height="45%" > 


<br/>
test 폴더 아래의 lottery.test.js 파일에 distribute 함수 테스트 코드 완성       
```
let betAmountBN = new web3.utils.BN('5000000000000000'); // web3.js 의 Bin number 라이브러리 사용  

describe('Distribute', function () {
    describe('When the answer is checkable', function () { // 정답 확인할 수 있는 상황 
        it('should give the user the pot when the answer matches', async () => { // 두 글자 다 맞았을 때
            await lottery.setAnswerForTest('0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86', {from:deployer})
                
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 1 -> 4
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 2 -> 5
            await lottery.betAndDistribute('0xab', {from:user1, value:betAmount}) // 3 -> 6
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 4 -> 7
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 5 -> 8
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 6 -> 9
                
            let potBefore = await lottery.getPot(); //  == 0.01 ETH
            let user1BalanceBefore = await web3.eth.getBalance(user1);
                
            let receipt7 = await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 7 -> 10 // user1에게 pot이 간다

            let potAfter = await lottery.getPot(); // == 0
            let user1BalanceAfter = await web3.eth.getBalance(user1); // == before + 0.015 ETH
                
            // pot 의 변화량 확인
            assert.equal(potBefore.toString(), new web3.utils.BN('10000000000000000').toString());
            assert.equal(potAfter.toString(), new web3.utils.BN('0').toString());

            // user(winner)의 밸런스를 확인
            user1BalanceBefore = new web3.utils.BN(user1BalanceBefore);
            assert.equal(user1BalanceBefore.add(potBefore).add(betAmountBN).toString(), new web3.utils.BN(user1BalanceAfter).toString())

        })

        it('should give the user the amount he or she bet when a single character matches', async () => { // 한 글자 다 맞았을 때
            await lottery.setAnswerForTest('0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86', {from:deployer})
                
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 1 -> 4
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 2 -> 5
            await lottery.betAndDistribute('0xaf', {from:user1, value:betAmount}) // 3 -> 6
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 4 -> 7
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 5 -> 8
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 6 -> 9
                
            let potBefore = await lottery.getPot(); //  == 0.01 ETH
            let user1BalanceBefore = await web3.eth.getBalance(user1);
                
            let receipt7 = await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 7 -> 10 // user1에게 pot이 간다

            let potAfter = await lottery.getPot(); // == 0.01 ETH
            let user1BalanceAfter = await web3.eth.getBalance(user1); // == before + 0.005 ETH
                
            // pot 의 변화량 확인
            assert.equal(potBefore.toString(), potAfter.toString());

            // user(winner)의 밸런스를 확인
            user1BalanceBefore = new web3.utils.BN(user1BalanceBefore);
            assert.equal(user1BalanceBefore.add(betAmountBN).toString(), new web3.utils.BN(user1BalanceAfter).toString())
        })

        it('should get the eth of user when the answer does not match at all', async () => {
            // 다 틀렸을 때
            await lottery.setAnswerForTest('0xabbe4645a8d37dc4d3c0a2b9e34588af78cd3cd0a42cfa73b9116774fd6edf86', {from:deployer})
                
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 1 -> 4
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 2 -> 5
            await lottery.betAndDistribute('0xef', {from:user1, value:betAmount}) // 3 -> 6
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 4 -> 7
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 5 -> 8
            await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 6 -> 9
                
            let potBefore = await lottery.getPot(); //  == 0.01 ETH
            let user1BalanceBefore = await web3.eth.getBalance(user1);
                
            let receipt7 = await lottery.betAndDistribute('0xef', {from:user2, value:betAmount}) // 7 -> 10 // user1에게 pot이 간다

            let potAfter = await lottery.getPot(); // == 0.015 ETH
            let user1BalanceAfter = await web3.eth.getBalance(user1); // == before
                
            // pot 의 변화량 확인
            assert.equal(potBefore.add(betAmountBN).toString(), potAfter.toString());

            // user(winner)의 밸런스를 확인
            user1BalanceBefore = new web3.utils.BN(user1BalanceBefore);
            assert.equal(user1BalanceBefore.toString(), new web3.utils.BN(user1BalanceAfter).toString())
        })

    })

    describe('When the answer is not revealed(Not Mined)', function () { // 정답 확인할 수 없을 때 - 채굴되지 않은 경우 

    })
        
    describe('When the answer is not revealed(Block limit is passed)', function () { // 정답 확인할 수 없을 때 - block limit이 지난경우 

    })

})
```
