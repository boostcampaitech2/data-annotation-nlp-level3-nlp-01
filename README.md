# 프로젝트 개요

> 실제 머신러닝에서 중요한 파트인 NLP 데이터제작의 흐름을 이해하고 풀고자하는 형태에 맞게 데이터를 제작하는 프로젝트입니다.
> 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b08de381-f37b-4212-970b-d2cabf1bebe8/Untitled.png)

- 해당 프로젝트에서는 **RE(Relation Entity) 데이터셋**을 제작하게 되었습니다.
- 저희는 **'마블'** **위키피디아 데이터**를 주제로 RE Task를 진행하였습니다.
- 데이터 셋을 제작하는 전체 과정을 경험하고 어떠한 어려움이 있는지 직접 체험하는 것이 목적입니다.
- 데이터 셋 제작을 완료하고 모델을 훈련시켜 직접 평가합니다.

# 프로젝트 진행과정

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/adefc2e0-68c5-406b-a1c7-f79df65a6f4d/Untitled.png)

## 데이터 획득, 정제

### 원시 **데이터 수집**

**원시데이터 수집**

1. https://github.com/paul-hyun/web-crawler 를 이용하여 위키피디아의 마블 데이터만을 선별
2. 표제어와 서브 아티클들을 선별하여 위키피디아 문서 내의 본문 텍스트 수집

### 데이터 전처리 & 문장 분리

**코드를 통한 전처리** 

1. kss를 활용해 문서를 문장으로 1차 분리
2. 분리된 문장의 끝이 '다.'로 끝나는 경우와 그렇지 않은 경우로 분류
3. 문장의 길이가 15 이하인 경우 제외
4. Pororo 라이브러리의 ner 태깅 모델을 활용해 문장 별 개체 종류와 개수 확인
5. 'Person'과 'Organization' 개체를 subject로 그외 개체를 object로 가정하고 subject와 object를 포함한 경우 활용 가능한 문장으로 인식

**파일럿을 통한 전처리**

1. 약 95% 작업을 코드를 통해 처리하고, 나머지 작업을 진행
2. 다. 로 끝나지 않아도 '다!', '다' 등과 같이 끝나는 문장을 다시 포함시킴
3. 분류한 문장중 길이가 너무 긴경우 (100자 이상) 문장으로 분리시키는 작업을 추가

## 데이터 주석

### Relation Map

**영화 제작**과 관련된 관계과 **영화 스토리**와 관련된 관계를 중심으로 추출하였습니다.

**'마블'**이라는 주제에 맞게 **Relation**(인물간 친족, 우호적 관계, 대립 관계, 사망 원인 등)**추출**을 진행하여 아래와 같이 스프레드 시트에 정리하였습니다. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7d97c7fe-b4eb-4324-9f74-c0d0f6f4752a/Untitled.png)

- [Relation Map 링크](https://docs.google.com/spreadsheets/d/1qT36nWvTIGjNksaHCSqYIKk2Il0rdxVmjSKUlIYiOwc/edit#gid=0)

### Guildline 작성

![https://i.imgur.com/mASzX17.png](https://i.imgur.com/mASzX17.png)

- KLUE Guidline을 참고하여 Relation Map을 토대로 아래와 같이 가이드라인을 작성하였습니다.
- 각 팀원은 해당 가이드라인을 토대로 태그 작업을 진행합니다.
- [Guildline 링크](https://docs.google.com/document/d/1QyL3q1x-V0-NcNqrhRDzpUmKLi5JY0VC/edit?usp=sharing&ouid=117348035096347046571&rtpof=true&sd=true)

### Tagging

**Tagtog**을 이용하여 데이터 태깅을 진행한 후 아래와 같이 스프레드 시트에 정리하였습니다. 

![https://i.imgur.com/OP2f9Mj.png](https://i.imgur.com/OP2f9Mj.png)

- 태그 작업을 진행하기 전, 원시데이터를 문장 단위로 나누고, 너무 긴 문장이나 짧은 문장을 먼저 추려냅니다.
- 추려낸 문장은 앞뒤 문장을 통해 필요없는 단어를 빼거나, 필요한 단어를 붙여주어 새로운 문장으로 생성하였습니다.
- [문장 Sheet 링크](https://docs.google.com/spreadsheets/d/1Us5BopmTVjQymheFrQt6Wos6x4FLQXnt/edit?usp=sharing&ouid=103199421593842133648&rtpof=true&sd=true)
- 전처리를 완료한 문장을 Tagtog에 업로드하여 작업을 진행하였습니다.
- 각자 250문장가량을 작업하고, 작업이 완료된 후 해당파일을 추출하여 xlsx 파일로 변환하였습니다.

![https://i.imgur.com/kPPY6f3.png](https://i.imgur.com/kPPY6f3.png)

![https://i.imgur.com/HckxTqn.png](https://i.imgur.com/HckxTqn.png)

![https://i.imgur.com/Y8iMFKN.png](https://i.imgur.com/Y8iMFKN.png)

- 변환된 xlsx 파일을 구글 스프레드 시트에 업로드 하고, 태깅이 잘 됐는지 검토합니다.
- 각자 1800 문장을 전부 검토하였으며, hard voting을 통해 라벨 작업을 새로 합니다.
- 논점이 있는 문장은 토론을 통해 합의하거나 아에 제외시켰습니다.
- [Validation 링크](https://docs.google.com/spreadsheets/d/1O_ylWxr28Gz8BwOs7H1_ZVrT9OqysZjztGBtasU8SwQ/edit#gid=1012725742)

## 데이터 평가

### 데이터 검수

- IAA로 **Fleiss Kappa 지표** 사용
    - *#raters =  4 , #subjects =  1846 , #categories =  13*
    - *PA =  0.9117009750812552*
    - *PE = 0.21525132229328975*
    - *Fleiss' Kappa = 0.887*
- 관계 레이블의 분포 / 문장 길이 분포 / Subj , Obj Entity 단어의 분포 / 문장 내 Entity 위치 분포의 적절성 평가

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e757525d-fc0b-4ead-af2b-93e40dd32fa8/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a1f02628-a1dd-48be-b9b8-d9f49697cc1e/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e2ba2d6-008e-4ded-980f-01e98ea8e2da/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cdb72ec6-2721-4ecd-b46f-84cb495a4ac8/Untitled.png)

- 구축한 데이터를 Train / Test 셋으로 적절히 분배 후 간단한 모델의 학습 이후 분류 성능을 확인

### 모델 튜닝 및 성능 측정

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/486c0ce5-b447-4f84-a405-a480b2598a7e/Untitled.png)

- *best eval micro f1 score:  86.2069*
- *bset eval loss:  0.9085*
- *best eval accuracy:  0.8689*

# 회고록

### **잘했던 점/새로 알게된 점**

- 태깅 과정에서의 이슈 기록 페이지 작성 및 의견 공유하였습니다.
- Relation Map 작성 이전에 미리 파일럿 태깅 해보면서 Relation 재정의하였습니다.
- 문장 분리와 Relation Map 작성을 위한 파일럿 태깅을 팀으로 분할해서 작업하였습니다.

### **아쉬웠던 점/개선해야할 점**

- 태깅 검토를 할 때 옆 사람의 작업이 보여서 어느정도 검토하는데 영향이 있던거 같습니다. 이에 서로 작업이 안보이도록 하는 것을 고려해야겠습니다.
- 태깅 작업시, 테스트 데이터를 추출하는 작업을 병행하는게 좋다.
- 작업량이 많아서 작업 과정에서 태깅을 하는 기준이 점차 변하는 문제
