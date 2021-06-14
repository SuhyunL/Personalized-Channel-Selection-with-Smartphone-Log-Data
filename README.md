# **개인화 광고 채널 선택을 위한**<br/>**데이터 분석 및 딥러닝 모델 제안**

## **🔸 Introduction**

우리가 손에 쥐고 놓지 않는 것 한가지, 바로 스마트폰.

![image](https://user-images.githubusercontent.com/75061420/121826819-bd99ec80-ccf4-11eb-984a-ffddc50f7bf7.png)

스마트폰은 가장 보편적으로 사람들이 일상적에서 소지하는 기기입니다. 이같은 일상성은 스마트폰을 가장 효과적인 광고 매체로 부상하도록 했죠. 디지털, 특히 모바일 광고는 가파르게 성장하고 있으며, 매체별 광고비도 점점 비싸지는 중입니다.
![image](https://user-images.githubusercontent.com/75061420/121826830-c68abe00-ccf4-11eb-936d-3fbef3ae955e.png)
점점 중요해지고, 또 돈도 많이 써야하는 광고인 만큼 기업 입장에선 더욱 효과적으로 광고하고 싶겠죠. 이에 기존의 광고판도 기존의 소비자 세그멘테이션을 통한 세분화에서 한발 더 나아가서, 개인화를 향해 나아가고 있습니다. 
![image](https://user-images.githubusercontent.com/75061420/121826848-d6a29d80-ccf4-11eb-8a22-0e0eab8d31e4.png)
실시간으로 변화하는 개개인의 니즈에 부합하는 리얼 타임, 맞춤형 콘텐츠를 제공하겠다는 건데요. 맞춤형 콘텐츠를 제공해서 효과가 있다면, 그 비싼 광고비가 아깝진 않을 겁니다. 다만 광고는 사실, 콘텐츠만 있어서 되는게 아니잖아요. 그 광고를 담는 채널이 있어야 존재합니다. 콘텐츠가 중요한 만큼, 채널도 중요합니다.
그래서 저는,
**개인화된 채널 추천을 해보려고 합니다.**

## **🔸 Goals**

개인화된 채널 추천을 하려면 무엇을 해야 할까요?
#### 1. 스마트폰 사용의 General Pattern과 Individual Pattern을 확인하기
![image](https://user-images.githubusercontent.com/75061420/121826878-02258800-ccf5-11eb-9181-5d09e7383472.png)

> 개인화는 개개인의 행동을 근거로 행해집니다.

> 각자의 행동은 사회로부터 비롯된 보편적인 패턴과 개인적 특성을 바탕으로 한 개별적인 패턴으로 구성되어 있습니다. 개인화는 이 개별적인 특성에 보다 초점을 맞추는 것입니다. 다만 개별적 특성을 판단하기 위해서는 보편적인 행동 패턴에 대한 선행적인 분석이 이루어져야 합니다. 알아야 이게 특별한 행동인지, 아닌지 분간할 수 있다는 것이죠.

#### 2. Individual Pattern을 활용해 Personalized Channel Recommendation 모델 만들기
> 소비자의 이전 어플리케이션 사용 패턴을 바탕으로, 추후 어떠한 어플을 사용할 지 예측할 수 있다면 보다 개인화된 광고 채널을 선택할 수 있을 것입니다. 

## **🔸 DATA OVERVIEW**
- 스마트폰 앱/웹 로그 데이터셋
- Android 6.0.0 이상을 대상으로 UsageStat API를 활용해 수집된 앱 사용 내역
- 31명의 피험자를 대상으로 2018.10월-11월 중 약 3주간 수집됨, 약 158만 건
- json형태로 타임스탬프, 기기, 패키지명, 사용어플명, 타입(포그라운드, 백그라운드) 등에 대한 내용을 담고 있음
- https://aihub.or.kr/keti_data_board/rt_other

## **🔸 Part1. General Pattern**
#### 하루에 핸드폰을 깨우는 횟수:
- 215회

#### 각 사용 행위 별 이용시간
- 평균 5분이지만,
- 데이터 간 편차가 굉장히 큼 (표준편차가 무려 14분)
- 누적그래프를 그려보면 4분 이하의 기록들이 전체의 75% 이상을 차지함
- 바이올린 플롯에서 대부분의 사람들의 평균이 거의 0에 가까움, 짧은 이용량들이 대다수를 차지함

#### 가장 인기있는 어플리케이션
- 빈도 기준: <br/>네이버, 크롬, 인터넷 등 인터넷 브라우저, 메시지, 전화 등 연락 어플, 기본홈, 소셜 어플리케이션, 유튜브, 멜론 등. 일상에서 필수적인 어플리케이션이 가장 많이 사용되는 것으로 나타남
- 평균 사용시간 기준: <br/> 게임이 다수, 이외 음악, 주식 어플리케이션도 긴 사용시간을 보임. <br/>평균 사용 시간이 긴 어플들은 주로 개인의 취미, 취향과 관련되는 듯.

#### 어플 간 이동 경향성
- 핸드폰을 깨우고 카톡을 켜거나, 웹서핑을 하거나, 인스타그램을 하거나.
- 핸드폰을 열었을 때 처음으로 켜는 어플 1위? 카카오톡.
- 핸드폰을 열고 카카오톡을 열거나, 카카오톡을 닫고 핸드폰을 끄는 등의 기본 홈 <-> 카카오톡 간의 이동이 가장 빈번하게 나타남
- 이외에도 허니스크린, 모어락, 시스템 UI등 다른 종류의 기본홈이나 페이스북, 웹 브라우저 등 다른 어플에서 카카오톡으로 이동하는 현상도 빈번하게 나타남
- 다른 어플 사용 중 연락을 위해 카카오톡으로 이동한 것으로 보임
- 기본 홈에서 브라우저로의 이동도 두번째로 많이 관찰됨.
- 네이버 이외에도 크롬, 인터넷 등 다른 웹서핑 브라우저로의 이동 또한 빈번.
- 대세 SNS답게 인스타그램은 카카오톡, 네이버 다음으로 핸드폰을 열었을 때 가장 빈번히 클릭하는 어플
- 
#### 어플리케이션 장르 별 시간에 따른 사용량 [빈도별, 총 시간별]
- 연락용 어플리케이션: <br/>오전 8시경부터 사용량이 급상승 <br/>(업무 시간대에 다양한 사람들과 다양한 이유로 연락해야 하기 때문에, 사용량이 늘어나는 것으로 보임)
- 소셜 어플리케이션: <br/>들쭉날쭉한 이용량 - 사용하지 않을 때에는 0에 가깝지만, 한번 사용하면 이용량이 많아짐 <br/>(한번 시작하면 좀 오래 했다가, 다시 한동안 건드리지 않고, 이런 식으로 사용하기 때문인 것으로 보임)
- 자기관리 어플리케이션: 일정하면서도 꾸준한 사용량
- Hobby, Lifestyle, Game 등 Entertainment: <br/>어플 사용량은 시간대와는 관련성이 떨어짐, 시간과 관련없이 사용량이 일정 <br/>(개인마다 취향이 갈리는 부분인데, 전체적인 평균을 내다보니 개별적인 특징이 묻힌 것으로 보임)
- #### 스마트폰 이용 시간대 분포
- 전체 사용자의 이용 시간을 보면, 저녁 시간-새벽 시간대에 사용량이 늘어나는 것을 제외하고는 큰 경향성을 찾기 어려움.
- 언제 핸드폰을 많이 하는지는 사용자 마다 다른 것으로 보임.
- 다만 사용자마다 각자가 휴대폰을 자주, 오랫동안 사용하는 시간대는 분명히 있는 것으로 보임
- 사용자별 휴대폰 사용 시간대 scatter plot을 보면, (x-요일, y-시간대, size = 사용 시간), 네모난 형태로 그래프가 나타남 - 요일이 달라져도, 일정한 시간에 핸드폰을 사용
- 또한 사용자별 scatter plot에서는 각 시간대마다 비슷한 사이즈의 점이 나타난다- 매일 비슷한 시간에, 비슷한 정도로 핸드폰을 사용

## **🔸 Part2. Individual Pattern**

인간의 행위: 보편적인 행동 패턴 + 개인적인 행동 패턴으로 구성<br/>
개인 나름의 특징적인 패턴을 알아내기 위해선, 개인의 행동들 중 주목할만한 부분 만 뽑아내야 함<br/>
이 특징적인 행동 패턴을 어떻게 판단할까?<br/>

**‘특징적인 행동 패턴’ 판단의 기준?**

밀리의 서재의 '취향지수'를 참고해 다음의 두 기준을 설정

![image](https://user-images.githubusercontent.com/75061420/121827011-8d9f1900-ccf5-11eb-8285-4b4250f92f85.png)

1. 이 취향이 나만의 것인가?
- '중요도' 스코어- 특정한 어플리케이션을 각 개인들이 얼마나 자주 사용 하는지의 비율을, 다른 사람들도 얼마나 자주 하는지에 대한 비율로 나눈 값
- ex) 예를 들어, 내가 카카오톡을 자주 한다고 하더라도, 남도 비슷한 비율로 카카오톡을 사용한다면 중요도가 높지 않을 것
- ex2) 특정 개인이 비인기 어플리케이션을 매우 자주 사용한다면 중요도가 매우 크게 나타날 것

2. 해당 행위에 충분한 시간을 투자하는가
- 본 프로젝트의 궁극적인 목적은 개인의 취향 파악을 통한 효율적인 광고 채널 추천에 있음
- 이런 목적을 감안한다면, 개인들이 산발적으로, 짧게 이용하는 어플리케이션들은 광고 효과가 떨어지기에 큰 의미가 없기에 추가한 지표

위 두가지 기준을 바탕으로<br/>
특정한 행동이 보편적인 것이 아니라, 개인화 채널 추천에 참고할만한 취향을 담는 행동인지를 판단하는 threshold를 설정<br/>
이를 바탕으로 행동을 4가지 그룹으로 분류

![image](https://user-images.githubusercontent.com/75061420/121827203-16b65000-ccf6-11eb-8bbe-5bbfb1bebb49.png)

#### 개인의 스마트폰 이용 행위에서 각 행동 그룹들이 차지하는 비율
- 행동 그룹 1 (개인만의 특성도 아니되, 이용 시간도 짧은 행위)이 약 70%로 대다수를 차지
- 다만 독특했던 것은 그룹 1을 제외한 나머지 시간들에서, 행위 2나 3의 비율은 많지 않고, 개인의 취향이 반영된 행위 4의 비율이 가장 많았다는 것
- 개개인이 스마트폰 이용 시간에서 꽤 큰 비율을 자신만의 취향, 취미에 맞게 사용하고 있다는 것을 추론해 볼 수 있음
![image](https://user-images.githubusercontent.com/75061420/121827316-8298b880-ccf6-11eb-9acb-76a0632eb421.png)

#### 개인의 시간대에 따른 어플별, 장르별, 대장르별 이용량을 행동 그룹에 따라 관찰
- 그래프를 전체적으로 바라보면, 핑크색 점(행동그룹4, 개인의 취향이 반영된 행위)가 비슷한 어플리케이션, 비슷한 장르, 비슷한 시간대에 뭉쳐있다는 것을 확인할 수 있음
- 개인의 취미, 혹은 라이프스타일을 반영한 만큼, 비슷한 위치에, 비슷한 시간대에, 비슷한 어플을 사용하는 식으로, 한데 뭉쳐 나타난 것
![image](https://user-images.githubusercontent.com/75061420/121827344-99d7a600-ccf6-11eb-9b0f-090bee78a28a.png)

## **🔸 PART 3. ML Model**

### LGBM 분류기
각 사용자별 선호 + 어플리케이션 사용 패턴을 바탕으로 사용자가 다음에 사용할 어플리케이션 ‘대장르’를 예측하도록 함
- Train:
    - 전체 데이터셋의 80%, 약 119432개의 data
    - Feature: 'pid','name','is_system_app','time','genre','referrer_package','referrer_app','referrer_genre','total_sec','importance_cat'
    - Target: 'genre_bigcol'
  
- Validation:
    - 시계열 데이터로 이전 데이터들을 train시켜 이후의 데이터를 예측하도록 했기에 cross validation이 불가,
    - 전체의 0.2만큼을 떼어내어 hold out validation으로 진행
    
- Result:
    - Weighted Average 0.27
    - p_id별로 나누어 따로따로 모델을 돌리면 정확도가 0.40 정도까지 올라가는 것으로 보아 모델 자체에서 pid를 구분해 개인의 취향과 패턴을 제대로 익히지는 못하는 것으로 보임.

### XGBRegressor
각 사용자별 선호 + 어플리케이션 사용 패턴을 바탕으로 사용자의 특정 어플리케이션 이용 시간을 예측해봄
- Train:
    - 전체 데이터셋의 80%, 약 119432개의 data
    - Feature: 'pid','name','is_system_app','time','genre','genre_bigcol','referrer_package','referrer_app','referrer_genre','importance_cat'
    - Target: 'total_sec'
  
- Validation:
    - 시계열 데이터로 이전 데이터들을 train시켜 이후의 데이터를 예측하도록 했기에 cross validation이 불가,
    - 전체의 0.2만큼을 떼어내어 hold out validation으로 진행
    
- Result:
    - 오차범위 6~7.5% 사이를 기록하며 꽤 좋은 성능을 보임


## **🔸 PART 4. DL Model**

### Deep Learning Model

- 목적: 각 소비자별 선호 + 어플리케이션 사용 패턴을 바탕으로 각 소비자가 다음에 사용할 ‘어플리케이션 명’를 예측하는 것.<br/>(딥러닝 모델의 경우 대장르 예측은 물론이고, 전반적으로 압도적으로 뛰어난 성능을 보였기 때문에 보다 어려운 태스크를 요구해 봄)

- Baseline Model Accuracy: 1 / 693 *100 = 0.001 (어플리케이션은 모두 693개)

- TRAIN:    
    - Feature: 개인ID, 사용 어플리케이션명, 어플리케이션 장르, 행위 그룹(1,2,3,4), 시간, 사용 시간대, Referrer(현재 어플리케이션으로 이동하기 전에 사용한) 어플리케이션명,  Referrer 장르, Referrer 패키지
    - Target: 사용 어플리케이션명

- MODEL: 훈련 데이터를 벡터화, 이를 임베딩 층에 가중치로 주입하고, LSTM 사용.

- TEST:
- 미래 사용할 어플리케이션을 예측할 때에는 어플리케이션을 알 지 못하므로 그 행위가 어떤 그룹에 속할지를 알 수는 없음.
- 이에, 관심도를 주지 않은 상태로 예측을 시켜 봄. (딥러닝 신경망 모델에서는 인풋 Shape가 모델이 구성된 Shape와 정확히 일치하지 않아도 예측이 가능했었음)

- Result:
    - weighted average 0.75
    - 갯수가 적은 클래스에 대해서는 예측력이 떨어지나, 충분한 기록이 있는 클래스에 대해서는 좋은 성능을 보임
    - 테스트 셋에서 각 행위가 해당하는 중요도 그룹을 말해주지 않더라도, 이미 모델은 훈련과정에서 개인들의 취향, 패턴을 파악했기에 다음과 같은 좋은 accuracy를 낼 수 있었던 것으로 보임

![image](https://user-images.githubusercontent.com/75061420/121827445-f5a22f00-ccf6-11eb-8cb0-fbae2b9bc90b.png)


