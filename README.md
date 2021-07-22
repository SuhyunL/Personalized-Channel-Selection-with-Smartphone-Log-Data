# **개인화 광고 채널 선택을 위한**<br/>**데이터 분석 및 딥러닝 모델 제안**

## **🔸 Introduction**

우리가 손에 쥐고 놓지 않는 것 한가지가 있습니다.
바로 스마트폰입니다.

![image](https://user-images.githubusercontent.com/75061420/121826819-bd99ec80-ccf4-11eb-984a-ffddc50f7bf7.png)

스마트폰은 가장 보편적으로 사람들이 일상적에서 소지하는 기기입니다. 이같은 일상성은 스마트폰을 가장 효과적인 광고 매체로 부상하도록 했죠. 디지털, 특히 모바일 광고는 가파르게 성장하고 있으며, 매체별 광고비도 점점 비싸지는 중입니다.

![image](https://user-images.githubusercontent.com/75061420/121826830-c68abe00-ccf4-11eb-936d-3fbef3ae955e.png)

더불어 스마트폰은 웹로그라는 자취를 남기는데요, 웹로그 데이터는 개인들의 일상과 관심사에 대한 긴밀한 추적이 가능하도록 합니다. 웹로그 데이터를 활용한 개인화된 마케팅에 대한 니즈와 기대감 또한 부상하고 있습니다.

![image](https://user-images.githubusercontent.com/75061420/121826848-d6a29d80-ccf4-11eb-8a22-0e0eab8d31e4.png)
(출처: 이화여대 브랜드커뮤니케이션 수업 자료)

다만 이 개인화 마케팅은 '콘텐츠'에 집중이 된 양상을 띱니다. 고객의 니즈에 걸맞는 내용, 즉 콘텐츠를 제공하겠다는 것이죠.

![image](https://user-images.githubusercontent.com/75061420/126421526-10c290da-21b4-4729-a0cb-330cb530b317.png)
(출처: 대홍기획 2020 트렌드 레포트)

최근에는 개인화에서 한발 더 나아가서, 콘텐츠가 제시되는 상황과 맥락까지 개인화한 '초개인화' 마케팅이 주목받고 있습니다. 모바일 Ad/Pr 콘텐츠가 제시되는 상황과 맥락은 사용자의 이용 어플리케이션, 즉 채널에 달려 있습니다. 이젠 단순한 컨텐츠 개인화가 아닌, 채널의 개인화까지 초점을 맞춘다는 것입니다.


## <br/>**🔸 Goals**

초개인화라는 시대적 요구사항을 고려하여, 이번 프로젝트의 목표는 다음과 같습니다.

**1. 채널 개인화를 위한 웹로그 데이터 분석 방법 제안 <br/> 2. 방법론을 바탕으로 한 머신러닝 / 딥러닝 광고 어플리케이션(채널) 추천 모델 구성**

막대한 양의 웹로그 데이터에서 개인들의 핵심적인 특징만을 반영하는 분석값을 얻어내는 방법론을 제시하고,
이 방법론을 바탕으로 이용자의 다음 번 스마트폰 이용에서 이용할 채널 예측 모델을 만들어 광고 채널 선택의 효율성을 높이는 것입니다.

이 시도가 성공적이라면, 카카오, 네이버, 유튜브 등 모바일 광고시장의 Big4가 아니더라도,
특정한 사용자가 사용 할/ 관심 가질 만한 채널에서 상대적으로 높은 광고 효율을 기대해볼 수 있습니다.


## <br/>**🔸 DATA OVERVIEW**
- 스마트폰 앱/웹 로그 데이터셋
- Android 6.0.0 이상을 대상으로 UsageStat API를 활용해 수집된 앱 사용 내역
- 31명의 피험자를 대상으로 2018.10월-11월 중 약 3주간 수집됨, 약 158만 건
- json형태로 타임스탬프, 기기, 패키지명, 사용어플명, 타입(포그라운드, 백그라운드) 등에 대한 내용을 담고 있음
- https://aihub.or.kr/keti_data_board/rt_other


## <br/>**🔸 Data Preprocessing & Basic EDA**
- Json형태의 데이터를 Dataframe화
- NaN값 처리, nunique, unique values 확인
- 중복 기록된 기록들 제거
- 사용자 식별이 간편하도록 pid 라벨 인코딩 (pid값을 0부터 시작하는 정수로 바꾸어주는 작업)
- 사용자의 실질적인 스마트폰 이용 기록만 남기기 (백그라운드 활동은 삭제, ForeGround, User Interaction 작업만 남기는 것)
- 비정상적인 사용 기록이 관측되는 (관측 오류로 보임) 이상치 제거하기
- 기존 특성들을 이용한 새로운 특성 생성
> **Genre :**<br/>
셀레니움을 기반으로 한 커스텀 크롤러 생성, 크롤러가 자동으로 약 800개 가량의 어플리케이션 명을 검색하고 장르값을 추출하도록 함<br/>

> **Genre_bigcol  :**<br/>
세분화된 장르들의 대장르<br/>

> **Referrer_name, Referrer_genre, Referrer_package :**<br/>
사용자가 특정 어플리케이션으로 이동하기 이전에 사용한 어플리케이션의 이름, 장르, 패키지를 보여주는 특성<br/>

> **Stay_time :**<br/>
스마트폰 이용 행위 별 개인이 머무른 시간<br/>

> **Total_sec :**<br/>
연산상의 편의를 위해 Stay_time을 초단위로 바꾼 값


## <br/>**🔸 Part1. 채널 개인화를 위한 웹로그 데이터 분석 방법 제안**

31명의 피험자를 대상으로 약 3주간 수집된 데이터는 약 158만건에 달하는 방대한 규모를 지니고 있습니다.
개인화를 위한 개개인의 특징적인 패턴을 파악하기 위해서는 스마트폰 사용의 General Pattern을 파악하고, 그와 구분되는 Individual Pattern에 집중할 수 있어야 합니다. 즉, Individual Pattern을 판단할 수 있는 방법론이 필요하다는 것입니다.

## 1) 스마트폰 사용의 General Pattern

#### 하루에 핸드폰을 깨우는 평균 횟수:
![image](https://user-images.githubusercontent.com/75061420/126584659-be5f2bfb-2b94-462f-a140-a508a853e8c3.png)
- 215회

#### 각 사용 행위 별 이용시간:
![image](https://user-images.githubusercontent.com/75061420/126584757-1a03fd53-6cf2-4218-a9ad-0f1522f35e55.png)
- 평균 5분이지만,
- 데이터 간 편차가 굉장히 큼 (표준편차가 무려 14분)
![image](https://user-images.githubusercontent.com/75061420/126584692-fae8fcec-8c7f-4e32-9829-21495d2dbce3.png)
- 누적그래프를 그려보면 4분 이하의 기록들이 전체의 75% 이상을 차지함
![image](https://user-images.githubusercontent.com/75061420/126584703-b549a235-2c36-46fc-9a1f-e5ec02ff056a.png)
- 바이올린 플롯에서 대부분의 사람들의 평균이 거의 0에 가까움, 짧은 이용량들이 대다수를 차지함

#### 가장 인기있는 어플리케이션:
![image](https://user-images.githubusercontent.com/75061420/126584851-04f4c813-fd88-4bb5-bfe0-d58c347bcd3f.png)

- 빈도 기준: <br/>네이버, 크롬, 인터넷 등 인터넷 브라우저, 메시지, 전화 등 연락 어플, 기본홈, 소셜 어플리케이션, 유튜브, 멜론 등. 일상에서 필수적인 어플리케이션이 가장 많이 사용되는 것으로 나타남
- 평균 사용시간 기준: <br/> 게임이 다수, 이외 음악, 주식 어플리케이션도 긴 사용시간을 보임. <br/>평균 사용 시간이 긴 어플들은 주로 개인의 취미, 취향과 관련되는 듯.

#### 어플 간 이동 경향성:
![image](https://user-images.githubusercontent.com/75061420/126585295-fec99370-975e-4ee0-b4be-495fa2a0da55.png)
- 핸드폰을 깨우고 빈번히 나타나는 이동은 카톡을 켜거나, 웹서핑을 하거나, 인스타그램을 하는 것이었음.
- 핸드폰을 열었을 때 처음으로 켜는 어플 1위? 카카오톡.
- 핸드폰을 열고 카카오톡을 열거나, 카카오톡을 닫고 핸드폰을 끄는 등의 기본 홈 <-> 카카오톡 간의 이동이 가장 빈번하게 나타남
- 이외에도 허니스크린, 모어락, 시스템 UI등 다른 종류의 기본홈이나 페이스북, 웹 브라우저 등 다른 어플에서 카카오톡으로 이동하는 현상도 빈번하게 나타남
- 다른 어플 사용 중 연락을 위해 카카오톡으로 이동한 것으로 보임.
- 기본 홈에서 브라우저로의 이동도 두번째로 많이 관찰됨.
- 네이버 이외에도 크롬, 인터넷 등 다른 웹서핑 브라우저로의 이동 또한 빈번.
- 대세 SNS답게 인스타그램은 카카오톡, 네이버 다음으로 핸드폰을 열었을 때 가장 빈번히 클릭하는 어플

#### 어플리케이션 장르 별 이동 패턴:
(referrer이라는 개념을 차용, 이전에 사용한 어플리케이션의 장르가 다음 사용을 위해 이동할 어플리케이션의 장르와 연관있지 않을까?)<br/>
(ex - 네비게이션을 켜면, 음악을 틀기 위해 음악 장르의 어플리케이션으로 이동할 것이다.)
![image](https://user-images.githubusercontent.com/75061420/126586679-f56e01e9-b79e-4361-af9f-72d7de8a9f48.png)
- Personalization(기본홈) 장르에서 커뮤니케이션 장르로의 이동 (휴대폰의 본질적인 존재 이유는 연락, 연락을 위해 휴대폰을 깨우고 커뮤니케이션 어플로 이동하는 패턴)
- 커뮤니케이션 장르의 어플리케이션에서 커뮤니케이션 장르의 어플로의 이동 (사람들이 한가지 연락 수단만 지닌 것이 아니라, 메시지, 카카오톡, 페이스북 메시지 등 다양한 커뮤니케이션 어플들을 오가며 소통하는 패턴)
- 지도 장르의 어플리케이션에서 음악 장르 어플리케이션으로의 이동 (운전을 준비하며 네비게이션을 연결하고, 음악을 트는 패턴)

#### 어플리케이션 장르 별 시간에 따른 사용량 [빈도별, 총 시간별]
![image](https://user-images.githubusercontent.com/75061420/126586117-82fb41bc-765a-481b-923e-3c4a8ab51704.png)

- 연락용 어플리케이션: <br/>오전 8시경부터 사용량이 급상승 <br/>(업무 시간대에 다양한 사람들과 다양한 이유로 연락해야 하기 때문에, 사용량이 늘어나는 것으로 보임)
- 소셜 어플리케이션: <br/>들쭉날쭉한 이용량 - 사용하지 않을 때에는 0에 가깝지만, 한번 사용하면 이용량이 많아짐 <br/>(한번 시작하면 좀 오래 했다가, 다시 한동안 건드리지 않고, 이런 식으로 사용하기 때문인 것으로 보임)
- 자기관리 어플리케이션: 일정하면서도 꾸준한 사용량
- Hobby, Lifestyle, Game 등 Entertainment: <br/>어플 사용량은 시간대와는 관련성이 떨어짐, 시간과 관련없이 사용량이 일정 <br/>(개인마다 취향이 갈리는 부분인데, 전체적인 평균을 내다보니 개별적인 특징이 묻힌 것으로 보임)

#### 스마트폰 이용 시간대 분포
![image](https://user-images.githubusercontent.com/75061420/126586394-ccfa3f23-dfe1-4842-90fe-43c2151eacb2.png)
- 전체 사용자의 이용 시간을 보면, 저녁 시간-새벽 시간대에 사용량이 미미하게 늘어나는 것을 제외하고는 큰 경향성을 찾기 어려움.
- 언제 핸드폰을 많이 하는지는 사용자 마다 다르기 때문인 것으로 파악됨.

![image](https://user-images.githubusercontent.com/75061420/126586546-43537ef3-3e79-47bb-9dd1-df8278f74c68.png)

- 다만 사용자마다 각자가 휴대폰을 자주, 오랫동안 사용하는 시간대는 분명히 있는 것으로 보임.
- 사용자별 휴대폰 사용 시간대 scatter plot을 보면, (x-요일, y-시간대, size = 사용 시간), 네모난 형태로 그래프가 나타남 - 요일이 달라져도, 일정한 시간에 핸드폰을 사용한다는 것을 의미함.
- 또한 사용자별 scatter plot에서는 각 시간대마다 비슷한 사이즈의 점이 나타난다- 매일 비슷한 시간에, 비슷한 정도로 핸드폰을 사용.


## **<br/> 2) 스마트폰 사용의 Individual Pattern**

특정 어플리케이션 이용이 해당 개인만의 유니크한 특징인지를 지수화한 '특징 지수'리는 지표를 만들고,
해당 지표를 사용시간과 함께 종합적으로 판단하여 특정한 어플리케이션 사용 행위가 개인화 채널 추천에에 참고할 만한 패턴인지를 분류함.

### 특징 지수:
특정한 어플리케이션 사용 행위가 한 개인만의 특징에 해당하는가를 나타내는 지표

**공식: (개인의 스마트폰 이용에서 특정 어플 이용이 차지하는 비율) / (전체 스마트폰이용에서 특정 어플 이용이 차지하는 비율)**

ex. A의 스마트폰 이용 행위 중 어플리케이션 '인스타그램' 이용 빈도가 차지하는 비율: 0.1이고,<br>
모든 사람들을 대상으로 한 스마트폰 이용 행위에서 어플리케이션 '인스타그램'의 이용 빈도가 차지하는 비율: 0.2이라면<br/>
--> A에게 있어서 인스타그램의 특징 지수= 0.5<br/>

ex2. B의 스마트폰 이용 행위 중 게임 '애니팡' 이용 빈도 비율: 0.4,<br/>
모든 사람들을 대상으로 한 스마트폰 이용 행위에서 게임 '애니팡' 이용 빈도가 차지하는 비율: 0.05라면<br/>
--> B에게 있어 애니팡의 특징 지수= 8<br/>


### **개인들의 스마트폰 사용 행위 기록들을 분류 :**

특징 지수와 사용 시간을 종합적으로 고려하여,<br/>
개인의 어플리케이션 사용 행위들을 총 개인화 채널 추천에 참고할만한 취향을 담는 행동인지에 대해 분류함.<br/>

**기준 1. 이 취향이 나만의 것인가?**

![image](https://user-images.githubusercontent.com/75061420/126592591-5aa3143d-651c-48f6-862d-233d962e9e95.png)

- 특징 지수: 특정한 어플리케이션을 각 개인들이 얼마나 자주 사용 하는지의 비율을, 다른 사람들도 얼마나 자주 하는지에 대한 비율로 나눈 값
- ex) 예를 들어, 내가 카카오톡을 자주 한다고 하더라도, 남도 비슷한 비율로 카카오톡을 사용한다면 중요도가 높지 않을 것
- ex2) 특정 개인이 비인기 어플리케이션을 매우 자주 사용한다면 중요도가 매우 크게 나타날 것<br/>

**기준 2. 해당 행위에 충분한 시간을 투자하는가**

![image](https://user-images.githubusercontent.com/75061420/126592075-01152d48-337f-4a17-9cc0-19e5be7f5b55.png)

- 본 프로젝트의 궁극적인 목적은 개인의 취향 파악을 통한 효율적인 광고 채널 추천에 있음
- 이런 목적을 감안한다면, 개인들이 산발적으로 짧게 이용하는 어플리케이션들은 광고 효과가 떨어지기에 큰 의미가 없다고 판단됨
- 개인들이 충분한 주의를 기울이며 광고 채널로서 가치가 있을법한 어플리케이션만 추리기 위해 개인별 어플리케이션 사용 시간이라는 지표 또한 추가함<br/>


**위 두 기준을 바탕으로,<br/>
특정한 행동이 보편적인 것이 아니라 개인만의 특징을 담는 행동인지, (75% 수준에서 분류, 특징 지수 4이상),<br/>
광고 채널로서 유의미한 역할을 할 수 있도록 충분한 시간과 주의를 투자하는지 (75% 수준에서 분류, 이용 시간 4분 이상)<br/>
를 분류하는 기준점(threshold) 4를 설정하고, 개인의 스마트폰 이용을 구성하는 행위들을 4가지 그룹으로 분류함**


![image](https://user-images.githubusercontent.com/75061420/126592456-049a9872-14f5-44e5-afc0-b80f9a8747b2.png)<br/>


### 개인의 스마트폰 이용 행위에서 개인의 특징적인 행동(행동 그룹 4)이 차지하는 비율:

![image](https://user-images.githubusercontent.com/75061420/126593641-2906453d-0632-4c29-bb6c-7c596d19de5b.png)
![image](https://user-images.githubusercontent.com/75061420/121827316-8298b880-ccf6-11eb-9acb-76a0632eb421.png)

- 행동 그룹 1 (개인만의 특성도 아니되, 이용 시간도 짧은 행위)이 약 70%로 대다수를 차지
- 다만 독특했던 것은 그룹 1을 제외한 나머지 시간들에서, 행위 2나 3의 비율은 많지 않고, 개인의 취향이 반영된 그룹 4의 비율이 가장 많았다는 것
- 개개인이 스마트폰 이용 시간에서 꽤 큰 비율을 자신만의 취향, 취미에 맞게 사용하고 있다는 것을 추론해 볼 수 있음

### 개인의 시간대에 따른 어플별, 장르별, 대장르별 이용량을 행동 그룹에 따라 관찰: 
![image](https://user-images.githubusercontent.com/75061420/126593641-2906453d-0632-4c29-bb6c-7c596d19de5b.png)
![image](https://user-images.githubusercontent.com/75061420/126593939-b9bbe630-fc0b-4b8f-86d9-9d6767df844a.png)

- 그래프를 전체적으로 바라보면, 핑크색 점(행동그룹4, 개인의 취향이 반영된 행위)가 비슷한 어플리케이션, 비슷한 장르, 비슷한 시간대에 뭉쳐있다는 것을 확인할 수 있음
- 개인의 취미, 혹은 라이프스타일을 반영한 만큼, 비슷한 위치에, 비슷한 시간대에, 비슷한 어플을 사용하는 식으로, 한데 뭉쳐 나타난 것


### pid1번, 10번 피험자의 예시로 살펴보는 개인별 사용 양상: 

![image](https://user-images.githubusercontent.com/75061420/126592927-9af35b9c-4804-4b50-9287-e36f83883845.png)
![image](https://user-images.githubusercontent.com/75061420/126594035-6f5862c5-0739-4d45-a405-dac651ace940.png)



## **🔸 PART 2. Individual Pattern을 활용한 Personalized Channel Recommendation 모델 만들기**

### Machine Learning Model
> 소비자의 이전 어플리케이션 사용 패턴을 바탕으로, 추후 어떠한 어플을 사용할 지 예측할 수 있다면 보다 개인화된 광고 채널을 선택할 수 있을 것입니다. 

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


## **🔸 PART 3. DL Model**

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


