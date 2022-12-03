# aix-assign-project-1-2022-fall
AI+X: Deep Learning Hanyang university

## Title: 삼성전자 사도 될까?



## Members: 

김재현, 경제금융학부, kimjh0306@hanyang.ac.kr

정민석, 기계공학부, jungms0613@gmail.com

한승현, 컴퓨터소프트웨어학부, captain8380@naver.com


## I. Proposal (Option A)


### Motivation: Why are you doing this?

'우리나라를 대표하는 주식인 삼성전자를 지금 사도 되는 것일까?' 라는 고민을 하게 되었습니다.


### What do you want to see at the end?

삼성전자를 지금 사면 내일 상승할 지를 다양한 feature들과 딥러닝 및 머신러닝 모델을 사용해서 예측해보려고 합니다.

또한, 어떤 지표가 중요한지도 찾아보겠습니다.


## II. Datasets
<img width = "100%" src="https://user-images.githubusercontent.com/117578583/204284156-c8d18d0f-9995-4bd9-b6c6-3ac4807a79d7.PNG"/>

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

## III. Methodology 

알고리즘은 랜덤포레스트(Random forest)를 선택하였습니다.

현재 데이터셋의 상황이 타이타닉 데이터셋과 유사하다고 생각했습니다. 

타이타닉 데이터셋의 생존여부가 이 데이터셋에서는 result(상승 or 하락)라 생각했습니다.

따라서 해당 데이터셋을 타이타닉 데이터 분석과 비슷한 방식으로 진행하려고 합니다.


## IV. Evaluation & Analysis
- Graphs, tables, any statistics (if any)

- 먼저 필요한 패키지를 설치합니다.

```R
install.packages('~~~~')   #library 오류시 먼저 install.package 사용하기 python에서 pip install과 같은 역할
library(ggplot2)           #drawing code
library(ggthemes)          #to use theme_few()
library(scales)            #to use label  
library(randomForest)      #prediction code
```

- DLdataset.csv를 다운 받은 다음 불러옵니다.

```R
all <- read.csv('C:/Users/ADMIN/OneDrive - 한양대학교/문서/카카오톡 받은 파일/DLdataset.csv', header = T,stringsAsFactors = F) #reading csv file
```

- 날짜에서 월과 일을 추출해서 열에 추가합니다.

```R
all$month <- substr(all$date, 6, 7)  #월 추출
all$day <- substr(all$date, 9, 10)   #일 추출
all$month <- as.factor(all$month)
all$day <- as.factor(all$day)
```

- 다음 날 종가가 오늘 종가보다 높다면 1을 주고 그렇지 않다면 0을 줘서 result열을 추가했습니다. 그러기 위해서 먼저 result열을 만들었습니다.
마지막 날(2021년 12월 말)은 다음 날 정보가 없으니 마지막행을 지우는 작업까지 수행했습니다.

```R
all$result <- 0  #create result column

#다음 날 종가가 오늘 종가보다 올랐다면 오늘행의 result 1 아니면 0
i <- 1
while (i < 1723){
  if (all$closing_price[i] < all$closing_price[i+1]){
    all$result[i] <- 1
  } else{
    all$result[i] <- 0
  }
  i <- i+1
}

all <- all[1:1722,]       #delete first row because of no result data in first row
all$order <- all$order-1  #because delete first row
```

- 새로운 feature들을 추가해줬습니다. price의 경우 종가를 100으로 나누고 반올림한 값입니다. trade는 해당일에 거래량을 총발행 주식수롤 나누고 1000배를 한 것을 백분율로 표현한 값입니다. margin은 net profit margin으로 한국말로는 순이익률이라고 합니다. 연 당기순이익을 연 매출액으로 나누는 게 일반적이지만 여기서는 분기별 당기순이익을 분기별 매출액으로 나눈 값입니다.

```R
all$price <- round(all$closing_price/100)   #reduce the closing price to a simple number
all$trade <- all$trading_volume*100*1000/all$number_of_issued_shares #1000 times the percentage of shares actually traded from issued stock
all$margin <- all$NetIncome/all$revenue #net profit margin
```

- 여기까지 작업한 내용을 보여주는 코드들은 다음과 같습니다.

```R
head(all)  #all의 앞부분 보기
tail(all)  #all의 뒷부분 보기
str(all)   #show structure of all
```

- 이제 데이터셋을 훈련용과 테스트용으로 나누겠습니다.

```R
train <- all[1:1200,]    #dividing train set from data set
test <- all[1200:1722,]  #dividing test set from data set
```

- random을 쓰면 항상 값이 변하기 때문에 set.seed 함수를 통해 결과값이 변하지 않게 해줍니다.

```R
set.seed(2022) #set the seed
```

- randomforest 모델을 사용하고, error rate를 나타내는 그래프를 그립니다.

```R
model <- randomForest(factor(result) ~ price + trade + margin + month , data = train) #create predicting model

#create plot of model's error rate
plot(model, ylim=c(0.30,0.70)) 
legend('topright', colnames(model$err.rate),col=1:3,fill=1:3)
```

- 모델에서 변수의 중요도를 계산하고 시각화 했습니다.
```R
#create data frame of model's importance
model_importance <- importance(model)
varImportance <- data.frame(Variables = row.names(model_importance),Importance = round(model_importance[ , 'MeanDecreaseGini'],2)) 

models_Importance <- varImportance


#create plot of model's importance 
ggplot(models_Importance, aes(x = reorder(Variables, Importance),y = Importance, fill = Importance)) +
  geom_bar(stat='identity') + geom_text(aes(x = Variables,y=0.5,label=Importance),hjust = 0, vjust = 0.55, size = 4, colour = 009933) +
  labs(x='Variables') + coord_flip() + theme_few()
```

- 만들 모델을 가지고 예측을 했습니다.
전체 중에 몇 %를 맞췄는지를 나타내는 것이 마지막 코드입니다.
```R
prediction <- predict(model, test) #save result of predicted test

increase_prediction <- data.frame(date = test$date, result = as.integer(prediction)-1) #create dataframe of predicted result

increase_prediction$SOF <- 'N' #create tag 'success or failure'

increase_prediction$SOF[test$result==increase_prediction$result] <- 'S'
increase_prediction$SOF[test$result!=increase_prediction$result] <- 'F'

success <- nrow(increase_prediction[increase_prediction$SOF=='S',]) #counting whate we predict successfully

print(success*100/522) #print percentage of result
```

- 이제 결과 값을 csv 파일로 저장합니다.
```R
write.csv(increase_prediction, file = 'solution.csv', row.names = F) #save csv file of predicted result
```


## V. Related Work (e.g., existing studies)
- Tools, libraries, blogs, or any documentation that you have used to do this project.

## VI. Conclusion: Discussion
