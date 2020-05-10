---
layout: post
title:  "[강의수강] 초보자를 위한 Lottery Dapp 개발 - 5"
excerpt: "인프런 강의 - Ethereum 실전! 초보자를 위한 Lottery Dapp 개발 : Lottery UI 개발, Lottery UI 기능 개발, 
React Lottery 마무리 작업, Lottery Dapp 리뷰"
date:   2020-05-10 22:50:36 +0530
categories: React Ethereum Dapp Truffle
---


<br/>

<h3>< Lottery UI 개발 ></h3>  

bootstrap library 추가 ( yarn add bootstrap 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_95.png" width="40%" height="30%" >   

<br/>
src 폴더 아래 index.js 파일 수정    
```
import 'bootstrap/dist/css/bootstrap.css'
```

<br/>
src 폴더 아래 App.js 파일 수정    
```
render() {
    return (
      <div className="App">
        {/* Header - Pot, Betting characters */}
        <div className="container">
          <div className="jumbotron">
            Lottery
          </div>
        </div>
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
```

chrome 에서 bootstrap 적용 확인 ( yarn start 명령어 사용 )  
<img src="/assets/imgs/Lottery&Dapp_96.png" width="40%" height="30%" >  

+ bootstrap4의 jumbotron 테마 활용 : https://www.w3schools.com/bootstrap4/bootstrap_jumbotron.asp  


<br/>
src 폴더 아래 App.js 파일 수정 ( constructor 추가, render 수정 )      
```
constructor(props) {
  super(props);

  this.state = {
    betRecords: [],
    winRecords: [],
    failRecords: [],
    pot: '0',
    challenges: ['A', 'B'],
    finalRecords: [{
      bettor:'0xabcd...',
      index:'0',
      challenges:'ab',
      answer:'ab',
      targetBlockNumber:'10',
      pot:'0'
    }]
  }
}

render() {
  return (
    <div className="App">
      {/* Header - Pot, Betting characters */}
      <div className="container">
        <div className="jumbotron">
          <h1>Current Pot : {this.state.pot}</h1>
          <p>Lottery</p>
          <p>Lottery tutorial</p>
          <p>Your Bet</p>
          <p>{this.state.challenges[0]} {this.state.challenges[1]}</p>
        </div>
      </div>
        
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
```

Chrome 새로고침 후, 변경사항 확인  
<img src="/assets/imgs/Lottery&Dapp_97.png" width="40%" height="30%" >  

<br/>
src 폴더 아래 App.js 파일 수정 ( getCard 추가, render 수정 )      
```
getCard = (_Character, _cardStyle) => {
  return (
    <button className={_cardStyle} >
      <div className ="card-body text-center">
        <p className="card-text"></p>
        <p className="card-text text-center">A</p>
        <p className="card-text"></p>
      </div>
    </button>
  )
}

render() {
    return (
      <div className="App">
        {/* Header - Pot, Betting characters */}
        <div className="container">
          <div className="jumbotron">
            <h1>Current Pot : {this.state.pot}</h1>
            <p>Lottery</p>
            <p>Lottery tutorial</p>
            <p>Your Bet</p>
            <p>{this.state.challenges[0]} {this.state.challenges[1]}</p>
          </div>
        </div>

        {/* Card section */}
        <div className="container">
          <div className="card-group">
            {this.getCard('A', 'card bg-primary')}
            {this.getCard('B', 'card bg-warning')}
            {this.getCard('C', 'card bg-danger')}
            {this.getCard('0', 'card bg-success')}
          </div>
        </div>


      </div>
    );
  }
}
```
+ bootstrap4 card 사용 : https://www.w3schools.com/bootstrap4/bootstrap_cards.asp  


Chrome 새로고침 후, 변경사항 확인 -> 4가지의 Card view 확인 가능   
<img src="/assets/imgs/Lottery&Dapp_98.png" width="40%" height="30%" >  



<br/>
src 폴더 아래 App.js 파일 수정 ( getCard 수정 )      
```
getCard = (_Character, _cardStyle) => {
  let _card = '';
  if(_Character === 'A'){
    _card = '🂡'
  }
  if(_Character === 'B'){
    _card = '🂱'
  }
  if(_Character === 'C'){
    _card = '🃁'
  }
  if(_Character === '0'){
    _card = '🃑'
  }

  return (
    <button className={_cardStyle}>
      <div className ="card-body text-center">
        <p className="card-text"></p>
        <p className="card-text text-center" style={{fontSize:300}}>{_card}</p>
        <p className="card-text"></p>
      </div>
    </button>
  )
}
```
+ card deck 사용 : https://en.wikipedia.org/wiki/Standard_52-card_deck 

Chrome 새로고침 후, 변경사항 확인     
<img src="/assets/imgs/Lottery&Dapp_99.png" width="40%" height="30%" >  

<br/>
src 폴더 아래 App.js 파일 수정 ( render 수정 )      
```
render() {
  return (
    <div className="App">
      {/* Header - Pot, Betting characters */}
      <div className="container">
        <div className="jumbotron">
          <h1>Current Pot : {this.state.pot}</h1>
          <p>Lottery</p>
          <p>Lottery tutorial</p>
          <p>Your Bet</p>
          <p>{this.state.challenges[0]} {this.state.challenges[1]}</p>
        </div>
      </div>

      {/* Card section */}
      <div className="container">
        <div className="card-group">
          {this.getCard('A', 'card bg-primary')}
          {this.getCard('B', 'card bg-warning')}
          {this.getCard('C', 'card bg-danger')}
          {this.getCard('0', 'card bg-success')}
        </div>
      </div>
      <br></br>
      <div className="container">
        <button className="btn btn-danger btn-lg">BET!</button>
      </div>
      <br></br>
      <div className="container">
        <table className="table table-dark table-striped">
          <thead>
            <tr>
              <th>Index</th>
              <th>Address</th>
              <th>Challenge</th>
              <th>Answer</th>
              <th>Pot</th>
              <th>Status</th>
              <th>AnswerBlockNumber</th>
            </tr>
          </thead>
          <tbody>
            {
              this.state.finalRecords.map((record, index) => {
                return (
                  <tr>
                    <td></td>
                    <td></td>
                    <td></td>
                    <td></td>
                    <td></td>
                    <td></td>
                    <td></td>
                  </tr>
                )
              })
            }
          </tbody>
        </table>
      </div>
    </div>
  );
}
```

Chrome 새로고침 후, 변경사항 확인 -> Bet버튼 생성, table 생성        
<img src="/assets/imgs/Lottery&Dapp_100.png" width="40%" height="30%" >  



<br/>
* * *
<br/>
<h3>< Lottery UI 기능 개발 ></h3> 

<br/>
src 폴더 아래 App.js 파일 수정 ( getPot 추가 )      
```
async componentDidMount() {
  await this.initWeb3();
  await this.getBetEvents();
  await this.getPot(); // getPot 테스트  
}
  
getPot = async () => {
  let pot = await this.lotteryContract.methods.getPot().call();
  let potString = this.web3.utils.fromWei(pot.toString(), 'ether'); // pot 숫자 -> 문자열
  this.setState({pot:potString})
}
```

Chrome 새로고침 후, 변경사항 확인 -> 실시간 팟머니 출력          
<img src="/assets/imgs/Lottery&Dapp_101.png" width="40%" height="30%" >  

<br/>
src 폴더 아래 App.js 파일 수정 ( componentDidMount 수정, pollData 추가, bet 함수 수정, render 수정 )      
```
async componentDidMount() {
  await this.initWeb3();
  // await this.pollData();
  setInterval(this.pollData, 1000); // 1초마다 호출  
}

pollData = async () => { 
  await this.getPot();
}

bet = async () => {
  // nonce - 특정 address 가 몇개의 트랜잭션들을 만들었는지 알수 있음 -> 트랜잭션 replay방지, 외부의 유저가 마음대로 사용하지 못하게 하는 기능o

  let challenges = '0x' + this.state.challenges[0].toLowerCase() + this.state.challenges[1].toLowerCase();
  let nonce = await this.web3.eth.getTransactionCount(this.account);
  this.lotteryContract.methods.betAndDistribute(challenges).send({from:this.account, value:5000000000000000, gas:300000, nonce:nonce})
}

render() {
   ...//생략
      <div className="container">
        <button className="btn btn-danger btn-lg" onClick={this.bet}>BET!</button> // onClick 추가 
      </div>
   ...//생략
  );
}
```


Chrome 새로고침 후, Bet 버튼 클릭시 상호작용 확인
- Bet 버튼 클릭 전 Metamask 이더 확인  
<img src="/assets/imgs/Lottery&Dapp_102.png" width="40%" height="30%" >  
- Bet 버튼 클릭후 승인 선택  
<img src="/assets/imgs/Lottery&Dapp_103.png" width="40%" height="30%" >  
- 계약 이후 Metamask 이더 확인  
<img src="/assets/imgs/Lottery&Dapp_104.png" width="40%" height="30%" >  
- Chrome 창에서 실시가 팟머니 확인  
<img src="/assets/imgs/Lottery&Dapp_105.png" width="40%" height="30%" >  


<br/>
src 폴더 아래 App.js 파일 수정 ( onClickCard 추가, getCard 수정, bet 함수 수정 )      
```
bet = async () => {
  // nonce - 특정 address 가 몇개의 트랜잭션들을 만들었는지 알수 있음 -> 트랜잭션 replay방지, 외부의 유저가 마음대로 사용하지 못하게 하는 기능o

  let challenges = '0x' + this.state.challenges[0].toLowerCase() + this.state.challenges[1].toLowerCase();
  let nonce = await this.web3.eth.getTransactionCount(this.account);
  this.lotteryContract.methods.betAndDistribute(challenges).send({from:this.account, value:5000000000000000, gas:300000, nonce:nonce})
  .on('transactionHash', (hash) =>{
    console.log(hash)
  })
}

onClickCard = (_Character) => {
  this.setState({
    challenges : [this.state.challenges[1], _Character]
  })
}
  
getCard = (_Character, _cardStyle) => {
  ...//생략
  
  return (
    <button className={_cardStyle} onClick = {() => {this.onClickCard(_Character)}}> // onclick 추가  
      <div className ="card-body text-center">
        <p className="card-text"></p>
        <p className="card-text text-center" style={{fontSize:300}}>{_card}</p>
        <p className="card-text"></p>
      </div>
    </button>
  )
}
```

Chrome 새로고침 후, console에서 Bet클릭 이후의 트랜잭션 해쉬 값 확인            
<img src="/assets/imgs/Lottery&Dapp_106.png" width="40%" height="30%" >  


<br/>
src 폴더 아래 App.js 파일 수정 ( pollData 수정, getBetEvents 수정, render 수정, getWinEvents 추가, getFailEvents 추가, makeFinalRecords 추가 )      
```
pollData = async () => {
  await this.getPot();
  await this.getBetEvents();
  await this.getWinEvents();
  await this.getFailEvents();
  this.makeFinalRecords();
}
  
getBetEvents = async () => {
  const records = [];
  let events = await this.lotteryContract.getPastEvents('BET', {fromBlock:0, toBlock:'latest'});
  console.log(events);
  for(let i=0;i<events.length;i+=1){
    const record = {}
    record.index = parseInt(events[i].returnValues.index, 10).toString();
    record.bettor = events[i].returnValues.bettor.slice(0,4) + '...' + events[i].returnValues.bettor.slice(40,42);
    record.betBlockNumber = events[i].blockNumber;
    record.targetBlockNumber = events[i].returnValues.answerBlockNumber.toString();
    record.challenges = events[i].returnValues.challenges;
    record.win = 'Not Revealed';
    record.answer = '0x00';
    records.unshift(record);
  }
  this.setState({betRecords:records})
}

getWinEvents = async () => {
  const records = [];
  let events = await this.lotteryContract.getPastEvents('WIN', {fromBlock:0, toBlock:'latest'});
    
  for(let i=0;i<events.length;i+=1){
    const record = {}
    record.index = parseInt(events[i].returnValues.index, 10).toString();
    record.amount = parseInt(events[i].returnValues.amount, 10).toString();
    records.unshift(record);
  }
  this.setState({winRecords:records})
}

getFailEvents = async () => {
  const records = [];
  let events = await this.lotteryContract.getPastEvents('FAIL', {fromBlock:0, toBlock:'latest'});
    
  for(let i=0;i<events.length;i+=1){
    const record = {}
    record.index = parseInt(events[i].returnValues.index, 10).toString();
    record.answer = events[i].returnValues.answer;
    records.unshift(record);
  }
  console.log(records);
  this.setState({failRecords:records})
}

makeFinalRecords = () => {
  let f = 0, w = 0;
  const records = [...this.state.betRecords];
  for(let i=0;i<this.state.betRecords.length;i+=1) {
    if(this.state.winRecords.length > 0 && this.state.betRecords[i].index === this.state.winRecords[w].index){
      records[i].win = 'WIN'
      records[i].answer = records[i].challenges;
      records[i].pot = this.web3.utils.fromWei(this.state.winRecords[w].amount, 'ether');
      if(this.state.winRecords.length - 1 > w) w++;
      
    } else if(this.state.failRecords.length > 0 && this.state.betRecords[i].index === this.state.failRecords[f].index){
        
      records[i].win = 'FAIL'
      records[i].answer = this.state.failRecords[f].answer;
      records[i].pot = 0;
      if(this.state.failRecords.length - 1 > f) f++;

    } else {
      records[i].answer = 'Not Revealed';
    }
  }
  this.setState({finalRecords:records})
}

render() {
   ...//생략
    <tbody>
       {
       this.state.finalRecords.map((record, index) => {
          return (
             <tr key={index}>
                <td>{record.index}</td>
                <td>{record.bettor}</td>
                <td>{record.challenges}</td>
                <td>{record.answer}</td>
                <td>{record.pot}</td>
                <td>{record.win}</td>
                <td>{record.targetBlockNumber}</td>
              </tr>
           )
         })
         }
     </tbody>
    ...//생략
  );
}
```

Chrome 새로고침 후, history 테이블 출려 확인  
<img src="/assets/imgs/Lottery&Dapp_107.png" width="40%" height="30%" >  










