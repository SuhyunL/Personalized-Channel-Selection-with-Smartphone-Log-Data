# **개인화 광고 채널 선택을 위한**<br/>**데이터 분석 및 딥러닝 모델 제안**

## **🔸 Introduction**

우리가 손에 쥐고 놓지 않는 것 한가지, 바로 스마트폰.

![image.png](attachment:48c6d0e5-2f8d-4ef7-97d0-3589d13bd175.png)

스마트폰은 가장 보편적으로 사람들이 일상적에서 소지하는 기기입니다. 이같은 일상성은 스마트폰을 가장 효과적인 광고 매체로 부상하도록 했죠. 디지털, 특히 모바일 광고는 가파르게 성장하고 있으며, 매체별 광고비도 점점 비싸지는 중입니다.

![image.png](attachment:036df242-f384-46d4-9a96-5976a93ab6fa.png)

점점 중요해지고, 또 돈도 많이 써야하는 광고인 만큼 기업 입장에선 더욱 효과적으로 광고하고 싶겠죠. 이에 기존의 광고판도 기존의 소비자 세그멘테이션을 통한 세분화에서 한발 더 나아가서, 개인화를 향해 나아가고 있습니다. 

![image.png](attachment:3a086e86-305d-426e-bd36-23112ee84f90.png)

실시간으로 변화하는 개개인의 니즈에 부합하는 리얼 타임, 맞춤형 콘텐츠를 제공하겠다는 건데요. 맞춤형 콘텐츠를 제공해서 효과가 있다면, 그 비싼 광고비가 아깝진 않을 겁니다. 다만 광고는 사실, 콘텐츠만 있어서 되는게 아니잖아요. 그 광고를 담는 채널이 있어야 존재합니다. 콘텐츠가 중요한 만큼, 채널도 중요합니다.
그래서 저는,
**개인화된 채널 추천을 해보려고 합니다.**

![image.png](attachment:04c12bb6-0cfe-4924-a646-b97255d2d87b.png)

## **🔸 Goals**

개인화된 채널 추천을 하려면 무엇을 해야 할까요?
#### 1. 스마트폰 사용의 General Pattern과 Individual Pattern을 확인하기
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
