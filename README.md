# fakenews-classification

# 📚개요
## 1. 배경 및 주제
<div style="text-align: center;">
  <img src="./images/newsnetwork.png" /> <br>
</div>
SNS의 발달로 정보 전파속도가 매우 빨라 가짜뉴스의 검증이 이루어지기 전, 먼저 정보가 전파되는 문제가 발생하고 있다.
<br>
따라서, SNS상의 정보와 퍼져나가는 양상을 바탕으로 가짜뉴스와 진짜 뉴스를 판단하는 분석 모델을 만들어 가짜 뉴스를 구분한다.

<br>

**가짜뉴스 특징**
- 끊임없는 재생산
- 서로 연결되지 않은 개인에 의한 전파 / 점 조직의 네트워크 형태
    - Graph(Network) 이용
- 빠른 확산 속도
- 주로 팔로워가 적은 사람에서 많은 사람으로 전파되는 양상
    - Graph(Network) 이용
- 회피성 언어 사용 
    - Text(텍스트마이닝) 이용


## 2. 프로젝트 일정

<div style="text-align: center;">
  <img src="./images/schedule.png" /> <br>
</div>

## 3. 분석 프레임워크

<div style="text-align: center;">
  <img src="./images/framework.png" /> <br>
</div>

## 4. 데이터 설명
txt : 트위터에서 공유되는 각종 뉴스 기사들의 retweet 데이터

csv : 해당 기사의 진위여부 데이터

<div style="text-align: center;">
  <img src="./images/data.png" /> <br>
</div>
<br>

# 💻 분석 프로세스

## 1.EDA
<details><summary> <b> 테이블(CSV) 데이터 </b> </summary>
<p>
	
- 기술통계량
<div style="text-align: center;">
  <img src="./images/describe.png" /> <br>
</div>

- 결측치 <br>
<div style="text-align: center;">
  <img src="./images/na.png" /> <br>
</div>

- 막대 그래프 <br>
<div style="text-align: center;">
  <img src="./images/barchart.png" /> <br>
</div>

 <br>

</p>
</details>

<details><summary> <b> 텍스트(TXT) 데이터 </b> </summary>
<p>
	
- title 변수의 길이, 토큰 수, 토큰 길이
<div style="text-align: center;">
  <img src="./images/title_hist.png" /> <br>
</div>

- text 변수의 길이, 토큰 수, 토큰 길이  <br>
<div style="text-align: center;">
  <img src="./images/text_hist.png" /> <br>
</div>

- title, text 변수 REAL/FAKE 뉴스 품사 통계 <br>
<div style="text-align: center;">
  <img src="./images/품사통계.png" /> <br>
</div>

- 기사 본문 품사가 동사, 형용사, 부사인 빈도 수 상위 50개 단어 <br>
<div style="text-align: center;">
  <img src="./images/기사품사.png" /> <br>
</div>

 <br>
  
</p>
</details>

## 2.데이터 전처리
- eda를 근거로 불필요한 변수를 제거 및 유의미한 새로운 변수 생성
<div style="text-align: center;">
  <img src="./images/전처리.png" /> <br>
</div>

<details><summary> <b> 기존 변수 이용한 파생 변수 생성 </b> </summary>
<p>

- author, images, source 세가지 변수 이용하여 파생 변수 생성
<div style="text-align: center;">
  <img src="./images/파생변수.png" /> <br>
</div>
	
<div style="text-align: center;">
  <img src="./images/작가변수.png" /> <br>
</div>

- author 개인의 fake/real news 작성 횟수를 고려해 점수 부여
(author의 신뢰도)

- 각 뉴스별 author 신뢰도의 평균 
→ author score 변수 생성

- images, source 또한 같은 방식으로 진행
 <br>

</p>
</details>

<details><summary> <b> 네트워크를 이용한 파생 변수 생성 </b> </summary>
<p>
	
- 팔로우-팔로워 네트워크 이용하여 파생 변수 생성
<div style="text-align: center;">
  <img src="./images/네트워크파생변수.png" /> <br>
</div>


<div style="text-align: center;">
  <img src="./images/네트워크변수.png" /> <br>
</div>

- 팔로워가 적은 사람에서 많은 사람으로 전파되는 경우가 많음
    - → 뉴스를 리트윗 하는 유저의 in-degree와 out-degree를 비교

- 서로 연결되지 않은 개인에 의해 전파, 점 조직 형태의 네트워크를 가짐
    - → 뉴스를 리트윗 하는 유저의 clustering-coefficient를 비교

- 가짜 뉴스를 리트윗 하는 유저들은 가짜 뉴스를 전파하기 위함
    - → 뉴스를 리트윗 한 유저들의 진짜 뉴스, 가짜 뉴스 수 비교
 <br>

 <br>
  
</p>
</details>