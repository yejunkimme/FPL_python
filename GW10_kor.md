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
> points return: 42  
> GW10 rating: 4.2M 

## 프로세스: 선발 과정

### 수비수

우선, FPL 전체 수비수들 중에서 엘리트 그룹을 선발하기 위해 다음과 같은 필터링을 거쳤다.

```
fpl_defenders = fpl_defenders.loc[(mrg_mid["90s"] > 4)]
fpl_defenders = fpl_defenders.loc[(mrg_mid["SCA90"] > 1.5)]
```

우리의 첫 번째 가설은 SCA(Shot-Creating Actions, 슈팅 창출 행위: 슛팅으로 연결되는 직전 두 개의 동작으로, 패스, 드리블, 파울 유도 등이 포함된다. 예컨대, 선수A가 드리블 성공 후 선수B에게 패스해 선수B가 슛팅을 시도했다면, 선수A는 두 개의 SCA를 기록한다.)가 공격포인트 생산력의 지표라고 보았다. 

![SCA+total_points](https://user-images.githubusercontent.com/51032518/100701927-f5d67f00-33e3-11eb-8b24-e3690846510a.png)  

![SCA_error_rate](https://user-images.githubusercontent.com/51032518/100701928-f707ac00-33e3-11eb-9c07-52a7f0d94ca0.png)  


