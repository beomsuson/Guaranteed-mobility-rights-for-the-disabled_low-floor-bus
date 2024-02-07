# 2021-SMU Data Campus Project

---
# <center>장벽 없는 휠체어<br><br> *장애인 이동권 개선을 위한 저상버스 경로 제안 및 기대효과*</center> [Uploading ppt_장벽없는 휠체어_장애인 이동권 개선을 위한 저상버스 경로 제안 및 기대효과.PDF…]()

---
### Project nickname : 일석칠조(7조)
### Project execution period : 2022. 6. 27 ~ 8. 31
### Project Hosting : 손범수 김정민 이현지 조민진 조혜진 홍정민
---

## ● Description

최근 전국 장애인 연대의 시위로 인해 더욱 화제로 떠오른 '장애인 이동권'. 저희는 해당 문제의 개선 필요성을 깨달아, 장애인들의 교통 편의 증진을 위한 방법을 모색하였습니다. 
저상 버스의 높은 수요, 특별 교통 수단들의 한계점, 서울시의 저상버스 보급률 확충 계획을 바탕으로, 장애인의 이동권 개선을 위한 교통 수단으로써 저상 버스를 선택했습니다.<br><br>

일석칠조는 장애인들의 이동 편의를 증진시키기 위해 현 저상버스 우선 보급 노선 선정, 기존 노선과 선정된 정류장을 비교하여 신노선을 구축하는 두가지 프로젝트를 진행하였습니다.<br>
노선 제안에는 크게 인구 데이터, 시설 데이터, 교통수단 데이터 3가지를 사용하였습니다.<br>
저상버스 우선 보급 노선의 경우, 1차는 거리 기준, 2차는 수요 기준으로 필터링을 통해 최종적으로 선정된 47개의 버스 정류장을 가장 많이 통과하는 9대의 버스노선을 선정하고 대기시간 감소를 위해 추가 배차를 제안합니다.<br>
신노선 구축 방법 또한 군집 내 정류장과 기존 노선의 거리를 기준으로 필터링 과정을 거친 후 그리디(Greedy) 알고리즘을 통하여 1차적으로 신 노선을 구축합니다. 이후 티맵 api를 통해 도로 교통 상황을 반영 하였고, 세부 기준에 따라 실질적으로 추가할 7대의 신 노선을 제안합니다. 더 나아가 선행연구와 장애인 콜텍시 데이터를 수집을 통해, 오전10시 ~ 오후 6시 사이에 신노선 저상버스 투입을 제안합니다.<br><br>
이처럼 일석칠조의 장애인 이동권 개선을 위한 저상버스 노선 확대와 신노선 제안 프로젝트는 대기 시간 감소와 교통 요금 절약이라는 두 가지 수치적 개선 효과를 도출해낼 수 있을 것입니다.

---

## ● Environment

Python Version 3.9.7 (Window)

---

## ● Prerequisite

import numpy as np

import pandas as pd
 
import matplotlib.pyplot as plt

import json

import requests

import folium

import seaborn as sns

import matplotlib.font_manager as fm

import random as rd

!pip install yellowbrick <br>
!conda install -c districtdatalabs yellowbrick

from sklearn.cluster import KMeans

from sklearn.preprocessing import StandardScaler

from sklearn.metrics import silhouette_score

from haversine import haversine

from random import randint

from datetime import datetime, timedelta

pip install xmltodict<br>
import xmltodict

import xml.etree.ElementTree as ET

---

