---


---

<h1 id="회귀-분석과-rnn을-이용한-youtube-인기-동영상-조회수-예측"><strong>회귀 분석과 RNN을 이용한 Youtube 인기 동영상 조회수 예측</strong></h1>
<h2 id="overview">Overview</h2>
<ul>
<li>Youtube 특징
<ul>
<li>YouTube의 사용자 수: 전 세계 20억명 이상</li>
<li>전체 인터넷 사용자 중 약 3분의 1을 차지</li>
<li>18~34세  연령대의 높은 시청률</li>
<li>일일 시청  10억시간</li>
</ul>
</li>
<li>Youtube는 위의 특징과 같이 미디어를 넘어서 전 세계의 문화로 자리매김했다. 따라서 Youtube 조회수 예측을 다양한 기법을 통해 분석하여 새로운 통찰을 제시하는 것은 단순한 미래 수치 예측 그 이상의 가치가 있을 것이다.</li>
<li>본 연구에서는 한국 유튜브 인기 급상승 동영상의 34,567개의 instance들을 사용하여 Youtube 조회수 예측을 목적으로 다양한 기법들을 활용했다.</li>
</ul>
<h2 id="연구-진행-과정">연구 진행 과정</h2>
<ol>
<li>Data Exploration</li>
<li>Data Preperation</li>
<li>Modeling</li>
<li>Evaluation</li>
<li>Discussion &amp; Futureworks</li>
</ol>
<h2 id="연구-기법">연구 기법</h2>
<img src="https://user-images.githubusercontent.com/50453570/102893600-35553000-44a5-11eb-9174-51cf0434b8bb.png" width="90%"></img>
<ol>
  ![image](https://user-images.githubusercontent.com/50453570/102906716-5ffcb400-44b8-11eb-8cb0-c41bc14d392e.png)
<li>PCA<br>
  <img src="https://user-images.githubusercontent.com/50453570/102902818-cbdc1e00-44b2-11eb-855f-24d72d055c30.png" width="60%"></img>
• PCA는 데이터의 분산(variance)을 최대한 보존하면서 서로 직교하는 새 기저(축)를 찾아, 고차원 공간의 표본들을 선형 연관성이 없는저차원 공간으로 변환하는 기법으로, 주성분 분석, 데이터 압축 등에 활용된다. 본 연구에서는 <strong>새로운 축으로 raw data를 저차원 공간으로 변환</strong>하는데 사용한다.</li>
<li>RNN - LSTM<br>
  <img src="https://user-images.githubusercontent.com/50453570/102902779-c088f280-44b2-11eb-8bd3-f5616b861888.png" width="60%"></img>
• RNN(Recurrent Neural Network)은 순환 신경망으로, 고정 길이 입력이 아닌 임의의 시퀀스를 다룰 수 있는 신경망이다.<br>
• 이때, LSTM셀은 RNN 신경망 구조에서 빠른 훈련 수렴과 데이터 장기 의존성을 지원하는 층으로, LSTM에서는 해당 층의 출력이 곧바로 나가지 않고 장기 상태에서 가장 중요한 부분이 저장된다.</li>
<li>Linear Regression<br>
• Baseline Linear Regression<br>
• Lasso Regression<br>
• ElasticNet Regression<br>
에서 Lasso와 ElasticNet은 규제(Regularized)파라미터가 존재하기 때문에 과적합(Overfitting)을 방지한다. 이때 규제의 강도는 Lasso가 가장 강하며, ElasticNet은 Ridge와 Lasso의 규제항을 합한 모델이다.</li>
</ol>
<h2 id="연구-결과">연구 결과</h2>
<p><strong>kaggle-한국유튜브인기동영상 Raw Data</strong></p>
<img src="https://user-images.githubusercontent.com/50453570/102903112-3c833a80-44b3-11eb-8efa-da54a1c4a780.png" width="80%"></img>
<li>Raw data에는 총 16개의 column이 존재한다.</li>
<li>data type은 string, int64, object, boolean, link와 같이 다양한 타입이 존재한다.</li>
</ul>
<p><strong>사용 attribute</strong></p>
![image](https://user-images.githubusercontent.com/50453570/102903194-558beb80-44b3-11eb-96d7-2f8c16b7bb5f.png)
<p><strong>publisht time에 따른 조회수(views)</strong></p>
<img src="https://user-images.githubusercontent.com/50453570/102903740-19a55600-44b4-11eb-9bc7-c04a72f2fe27.png" width="90%"></img>

<p><strong>1. Correlation Analysis</strong></p>
<img src="https://user-images.githubusercontent.com/50453570/102903227-60468080-44b3-11eb-9a68-6c0c326c789e.png" width="60%"></img>
<blockquote>
<p>• views-comment_count : 매우 강한 상관관계<br>
• views-likes : 강한 상관관계<br>
• views-dislikes : 중간 정도 상관관계<br>
• category_id-views / publish_time-views : 상관성이 거의 없음.</p>
  ![image](https://user-images.githubusercontent.com/50453570/102903227-60468080-44b3-11eb-9a68-6c0c326c789e.png)
</blockquote>
<p><strong>2. Multiple Linear Regression - OLS</strong></p>
<blockquote>
<p>선형식<br>
views = 210100 + 54.6277dislikes - 26.9779comment + 19.6944likes<br>
R-squred 80.2% (모델이 views변동성의 80.2%를 설명)</p>
</blockquote>
<p><strong>3. PCA with Regression using Pipeline</strong><br>
K-fold Validation Score (MSE)</p>
<blockquote>
<ol>
<li>Linear -  cv3: 63.6%, cv10: 61.8%</li>
<li>Lasso - cv3: 63.6%, cv10: 61.8%</li>
<li>ElasticNet - cv3: 65.5%, cv10: 62.5%</li>
</ol>
</blockquote>
<p>Training Validation Score가 가장 높은 Elastic Net에 Test set을 fit시킨 결과,<br>
MSE score - 71.5%<br>
R-squared score - 78% 의 성능 결과가 나왔다.</p>
<img src="https://user-images.githubusercontent.com/50453570/102903373-997ef080-44b3-11eb-95e2-8e5f65d8bb95.png" width="40%"></img>

<p>이를 기반으로 2달 뒤의 조회수를 예측한 결과,<br>
**299,370(views)**이다.</p>
<p><strong>4. RNN-LSTM</strong><br>
LSTM의 Unit 개수를 다르게 하여 unit 16, 20, 32개로 조회수 예측을 했다.</p>
<ol>
<li>Unit 16</li>
<img src="https://user-images.githubusercontent.com/50453570/102903800-2b86f900-44b4-11eb-9b89-102e52feaa06.png" width="70%"></img>
<li>Unit 20</li>
<img src="https://user-images.githubusercontent.com/50453570/102903438-b0bdde00-44b3-11eb-8cd4-0b155001eae6.png" width="70%"></img>
<li>Unit 32</li>
<img src="https://user-images.githubusercontent.com/50453570/102903498-c4694480-44b3-11eb-9d8f-42805bfdda93.png" width="70%"></img>

</ol>

<h2 id="결론-및-제언">결론 및 제언</h2>
<ul>
<li>결과적으로 가장 성능이 좋았던 모델링 방법은 PCA를 pipeline을 이용해 ElasticNet Regression모델과 연결 한 것이었다. 이때, PCA의 주성분인 PC의 개수는 3개로 지정하였으며, 이는 기존 분산량의 98.3%이상을 보 존한다. 해당 모델의 Test score(MSE)는 71.5%, Train score는 78%였다. 이를 기반으로 한 2달 뒤의 유튜브 조회수는 299,370회였다. PCA를 적용하지 않았을 때보다 25% 이상 높아진 결과로써, 변동성이 큰 유튜브 조회수의 데이터 특성상, PCA로 데이터를 압축하는 것이 분석에 효과적이었음을 알 수 있다. 한편, 높은 성 능을 기대했던 다층퍼셉트론, RNN의 LSTM셀을 이용한 방법에서는 Accuracy 혹은 MSE와 같은 평가 척도가 0에 가까운 것을 확인했다. 하이퍼파라미터나 활성화 함수의 변경에 따라 결과의 차이가 있을 수 있으나, 단지 구조가 복잡한 모델링 기법을 사용한다고 해서 성능의 향상을 기대할 수 없음을 알 수 있었다.</li>
<li>이는 각기 다른 채널, 영상들의 전반적인 시 간에 따른 유튜브 조회수를 예측한 것이므로 향후 각 개인 채널별 영상들을 데이터 셋으로 선정한다면 해 당 모델을 이용하여 조회수를 예측하고, 컨텐츠 선정이 가능해보인다. 다만, 시도한 모델의 기법들이 다양하 므로 평가 척도가 고르지 못한 점, 각 기법들 중 가장 훈련 성과가 좋았던 모델을 중심으로 하이퍼파라미터 튜닝이 세분화되어 이루어지지 못한 것이 한계점이다. 따라서 후속 연구에는 하이퍼파라미터 튜닝, 조회수에 영향을 미치는 요인 및 조회수를 기반으로한 컨텐츠 제작 아이디어 추천 등의 다양한 분야에 대한 접근을 기대하는 바이다</li>
</ul>
<h2 id="참고-자료">참고 자료</h2>
<p><a href="https://www.kaggle.com/datasnaek/youtube-new">kaggle-유튜브인기동영상 데이터셋</a><br>
<a href="https://teddylee777.github.io/tensorflow/LSTM%EC%9C%BC%EB%A1%9C-%EC%98%88%EC%B8%A1%ED%95%B4%EB%B3%B4%EB%8A%94-%EC%82%BC%EC%84%B1%EC%A0%84%EC%9E%90-%EC%A3%BC%EA%B0%80">LSTM 예측</a><br>
[유튜브 데이터 분석]</p>

