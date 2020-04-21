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
Lottey.sol 파일에 distribute 함수, getBlockStatus 함수 추가  
    
```
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
```
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


