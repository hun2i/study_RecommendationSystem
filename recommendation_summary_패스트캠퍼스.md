# 추천시스템 - 패스트캠퍼스
- 사용자 관점
	- 사용자의 취향/선호를 파악
	- 사용자가 좋아할 것으로 예상되는 아이템을 예측/제공
- 서비스 제공자 관점
	- 서비스 제공자가 목표로 하는 KPI(매출, PV, UV)를 달성
- 도메인에 따라 다른 접근 방식 필요
- 엔지니어가 바라보는 추천
	- 요건, 데이터, 모델, 계량방식, UI/UX
	- 사용자나 서비스에 필요한 바를 잘 정의하고, 
	- 다양한 데이터를 수집하고 활용하여, 
	- 적절한 방식을 통해 아이템의 적합도를 계량하여, 
	- 적절한 방식으로 제공하는 작업

## 추천 요건에 따른 분류
- Best Recommendation
	- Input: 전체 특정 카테고리
	- 예시: 베스트셀러
	- Ranking 지표: 베스트지표
	- 정의
		- Best 지표에 따라 다양한 추천 결과가 생성
		- 이력이 없는 신규 사용자에게도 추천 가능
		- 추천 결과가 없는 경우 보완해주는 역할
		- Popularity가 높은 아이템 추천
- Related Recommendation
	- Input: 특정 Item
	- 예시: 관련 상품
	- Ranking 지표: 아이템간의 유사도
	- 정의
		- 대체재(서로 대신 쓸 수 있는 관계의 두 가지의 재화) vs 보완재(서로 보완 관계에 있는 재화)
		- 주어진 아이템과 유사도가 높은 아이템 추천
- Personalized Recommendation
	- Input: 특정 User
	- 예시: 당신을 위한 맞춤 추천
	- Ranking 지표: 사용자의 아이템 선호도
	- 정의
		- 사용자 별 개별화된 세밀한 추천 결과 제공
		- 사용자 입장에서 본인에게 큐레이션된 서비스를 제공
		- 사용자의 이력 데이터가 풍부해야 좋은 추천 결과 제공이 가능
		- 사용자의 선호도가 높은 아이템 추천, 사용자와 유사한 사용자들이 선호하는 아이템 추천
- Context Aware Recommendation
	- Input: 특정 상황
	- 예시: 비오는 날 듣기 좋은 음악
	- Ranking 지표: 상황과 아이템의 유사도

## Feedback 데이터에 따른 분류
- Explicit Feedback
	- 사용자의 아이템에 대한 명시적 선호 정보를 가진 이력
		- Rating 이력
			- 1점~10점, 만족/보통/불만족, 좋아요/싫어요
- Implicit Feedback
	- 사용자의 아이템에 대한 암시적 선호 정보를 가진 이력
		- 사용자의 아이템 소비 이력
			- 조회, 구매, 시청

## 모델에 따른 분류
- 아이템 인기도, 아이템-아이템 유사도, 사용자-아이템 선호도, 사용자-사용자 유사도
- Content-based Filtering/Recommendation
	- 아이템의 속성 기반 추천
	- 다른 사용자의 행동 이력 사용 X
	- 장점
		- No Item Cold Start Problem
	- 단점
		- User Cold Start Problem
		- Requires contents for item
		- Burden for Content Processing Less Diversity
- Collaborative Filtering
	- 사용자들의 행동 이력 기반 추천
	- 아이템의 속성은 사용 X
	- 장점
		- No item content required
		- Better Prediction Accuracy
	- 단점
		- Item & User Cold Start Problem
		- Data Sparsity
		- Scalability
		- Shilling Attack
- Hybrid Recommendation
	- Content-based + Collaborative Filtering
	- 아이템의 속성과 사용자의 행동 이력을 모두 사용

## 계량 방식에 따른 분류
- 점수 예측
	- 사용자의 아이템에 대한 선호 점수를 예측, 선호 점수가 높은 아이템을 추천
- Top-K Recommendation
	- 선호 점수에 기반한 아이템의 Ranking이 중요
	- 선호 점수를 잘 예측할 필요는 없음

## 추천시스템 성능 평가
- 기존 모델 개선/신규 모델 개발 -> (사전검증) 이력 데이터 기반 성능 평가 -> 심사 위원 평가 -> (사후검증) A/B Test
- 온라인 성능 평가 - A/B Test
	- 서비스의 KPI 지표로 평가
- Rating Prediction 성능 평가
	- 모델이 예측한 Rating과 사용자의 실제 Rating 차이를 계산
	- RMSE: 큰 오차에는 큰 페널티, 작은 오차에는 적은 페널티 부여
	- MAE: 오차만큼의 페널티를 부여
- Top-K 추천 성능 평가
	- 추천한 아이템 중 사용자에 의해 클릭된 아이템의 위치&개수를 이용하여 평가
	- Precision@K의 한계
		- 추천된 아이템에서 만족한 아이템이 동일한 개수일 때 추천된 순서에 따라서 값이 달라지지 않기 때문에 순서를 반영하지 않음
	- Average Precision
		- 추천된 아이템의 순서를 반영하여 관련성 있는 아이템이 상위에 추천될 때 더 높은 값을 가짐
	- MAP: Mean Average Precision
		- 평가 데이터가 여러 개인 경우 각 평가 데이터에 대한 결과의 평균을 계산
	- NDCG: Normalized Discounted Cumulative Gain
		- AP와 마찬가지로 관련성이 결과를 상위에 추천하는지를 평가하는 지표
		- DCG@K는 K값에 따라서 민감하게 변함
		- K에 따른 변화를 최소화하기 위해 최선의 순서일 때의 DCG@K, 즉 I(deal)DCG@K를 구하고 이를 이용하여 정규화한 지표
		- DCG는 K값이 커지면 커지는데 반해 NDCG는 K값에 따라서 변화가 적고, 0~1 사이의 값을 가지기에 모델의 성능을 좀 더 쉽게 비교할 수 있음

## 추천시스템 고려사항
###  Prediction
- 사용자의 다음에 행위(클릭/구매/시청 등)할 아이템을 예측하여 추천
- 매트릭스를 본 사용자에게 매트릭스2, 매트릭스3을 추천
- 사용자의 navigation step을 단축
### Discovery
- 사용자가 인지하지 못하는 아이템을 추천
- 매트릭스를 본 사용자에게 인셉션, 블레이드러너를 추천
- Longtail 아이템을 제공하여 사용자의 서비스 exploration을 높임
### 추천의 만족도: Beyond Accuracy
- Accuracy: 유저의 평소/소비에 맞게 예측하는지
- Diversity: 다양한 유형의 아이템이 추천되는지
- Serendipity: 예상치 못한 (surprise) 아이템이 추천되는지
- Novelty: 그동안 경험하지 못한 새로운 아이템이 추천되는지
- Coverage: 얼마나 많은 아이템이/사용자에게 추천되는지?
### Trust and Explanation
- 추천 결과에 대해 사용자가 신뢰할 수 있어야 함
- 추천 결과가 어떻게 생성되었는지 설명 제공
