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
        </table>
      </div>
    </div>
  );
}
```

Chrome 새로고침 후, 변경사항 확인 -> Bet버튼 생성, table 생성        
<img src="/assets/imgs/Lottery&Dapp_100.png" width="40%" height="30%" >  











