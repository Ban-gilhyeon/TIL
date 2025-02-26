## 서론 
Uredgo 프로젝트를 진행하며 User_Service와 Notification_Serivce를 맡아서 작업을 하였다. 

문제는 User_Service에서 추가로 닉네임 중복 검증과 비속어(부적절한) 닉네임 검증 로직이 필요하였다. 

비속어를 필터링하는 법은 주로 
1. 기존의 LLM 모델 (ChatGPI, Google Gemini) 사용 
2. 오픈소스의 ai모델을 직접 머신러닝하여 사용 
크게 두 가지로 찾아졌다. 

문제는 1번의 경우 비용이 발생한다는 것 ... 이미 애플 소셜 로그인에 12만원 상당과 이전 데브코스에서 AWS로 비용을 내서 취준생인 내게는 비용적인 문제가 부담스러웠다.

그래서 2번의 방법으로 .. (어차피 GPT가 많이 해줄거라 무서울 것 없음..)

2번 방식의 모델은 여러가지가 있더라 
TensorFlow 
- keras
- Toxicity/Offensiveness
PyTorch 
- Hugging Face Transformers 
	- Bert
	- RoBERTa
	- Detoxify
등등 .. 

아래 블로그를 참고하여 PyTorch에 Bert모델을 학습 시키기로 하였다. 
https://uiandwe.tistory.com/1395

스마일게이트의 usmile 데이터셋을 활용하여 라벨링 작업을 따로 할 필요는 없었다 

## PyTorch

PyTorch는 신경망 구축에 사용되는 오픈소스 딥러닝 프레임워크
- 간단한 선형 회귀 알고리즘부터 복잡한 컨볼루션 신경망과 컴퓨터비전 및 자연어 처리에 특화 
- 생성형 트랜스포머 모델에 이르기까지 다양한 신경망 아키텍처를 지원

### Tensor
모든 머신러닝 알고리즘에서, 심지어 소리나 이미지처럼 겉으로 보기에 숫자가 아닌 정보에 적용되는 알고리즘에서도 데이터는 반드시 숫자로 표현되어야 함 