## ● Files
* **Data preprocessing** <br> 

   * 목록 1-1 병원_raw_data.xlsx: 
   * 목록 1-2 보건복지부_장애인복지관 현황_20191231_raw_data.csv
   * 목록 1-3 월별_장애인_지하철_이용건수.xlsx
   * 목록 1-4 서울시 사회복지시설(장애인거주시설) 목록.csv
   * 목록 1-5 서울시 사회복지시설(장애인생산품판매시설) 목록.csv
   * 목록 1-6 서울시 사회복지시설(장애인의료재활시설) 목록.csv
   * 목록 1-7 서울시 사회복지시설(장애인지역사회재활시설) 목록.csv
   * 목록 1-8 서울시 사회복지시설(장애인직업재활시설) 목록.csv
   * 목록 1-9 BUS_STATION_BOARDING_MONTH_202201.csv
   * 목록 1-10 BUS_STATION_BOARDING_MONTH_202202.csv
   * 목록 1-11 BUS_STATION_BOARDING_MONTH_202203.csv
   * 목록 1-12 BUS_STATION_BOARDING_MONTH_202204.csv
   * 목록 1-13 BUS_STATION_BOARDING_MONTH_202205.csv
   * 목록 1-14 BUS_STATION_BOARDING_MONTH_202206_1.csv
   * 목록 1-15 busline_data.xlsx
   * 목록 1-16 busline_data.xlsx
* **Datasets**
   * 목록 1-1 dataset.csv
   * 목록 1-2 접근성_점수_필터링_dataset.csv
      
 * **Filtering**
    * 목록 1-1 접근성점수_장애인수_필터링dataset.csv
    * 목록 1-2 접근성점수_장애인수_콜택시_필터링dataset.csv
    * 목록 1-3 접근성점수_장애인수_콜택시_버스승하차_필터링dataset.csv
    * 목록 1-5 dataset_final.csv
 * **Route selection**
    * 목록 1-1 busline_data.xlsx
    * 목록 1-2 구별_장애인수.csv
    * 목록 1-3 data_01_06_시간비교.xlsx

---

## ● Leverage APIs
tmap 네비게이션 api를 통해 point1 -> point2 가는 시간 계산(도로 반영)<br>
>Url = https://apis.openapi.sk.com/tmap/routes?version={version}&callback={callback}

카카오 api 활용 주소를 통해 위도 경도 출력<br>
>Url = 'https://dapi.kakao.com/v2/local/search/address.json?query={}

서울시 장애인 콜시스템 1월 - 6월 XML 데이터 출력
>http://openapi.seoul.go.kr:8088/{api_key}/xml/disabledCalltaxi/1/1000/{dates}

---


## ● Usage
파일의 구성은 다음과 같습니다<br><br>

>/1_데이터_전처리/2_지하철역_전처리<br>
./1_데이터_전처리/3_장애인시설_전처리/1_장애인사회복지시설_데이터셋<br>
./1_데이터_전처리/3_장애인시설_전처리/2_장애인사회복지시설_데이터셋_좌표추가<br>
./1_데이터_전처리/4_버스데이터_전처리/0_버스정류장_승하차인원/승하차평균이상정류소선별<br>
./1_데이터_전처리/4_버스데이터_전처리/1_버스데이터전처리<br>
./1_데이터_전처리/4_버스데이터_전처리/2_버스데이터확인<br>
./1_데이터_전처리/5_콜택시데이터_전처리/departure_destination(01_06)<br><br>
./2_데이터셋/1_최소_거리_계산<br>
./2_데이터셋/2_데이터셋_만들기<br><br>
./3_접근성_지수/접근성지수<br><br>
./4_택시_버스_장애인수_필터링/1_장애인수필터링/접근성점수_장애인수_필터링dataset<br>
./4_택시_버스_장애인수_필터링/2_콜택시필터링/콜텍시평균_자르기<br>
./4_택시_버스_장애인수_필터링/3_버스승하차필터링/버스_승하차_필터링<br>
./4_택시_버스_장애인수_필터링/4_지하철승하차필터링/지하철승하차_필터링<br><br>
./5_노선선정/1_정류장_시각화<br>
./5_노선선정/2_정류소_클러스터링(k-means)<br>
./5_노선선정/3_기존_노선_비교_노선배차용<br>
./5_노선선정/3_기존_노선_비교_노선수정용<br>
./5_노선선정/4_지역별장애인_및_배차버스시각화<br>
./5_노선선정/4_노선선정<br>
./5_노선선정/버스_대기시간<br>
