# aix-assign-project-1-2022-fall
AI+X: Deep Learning Hanyang university

# Title: 삼성전자는 사도 될까?



# Members: 

김재현, 경제금융학부, kimjh0306@hanyang.ac.kr

정민석, 기계공학부, jungms0613@gmail.com

한승현, 컴퓨터소프트웨어학부, captain8380@naver.com


# I. Proposal (Option A)


- Motivation: Why are you doing this?

'우리나라를 대표하는 주식인 삼성전자를 지금 사도 되는 것일까?' 라는 고민을 하게 되었습니다.


- What do you want to see at the end?

삼성전자를 지금 사면 내일 상승할 지를 다양한 feature들과 딥러닝 및 머신러닝 모델을 사용해서 예측해보려고 합니다.

또한, 어떤 지표가 중요한지도 찾아보겠습니다.


# II. Datasets
<img width = "80%" src="https://user-images.githubusercontent.com/117578583/203816444-d0f28170-5828-4ad3-9378-c418b8b154c0.PNG"/>

삼성전자의 2015년부터 2021년까지 주가데이터를 가져왔습니다.

열에는 일자, 종가, 거래량, 상장주식수, 매출액, 당기순이익, month, day, result가 있습니다.

일자는 거래일을 의미합니다. 따라서 한국 주식장이 쉬는 날(ex: 주말, 공휴일 등등)은 포함되어 있지 않습니다. 따라서 해당 데이터셋의 데이터행은 인덱스 제외 1723행입니다.

종가는 해당 거래일의 마지막 가격을 의미합니다.

거래량은 해당 거래일의 거래주식수를 의미합니다.

상장주식수는 상장되어 있는 삼성전자 주식수를 의미합니다.

매출액 및 당기순이익은 금융감독원 전자공시시스템(DART)에 공시된 분기별 자료를 바탕으로 했으며 단위는 억원입니다.

ex) 첫 행의 매출액 및 당기순이익을 해석하면 해당 분기(2015년 1분기: 2015.01~2015.03) 매출액은 약 47조 1179억 2천만원이고 당기순이익은 약 4조 6258억 2천만원을 의미합니다.

분기별 매출액 및 당기순이익이기에 해당년도 1분기 2분기 3분기 4분기 합을 구하시면 사업보고서에 나오는 해당 년도 매출액 및 당기순이익이 나옵니다.

해당 지표는 연결재무제표를 기반으로 추출하였습니다.

month는 해당 월을 의미하고, day는 해당 일을 의미합니다. 월별, 일별로 특징이 있을 것으로 생각해 추출하였습니다.

result는 다음날 등락률이 양수이면 1 음수이면 0으로 하였습니다. 예를 들어 오늘 종가보다 내일 종가가 더 높다면 오늘 행의 result에 1을 해놓았습니다.

해당 데이터 파일은 KRX 정보데이터시스템, DART를 통해 추출된 데이터를 합성하여 직접 만들었습니다.

# III. Methodology 

알고리즘은 랜덤포레스트(Random forest)를 선택하였습니다.

현재 데이터셋의 상황이 타이타닉 데이터셋과 유사하다고 생각했습니다. 

타이타닉 데이터셋의 생존여부가 이 데이터셋에서는 result(상승 or 하락)라 생각했습니다.

따라서 해당 데이터셋을 타이타닉 데이터 분석과 비슷한 방식으로 진행하려고 합니다.


# IV. Evaluation & Analysis
- Graphs, tables, any statistics (if any)

# V. Related Work (e.g., existing studies)
- Tools, libraries, blogs, or any documentation that you have used to do this project.

# VI. Conclusion: Discussion
