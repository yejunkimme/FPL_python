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

의외로 경기당 SCA지수가 어시스트 획득에 직결되는 지표가 아님을 오차 막대 그래프에서 확인할 수 있다. 결국 SCA 이외에도 다음과 같은 지표들을 함께 넣어 그래프를 그렸고, 이 중 다섯 명의 선수를 선발했다. 

![45 rotated](https://user-images.githubusercontent.com/51032518/100708229-4a332c00-33ef-11eb-85f3-278a2474485c.png)



**개선할 점:**

*1. SCA/90*

SCA/90와 어시스트간의 상관관계 오차율이 너무 커 SCA가 효율적인 지표로 활용되지 못했다. 이를 보완하기 위해 다음과 같은 수정사항을 제기했다. 스케일된 '어시스트-SCA/90' 값을 오차 상수로 써서, SCA/90값에 곱해보았다.

```
fpl_defenders['SCA90_constant'] =( fpl_defenders['SCA_error_rate']* fpl_defenders['SCA90_scaled'] ) 
```

![SCA_constant](https://user-images.githubusercontent.com/51032518/100707165-91b8b880-33ed-11eb-9caf-11e3f6df7890.png)

그 결과 더 정교한 모델링 지표가 나왔다. 몇몇 선수를 제하고 거의 정확하게 어시스트 확률을 계산할 수 있게 됐다.

*2. 포인트 획득 세분화 *

SCA/90 지표로 어시스트 확률을 계산할 수 있게 된 만큼 골, 무실점, 보너스 포인트로 인한 포인트 획득을 예측할 수 있는 지표들을 찾아내고 이를 통해 더욱 정교한 포인트 예측 모델을 만들어야 한다. 

### 미드필더


엘리트 그룹으로 추리기 위해 수비수와 마찬가지로 90x4분 이상의 출전시간, 경기당 슛팅 창출 행위 2.5회 이상으로 필터링을 했다.

```
fpl_midfielders = fpl_midfielders.loc[(mrg['90s'] > 4)]
fpl_midfielders = fpl_midfielders.loc[(mrg_mid["SCA90"] > 2.5)]
```

또한 상대 페널티 박스 안 터치 수 지표를 그래프에 추가해 비교했다. 페널티 박스 안 터치 수가 높을수록 골 혹은 어시스트를 기록할 확률이 높다는 가정에서다. 

![midfielder](https://user-images.githubusercontent.com/51032518/100709525-a303c400-33f1-11eb-8890-5fde926a071c.png)

이렇게 추려진 47명의 선수들을 그래프를 통해 보고 5명의 선수를 선발했다.

**개선할 점:**

1. 수비수를 선발할 때와 거의 같은 지표들을 통해 미드필더를 선발했다. 오직 페널티 박스 안 터치 수 지표를 추가해서 비교하기에는 더욱 정교한 지표들이 필요하다. 다음 선발 때는 xG, 슛팅 시도 등을 추가해 활용해 볼 생각이다.

### 공격수

미드필더 선발에 썼던 필터링을 그대로 공격수에도 적용했다.

![forwards](https://user-images.githubusercontent.com/51032518/100710664-5de09180-33f3-11eb-89c8-c45fcd4bd0a0.png)

**개선할 점:**

1. xG 값과 더불어 더 정교한 골 예측 지표가 필요하다.


## 포인트 분석

예측-결과 비교

이름|예측값|결과값|예측결과|(예측)-(결과)
----|-----|------|-------|------------
케인 (FW)|5 이상|	2	|실패	|-3
 칼버트 (FW)|	6 이상|	2|	실패|	-4
 페르난데스 (MF)|	5 이상|	10|	성공|	+5
 손흥민 (MF)|	5 이상|	3|	실패|	-2
 클리히 (MF)|	2+a|	3|	성공|	+1
 제네포 (MF)|	2+a|	5|	성공|	+3
 로버트슨 (DF)|	5|	2|	실패|	-3
 칠웰 (DF)|	5|	8|	성공|	+3
 마쉬아퀴 (DF)| 2+a|	1|	실패|	-1
 크레스웰 (DF)|	5|	2|	실패|	-3
 맥카시 (GK)|	2+a|	2|	성공|	-
합계|	44+a|	40|	성공 : 5<br>실패 : 6| 	-4 + a

획득 세부

이름|총 포인트|	포인트 세부
----|---------|----------
 케인 (FW)|	2|	60+ : 2
 칼버트 (FW)|	2|	60+ : 2
 페르난데스 (MF)|	10|	60 + : 2<br>1G : 5<br>1A : 3
 손흥민 (MF)|	3|	60+ : 2<br>CS : 1
 클리히 (MF)|	3|	60+ : 2<br>CS : 1
 제네포 (MF)|	5|	60+ : 2<br>1A : 3
 로버트슨 (DF)|	2|	60+ : 2
 칠웰 (DF)|	8|	60+ : 2<br>CS : 4
 마쉬아퀴 (DF)|	1|	60- : 1
 크레스웰 (DF)|	2|	60+ : 2
 맥카시 (DF)|	2|	60+ : 2

포인트 최다 획득 : 10 pts|포인트 최저 획득 : 1 pts
------------------------|-----------------------
페르난데스 (MF)<br>![bruno](https://resources.premierleague.com/premierleague/photos/players/110x140/p141746.png)|마쉬아퀴(DF)<br>![masuaku](https://resources.premierleague.com/premierleague/photos/players/110x140/p105717.png)

(예상)-(결과) 최대 : +5 pts|(예상)-(결과) 최저 : -4 pts
------------------------|-----------------------
페르난데스 (MF)<br>![bruno](https://resources.premierleague.com/premierleague/photos/players/110x140/p141746.png)|칼버트(FW)<br>![DCL](https://resources.premierleague.com/premierleague/photos/players/110x140/p177815.png)
