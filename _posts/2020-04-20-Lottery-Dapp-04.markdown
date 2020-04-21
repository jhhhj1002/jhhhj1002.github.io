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
