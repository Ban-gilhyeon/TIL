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

```
**1. Python에서 ONNX 모델 생성**

• **모델 학습 완료 후 변환:**

Python에서 학습한 모델을 ONNX 포맷으로 변환합니다.

이 과정에서 dummy input 데이터를 사용해 모델을 내보내면, 결과로 “model.onnx” 파일이 생성됩니다.

**2. ONNX 파일 준비 및 배포**

• **ONNX 파일 확보:**

생성된 “model.onnx” 파일을 Java Spring 프로젝트에서 접근 가능한 위치(예: 서버 파일 시스템, 클라우드 스토리지 등)에 배치합니다.

**3. Java Spring 프로젝트에 ONNX Runtime 추가**

• **의존성 추가:**

Maven이나 Gradle을 통해 ONNX Runtime Java 라이브러리를 프로젝트에 추가하여, ONNX 파일을 로드하고 추론할 수 있는 기능을 사용합니다.

**4. ONNX 파일 불러오기 및 모델 세션 생성**

• **파일 로드:**

Java 애플리케이션이 시작될 때, ONNX Runtime API를 사용해 지정된 경로에서 “model.onnx” 파일을 로드합니다.

• **세션 생성:**

로드한 파일을 기반으로 ONNX Runtime 세션(session)을 생성합니다. 이 세션은 추론 요청이 들어올 때마다 재사용됩니다.

**5. 입력 데이터 준비**

• **토큰화 과정 재현:**

사용자가 입력한 텍스트를 Python에서 사용했던 토크나이저와 동일한 방식(또는 호환 가능한 방식)으로 전처리하여, 정수 배열(예: input_ids, attention_mask)로 변환합니다.

• **입력 텐서 생성:**

변환한 데이터를 ONNX Runtime에서 요구하는 형태(예: 배치 사이즈와 시퀀스 길이를 가진 텐서)로 변환합니다.

**6. 추론 실행 및 결과 처리**

• **추론 실행:**

준비된 입력 텐서를 ONNX Runtime 세션에 전달하여, 모델 추론을 실행합니다.

• **결과 획득:**

모델은 logits와 같은 결과 텐서를 반환하며, 이를 후처리(예: 시그모이드 함수 적용, 임계값 적용 등)하여 최종 예측 결과(비속어 여부 등)로 변환합니다.

**7. Java Spring과의 통합**

• **Service 계층 구성:**

위의 추론 로직을 하나의 Service 클래스로 캡슐화합니다.

이 Service는 ONNX 파일을 로드한 후, 입력 데이터를 받아 추론 결과를 반환하는 메서드를 제공합니다.

• **REST API Controller 구성:**

Spring Controller에서 클라이언트의 HTTP 요청을 받아 Service를 호출하고, 결과를 JSON 형태로 응답합니다.

  

예를 들어, 클라이언트가 특정 문장을 전송하면 Controller가 이 문장을 Service로 전달하고, Service는 ONNX 모델을 통해 추론한 결과(예: 각 클래스별 확률 또는 이진 예측)를 반환합니다.
```
