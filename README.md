# Bitcoin과 stock(SP500) system trading


- 도구

    [![파이썬 Badge](https://img.shields.io/badge/python-3776AB?style=flat-square&logo=python&logoColor=white&link=mailto:wjtls01@naver.com)](mailto:wjtls01@naver.com)

    [![파이토치 Badge](https://img.shields.io/badge/creonAPI-EE4C2C?style=flat-square&logo=pytorch&logoColor=white&link=mailto:wjtls01@naver.com)](mailto:wjtls01@naver.com)
    
    [![파이토치 Badge](https://img.shields.io/badge/UpbitAPI-EE4C2C?style=flat-square&logo=pytorch&logoColor=white&link=mailto:wjtls01@naver.com)](mailto:wjtls01@naver.com)

    [![주피터 Badge](https://img.shields.io/badge/jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white&link=mailto:wjtls01@naver.com)](mailto:wjtls01@naver.com)

- 목표: 자동매매 로직 구현

- 구현 이유: 2017년 12월경 개인적으로 코인 트레이딩을 하며 단순한 전략으로 수익을 냈었다. <br/>
   이후 2021년도에 같은 전략을 사용해도 수익을 내지 못했는데, <br/>
   원인은 심리적 요인으로 인한 기계적인 손절 매매의 부재 및 시장에서 새로운 알파의 생성이었다.  <br/> <br/>
   따라서 수익을 내는 전략을 찾기 위한 목적 이라기 보단 개인의 전략을 백테스팅을 하여 수익률을 확인하고 <br/>
   향후 자동매매를 할 수 있는 환경을 구축하고자 프로젝트를 진행했다.
  
<br/><br/><br/><br/>

## 기능
  - 주식(Kindex SP500) 또는 비트코인 자동매매
  - upbit API 사용
  - Creon API 사용
  - creon(대신증권) 자동 로그인
  - 데이터 호출
  - 사용할 지표 계산 (stochastic RSI , Voluem ratio, 투자 심리도 )
  - 매매시 실시간 잔고 , 보유수량, 현금 등 확인
  - slack 을 활용하여 매매할경우 메세지 전송
  - 지정 시간동안만 매매

  <br/><br/><br/><br/>

## 본론

  - 과거 사용했던 전략(주식)
    - 1. stochastic RSI가 기준값 이하로 데드 크로스 이후 다시 골든크로스 할때 가진 금액의 50 % 매수.<br/>
      (주식은 정해진 추세가 코인에 비해 상대적으로 오래 지속되는 편이므로 골든크로스시 매수하는 전략 선택)<br/>
    - 2. 주식 보유도중 투자심리도가 10% 이하로 떨어질경우 추가 매수<br/>
    - 3. Volume Ratio 가 300% 이상이 될경우 전량 매도<br/>
 <br/>

  - 비트코인 매매 전략
    - 1. 15분봉의 stochastic RSI가 기준값 이하 이고, volume ratio가 기준값 이하인 경우  매수
    - 2. 실시간 stochastic RSI가 기준값 이하 이고, 투자 심리도가 기준값 이하인 경우 매수
    - 3. 여러 매매 전략을 설정( 보수적인 매매, 공격적인 매매)
    - 4. 보수적인 매매 전략: 지표들의 매매 조건을 까다롭게 설정하여 매매 횟수가 적어진다.
    - 5. 공격적인 매매 전략: 지표들의 매매 조건을 느슨하게 설정하여 매매 횟수가 많아진다.
   
  추가적으로 매수,매도 시점에 실시간으로 slack 메세지를 전송하였다.
  
  위와 같이 임의로 간단한 전략을 설정하여 백테스팅 및 실전 매매를 일주일간 진행했다.

  <br/><br/><br/>

 
## 결론 
  - (Kindex SP 500 )
    - 1. 백테스팅시 대부분의 전략은 시장 수익률보다 저조한 수익률을 보이거나 비슷한 수익률을 보인다.
    - 2. 지표들의 매매 기준값 또는 계산시 rolling 기간등 하이퍼 파라미터를 수정하면<br/> 
         백테스팅 기간의 시장수익률 보다 높은 수익률을 얻는 전략을 찾을수 있었다.
    
  - (Bitcoin)
    - 1. 공격적인 매매 전략으로 실전 트레이딩을 할 경우 상승장에서 시장수익률 보다 낮은 수익률을 보였고,<br/> 
         하락장에서 시장수익률과 비슷했다.
    - 2. 보수적인 매매 전략으로 실전 트레이딩을 할 경우 매매 횟수가 줄어들었기 때문에 <br/> 
         상승장에서 시장수익률 보다 낮은 수익률을 보였고, 하락장에서 시장 수익률 보다 수익률이 높았다
         
  - Kindex SP500 과 비트코인 트레이딩 결과  <br/> 
    비트코인 트레이딩 에서 사용한 전략은 과소적합(적절한 전략을 찾더라도 과적합 일 것이다.) ,  <br/> 
    Kindex SP500에서 찾은 전략은 과적합이었며 향후 실전 매매에서는 통용되지 않는다. (전략은 유동적으로 바뀌어야 한다는 결론) 
    
         
  <br/><br/><br/>
  

## 문제점 및 개선방안

   - 특정 기간동안의 적절한 퀀트 전략을 찾더라도 향후 시점에 통용되지 않았다.<br/> 
     -> 수많은 전략을 저장하고 알파가 바뀔 때 적절한 전략을 사용할 수도 있고, 더 복잡한 전략을 사용할 수도 있다.
