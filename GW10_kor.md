## FPL GW10 레포트

## 인덱스
- 라인업
  - 포인트 결과
  - 프로세스: 선발 과정
    - 수비수
    - 미드필더
    - 공격수
  - 가설: SCA는 공격포인트 리턴의 효율적 지표인가?
- 포인트 분석
  - 예상
  - 결과
- 경기 분석 (eye-test)
- 결론
- 새로운 가설

### 라인업

![image](https://user-images.githubusercontent.com/51032518/100699276-de949300-33dd-11eb-9da5-2ce92fb0699c.png)

포인트 결과:
> GW10 average: 44  
> point return: 42  
> GW10 rating: 4.2M 

## 프로세스: 선발 과정

### 수비수

우선, FPL 전체 수비수들 중에서 엘리트 그룹을 선발하기 위해 출전 시간 90x4분 이상, 90분당 SCA 1.5이상을 기준으로 필터링을 했다. 

```
fpl_defenders = fpl_defenders.loc[(mrg_mid["90s"] > 4)]
fpl_defenders = fpl_defenders.loc[(mrg_mid["SCA90"] > 1.5)]
```

필터링 결과, 총 24명의 선수가 레이더에 걸렸다. 이 24명의 선수들로 SCA 지수가 공격포인트 리턴의 효율적 지표인지 살펴보았다. 

`*SCA: 슛팅으로 연결되는 직전 두 개의 동작으로, 패스, 드리블, 파울 유도 등이 포함된다. 예컨대, 선수A가 드리블 성공 후 선수B에게 패스해 선수B가 슛팅을 시도했다면, 선수A는 두 개의 SCA를 기록한다.)`

경기당 SCA가 높을수록 어시스트를 기록할 확률이 높다는 가정이 맞는지 확인해보기로 하자.

![Ast+AST90](https://user-images.githubusercontent.com/51032518/100705999-8b294180-33eb-11eb-96ec-a3a136443feb.png)  

![SCA_error_rate](https://user-images.githubusercontent.com/51032518/100706002-8bc1d800-33eb-11eb-8411-6648ebf61e1d.png)

의외로, 경기당 SCA지수가 어시스트 획득에 직결되는 지표가 아님을 오차 막대 그래프에서 확인할 수 있다.

이 오차율을 SCA90 상수로 써, 어시스트 획득 확률을 계산해 보았다. 

```
fpl_defenders['SCA90_constant'] =( fpl_defenders['SCA_error_rate']* fpl_defenders['SCA90_scaled'] ) 

```

![SCA_constant](https://user-images.githubusercontent.com/51032518/100707165-91b8b880-33ed-11eb-9caf-11e3f6df7890.png)

SCA90에 [Assist_scaled]-[SCA90_scaled]로 계산한 오차율을 상수로 곱하니 더 정교한 모델링 지표가 나왔다.

