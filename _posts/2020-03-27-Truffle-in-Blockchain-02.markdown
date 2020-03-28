---
layout: post
title:  "[강의수강] 블록체인 Dapp 개발에 트러플 활용하기_기본편 - 중"
date:   2020-03-27 18:38:36 +0530
categories: Blockchain Dapp Truffle
---

< 로컬 이더리움( Ganache )에 배포하기 >

Ganache 설치

<img src="/assets/imgs/Blockchain&Truffle_11.png" width="80%" height="45%" >

Quick start 선택  
  RPC Server : HTTP://127.0.0.1:7545  
  NETWORK ID : 5777  

<img src="/assets/imgs/Blockchain&Truffle_12.png" width="80%" height="45%" >

+ 실제운영서버, 테스트넷(Testnet), 매인넷(Mainnet), 로컬 에 컴파일된 SmartContract 배포 가능

이번 실습에서는 로컬 (Ganache)에 배포 예정
배포를 위해서는 migrations 폴더 아래에 JavaScript 코드 작성 필요


배포 Script 작성 ( ex - 파일명 : 2_deploy_hello.js )

```javascript

const helloworld = artifacts.require("HelloWorld"); // HelloWorld 는 컨트랙트 이름

module.exports = function(deployer){
    deployer.deploy(helloworld,"Hello,World!"); // 생성자에게 "Hello,World!" 파라미터를 넘겨줌 ( 파라미터가 여러개인 경우 콤마 사용 )
};

```

배포 target 설정 ( truffle-config.js 의 networks 에서 설정 가능 )  
   -> development 에 배포 가능  
  Ganache의 Network Id, Port 번호 참고  

```

 development: {
 host: "127.0.0.1",     // Localhost (default: none)
 port: 7545,            // Standard Ethereum port (default: none)
 network_id: "5777",       // Any network (default: none)
 },

```

배포 명령어 : truffle migrate + 배포 타깃 지정 ( 없을시 development로 자동 배포 )
  + 처음부터 새로 배포 : truffle migrate --reset

ex - 타겟 설정 (development) : truffle migrate --network development  
      -> development 에 설정된 local Ganache 에 배포 가능  
ex - ropsten 테스트넷에 배포 : truffle migrate --network ropsten  
      -> ropsten 에 설정된 테스트넷에 배포 가능  



로컬 Ganache에 배포   
  -> 배포 2개 완료 , 컨트랙트 주소( contract address ) 부여 완료 
<img src="/assets/imgs/Blockchain&Truffle_13.png" width="80%" height="45%" >  
<img src="/assets/imgs/Blockchain&Truffle_14.png" width="80%" height="45%" >


< Rinkeby에 배포하기( truffle-hdwallet-provider ) >

실제 테스트넷에 배포하기 ( ex - 테스트넷 : ropsten / rinkeby)
  -> 이번 실습에서는 rinkeby에 배포
  
공개된 테스트넷을 이용하기 위해서는 외부서비스 사용 필요   
  -> [Infura] 에서 이더리움 네트워크에 접근할 수 있는 인프라 서비스 제공 


HDWalletProvider 패키지 설치 ( 배포시 전자서명을 해야하기 때문 )  
    truffle-config.js 상단의 HDWalletProvider 선언 주석 제거,   
    @truffle/hdwallet-provider 설치( npm install @truffle/hdwallet-provider) 
```
const HDWalletProvider = require('@truffle/hdwallet-provider');
```
<img src="/assets/imgs/Blockchain&Truffle_15.png" width="80%" height="45%" > 


Infura 사이트에서 API key 발급 필요
provider 와 network_id 를 제외한 나머지는 주석처리를 하여 default 값 사용가능 
private key : 메타마스크의 계정키  
private key를 HDWalletProvider의 key로 넘겨주어야함  
    new HDWalletProvider( private key, API key ) 로 설정  
rinkeby의 network_id 는 4  
```
     rinkeby: {
       provider: () => new HDWalletProvider("0F8368B7339B099A5A2DC0D25A6414CEDD79F5EC13BE3618B4820B1B12AE6FFF",   "https://rinkeby.infura.io/v3/46d357e8ef4242ca8dc45778639636c1"),
       network_id: 4,       // Ropsten's id
//       gas: 5500000,        // Ropsten has a lower block limit than mainnet
//       confirmations: 2,    // # of confs to wait between deployments. (default: 0)
//       timeoutBlocks: 200,  // # of blocks before a deployment times out  (minimum/default: 50)
//       skipDryRun: true     // Skip dry run before migrations? (default: false for public nets )
     },
```


rinkeby 테스트넷에 만든 SmartContract 배포 ( truffle migrate --network rinkeby --reset )  
<img src="/assets/imgs/Blockchain&Truffle_16.png" width="80%" height="45%" > 
<img src="/assets/imgs/Blockchain&Truffle_17.png" width="80%" height="45%" > 
<img src="/assets/imgs/Blockchain&Truffle_18.png" width="80%" height="45%" > 
<img src="/assets/imgs/Blockchain&Truffle_19.png" width="80%" height="45%" > 





[Infura] : https://infura.io/
