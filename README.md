# **QGIS** 이용한 풋거름 질소량 분포지도 작성하기

> 김솔아 작성 (2022-07-14)


<br>
</br>

## QGIS란?
QGIS는 데이터 편집 및 분석을 제공하는 자유-오픈 데스크톶 지리정보체계 프로그램이다. 윈도우, 리눅스, 안드로이드 등 다양한 운영체제에서 사용 할 수 있으며, 개발자와 사용자들간의 커뮤니티를 통해 개발 되며 무료로 배포되고 있다.
<br>
</br>

![qgis image](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/QGIS_logo%2C_2017.svg/2560px-QGIS_logo%2C_2017.svg.png)

<br>
<br>
<br>
<br>

## 1. 팜맵 편집

[공공데이터포털 data.go.kr](https://www.data.go.kr)로 들어간다.

<img src=https://user-images.githubusercontent.com/93892724/178959452-62027419-189f-4e24-9e52-1705bb1aea78.png width="700">

팜맵을 검색하고 파일데이터에서 밑 그림과 같은 정보를 클릭한다.

![image](https://user-images.githubusercontent.com/93892724/178960413-05c8dc18-d1eb-4560-9851-f1c8bc269875.png)

<br>
<br>

주기성 과거 데이터에서 오른쪽 더보기를 클릭하면 여러 지역의 팜맵정보가 등장한다. 아래로 스크롤을 하면서 시기와 찾는 지역을 클릭해서 다운로드를 진행한다.
(zip이라 다운로드 받는데 시간이 좀 걸린다. 인내심을 갖고 기다리면 다운로드가 되어있을 것이다.)

![image](https://user-images.githubusercontent.com/93892724/178961131-3ef8aea8-64dc-4afd-98cf-d21011a22d46.png)

![image](https://user-images.githubusercontent.com/93892724/178961734-ae229c06-e482-4356-8539-a3c0a305735c.png)

<br>
<br>

다운 받은 이후
* '도별로' 분류된 팜맵 압축풀기
* 필요한 '시군' 팜맵 압축풀기
* 'shp' 확장자를 가진 파일 QGIS 에서 열기

<br>
<br>

전라북도의 팜맵을 다운로드 받은 뒤, 김제시의 팜맵만 QGIS로 불러온다.
QGIS로 불러올때에는 'shp'의 확장자를 가지고 있는 파일만을 불러오면 된다.

이후에 해당 지역(김제시) ndvi(식생지수)파일도 QGIS에 불러온다.

![image](https://user-images.githubusercontent.com/93892724/178963516-99da388d-c32f-44d6-a83e-84ab9a2e9585.png)

<br>
<br>

---

* 좌측 레이어 패널에서 '팜맵정보 SHP_전라북도_김제시 2020'을 클릭
* 영역 또는 단일클릭으로 객체선택(빨강표시)아이콘 클릭
-> SHIFT + 마우스 좌클릭 드래그를 이용하여 필요한 데이터만 선택(노란표시가 선택된것)

![image](https://user-images.githubusercontent.com/93892724/178964766-bc389efe-b499-4fe1-a561-69d5049a682f.png)

<br>
<br>

---

* 좌측 레이어 패널에서 팜맵 우클릭 -> export -> 선택한 객체를 다른 이름으로 저장 클릭
* 포맷 : 사진처럼 설정
* '...' 클릭 -> 역상을 저장할 경로와 이름 설정(이름은 영어로 설정해둘것)
* 좌표계 : 영상 좌표계와 동일한 좌표계 설정
* 확인 클릭

![image](https://user-images.githubusercontent.com/93892724/178968051-c562f949-de3c-4e68-8629-e18b4de2b9dc.png)

이렇게 되면 좌측 레이어 패널에 GJ_clip이라는 레이어가 생성된다.

<br>
<br>

---

이제 속성값 추출(구역 통계)를 해볼 것이다.
풋거름 질소량 분포지도를 작성하기 위해서는 '조사지점 식생 지상부 질소량(현장데이터)'과 '드론 영상으로 산정한 식생지수(NDVI)값'이 필요하다. 
내보내기한 팜맵을 활용하여 식생지수(NDVI)값을 추출해보자.



* 상단의 툴바에서 툴박스 아이콘 클릭
* 오른쪽 공간 처리 툴박스에 래스터 분석 클릭 -> 구역 통계 클릭
* 입력레이어 : 내보내기한 팜맵
* 래스터 레이어 : 식생지수(NDVI) 영상
* 산출 열 접두어 : ndvi_
* 계산할 통계 : '...' 클릭 -> '평균' 값만 설정
* 저장할 경로와 이름 설정 ('...' 클릭) 후 실행 클릭

![image](https://user-images.githubusercontent.com/93892724/178969521-9147b5c2-1b9b-49ec-8389-ce09953f2988.png)
-> 이렇게 되면 GJ_clip_stats 구역통계가 생성된다.

<br>
<br>

---

![image](https://user-images.githubusercontent.com/93892724/178971851-12c41a0e-84ca-41f1-b287-d19ee373415d.png)

-> 제일 우측의 속성값이 추출된 것을 확인할 수 있다.

<br>
<br>

---

* 좌측 레이어 패널에 식생지수(ndvi)를 클릭하고 상단 메뉴바 '래스터' -> '추출' -> '마스크 레이어로 래스터 자르기...' 클릭
* 입력레이어 : 식생지수(NDVI) 영상
* 마스크레이어 : 내보내기한 팜맵
* 산출 밴드에 지정한 NODATA 값을 할당 : '0'(옵션)
* 잘라낸 래스터의 범위를 마스크 레이어의 범위와 일치 '체크' ('체크' 안할 시 사각형 형태로 잘라짐)
* '...'클릭 -> 저장할 경로와 이름 설정
* '실행' 클릭
* '닫기' 클릭

![image](https://user-images.githubusercontent.com/93892724/178973551-bd0abf20-66c6-414b-b50c-85d7a54a9291.png)


래스터 자르기 결과물은 아래와 같다.
![image](https://user-images.githubusercontent.com/93892724/178973939-1702cc66-d598-40aa-8aed-de5a4e2e16e1.png)

<br>
<br>

---

질소량 회귀식을 표현식에 적용하여 질소량 분포지도를 만들기 위해서는 'SCP(Semi-Automatic Classification Plugin)'플러그인을 설치해야 한다.

* 상단 메뉴바 '플러그인' -> '플러그인 관리 및 설치...' 클릭
* 검색창에 'Semin-Automatic Classification Plugin' 검색 후 플러그인 설치 클릭

![image](https://user-images.githubusercontent.com/93892724/178974327-8ec15cf5-794c-41bb-9911-7b3380a044cf.png)

SCP 플러그인을 이용하여 질소량 분포도를 만든다.

* 좌측 상단 SCP(빨간 표시) 아이콘 클릭
* SCP창 좌측의 'Band calc' 클릭
* 'Expression'창에 질소량 회귀식(초록색 식) 입력
* 오른쪽 방향 화살표(RUN, 빨간표시) 클릭

![image](https://user-images.githubusercontent.com/93892724/178988869-f4d043e6-e196-4b86-a154-7adcc0134006.png)

-> <u>**질소량 분포지도**</u>(GJ_ndvi_clip) 완성
<br>
<br>

---

* 좌측 레이어 패널에 내보내기한 팜맵을 클릭
* 상단의 툴바에서 툴박스(톱니바퀴) 아이콘 클릭
* 래스터 분석 클릭 -> 구역 통계 클릭
* 입력레이어 : 내보내기한 팜맵
* 래스터 레이어 : '질소량 분포지도' 영상
* 산출 열 접두어 : ND_
* 계산할 통계 : "..." 클릭
* 계산할 통계 설정 -> '평균'값만 설정
* '...' 클릭 -> 저장할 경로와 이름 설정 (*파일 확장자명을 반드시 .csv로 저장*)
* '실행' 클릭

![image](https://user-images.githubusercontent.com/93892724/178978818-32d893e9-7fb5-4eae-a2b7-680ef014aa25.png)

<br>
<br>

---

* 내보내기한 팜맵 우클릭 -> 속성 클릭
* 레이어 속성창 좌측 '결합' 클릭
* 새결합 추가 아이콘(밑에 초록색 '+' 모양) 클릭
* 결합 레이어 : 구역통계한 csv파일 클릭
* 결합된 필드 '체크' -> ND_mean 데이터 선택
* 확인 클릭 후 적용 클릭
* 결합된 팜맵 우클릭 -> '속성 테이블 열기' 클릭
* 제일 우측의 '결합'된 '질소량 분포지도' 데이터 값 확인

![image](https://user-images.githubusercontent.com/93892724/178979420-5f5d282f-e780-4747-8918-4c8e89e3ab7c.png)

![image](https://user-images.githubusercontent.com/93892724/178979978-05df39ec-3595-4080-af1c-8eadbf251bca.png)

<br>
<br>

---

* 좌측 레이어 패널에서 내보내기한 팜맵 우클릭 -> 속성 클릭 -> 좌측 '필드' 클릭 -> '필드 계산기'(빨간표시 클릭)
* 산출 필드 이름에 이름 설정 -> 산출 필드 유형 : '십진수(실수)' 
* 표현식 빈칸에 '결합된 데이터 필드명' 입력
* '확인' 클릭

![image](https://user-images.githubusercontent.com/93892724/178981058-39736114-4e2e-4fea-9b0d-3c45c23555d1.png)


![image](https://user-images.githubusercontent.com/93892724/178981540-6f9a093c-7186-4809-956b-30094fc3d8de.png)
-> 실수형으로 표현된 결합 데이터 필드 생성 완료 (GJ_ND_mean, 빨간표시)
-> 적용 누르고 확인 클릭

<br>
<br>

---

* 좌측 레이어 패널에서 내보내기한 팜맵 우클릭 -> '속성 테이블' 클릭 -> 빨간 표시대로 맞춰주고 모두 갱신 버튼 클릭

![image](https://user-images.githubusercontent.com/93892724/178990520-7afc55eb-725f-49ce-89f2-1383ca30818e.png)
-> GJ_ND_mean값이 Null값에서 실수형태의 GJ_ND_ND_mean의 값으로 맞춰짐

<br>
<br>

---

* 내보내기한 팜맵 우클릭 -> '속성' 클릭 -> '심볼' 클릭 -> '단일 심볼'을 '단계 구분'으로 변경 -> '값' 칸에 실수형태의 결합된 데이터 필드명 선택 -> 색상 선택 -> '분류'(빨간표시) 클릭 -> '적용' 클릭 후 '확인' 클릭

![image](https://user-images.githubusercontent.com/93892724/178991140-36f0192f-9bc9-4e0b-bfab-aeed7a387f06.png)

![image](https://user-images.githubusercontent.com/93892724/178991808-873db619-eaec-4849-8fe2-e0a330ee37ff.png)

=> 완성된 필지별 질소량 분포지도 확인.

<br>
<br>

---

* 상단 메뉴바 '프로젝트' 클릭 -> '새 인쇄 조판...' 클릭
* '조판 인쇄 생성'창 활성화 -> 지도 이름 입력 -> 확인 클릭
* 좌측 툴바에서 '지도 추가' 아이콘 클릭
* 흰색 배경에 '좌클릭 + 드래그' 이용하여 '필지별 질소량 분포지도' 추가

![image](https://user-images.githubusercontent.com/93892724/179159593-57cc7e21-8c43-4602-898f-5328aac7b02e.png)

<br>
<br>

---

* 좌측 툴바에서 방위표 추가 아이콘 클릭
* 흰색 배경에 '좌클릭 + 드래그' 이용하여 '방위표' 추가
* '방위표' 선택된 상태에서 '항목 속성' 클릭 -> 디자인 변경 가능

![image](https://user-images.githubusercontent.com/93892724/179160080-e36610fe-a8ff-4133-8f38-81e5752dfd42.png)

<br>
<br>

---

* 좌측 툴바에서 범례 추가 아이콘 클릭 
* 흰색 배경에 '좌클릭 + 드래그' 이용하여' 범례 추가
* '범례' 선택된 상태에서 '항목 속성' 클릭
* '범례 항목'에서 '자동갱신' 체크 된 상태 -> 범례 편집 불가
* '범례 항목'에서 자동갱신' 체크 해제된 상태 -> 범례 편집 가능
-> 필요한 항목만 편집 후 디자인 변경해가며 범례 완성

![image](https://user-images.githubusercontent.com/93892724/179162015-2753957d-3b2a-4cd9-a6ea-ca8bb7fdd9b5.png)


<br>
<br>

---

* 좌측 툴바에서 축척 막대 추가 아이콘 클릭
* 흰색 배경에 '좌클릭 + 드래그' 이용하여' 축척 막대 추가
* 축척 막대 선택된 상태에서 '항목 속성' 클릭
* 단위 설정, 선분 설정

![image](https://user-images.githubusercontent.com/93892724/179162775-b458cf2b-7de8-4b0e-9214-e5fb107d9955.png)


<br>
<br>

---

* 상단 메뉴바 '조판' => 'PDF로 내보내기...' 클릭
* 저장할 경로와 이름 설정 후 '저장' 클릭
* PDF 내보내기 옵션 기본값 그대로 '저장' 클릭


![image](https://user-images.githubusercontent.com/93892724/179163245-95e90b74-dbd2-445c-ad34-9da2c04c5fdc.png)

<br>
<br>

---


![GJ_Readme_map_1](https://user-images.githubusercontent.com/93892724/179163852-5c264103-53b4-4db9-a641-f472b7cc9d92.jpg)

-> PDF파일로 내보내진 '필지별 질소량 분포지도' 완성!




