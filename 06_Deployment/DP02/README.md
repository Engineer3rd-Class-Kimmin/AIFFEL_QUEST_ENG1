# DP02 모델 배포 기초

## 1. 실습 수행 내역

이번 실습에서는 FastAPI를 이용하여 API 서버를 실행하고,
Path Parameter와 Query Parameter의 사용 방법을 학습하였다.

또한 MNIST 모델을 FastAPI 서버에 연결하여 `/predict` 엔드포인트를 구현하고,
Swagger UI에서 실제 추론 요청과 응답을 확인하였다.

---

## 1.1 섹션 1.5 - FastAPI 서버 실행

FastAPI 애플리케이션을 실행하고 서버가 정상적으로 동작하는지 확인하였다.

### 서버 실행

![1.5 서버실행](./images/1.5%20서버실행.png)

### 상태 코드 200 확인

서버에 요청을 보내 정상적으로 응답하는 것을 확인하였다.

HTTP 상태 코드 `200`은 요청이 정상적으로 처리되었다는 의미이다.

![1.5 상태코드200](./images/1.5%20상태코드200.png)

---

## 1.2 섹션 2.3 - Query Parameter

Query Parameter를 이용하여 모델 목록을 조건에 따라 조회하였다.

`status=running`을 사용하면 실행 중인 모델만 조회할 수 있고,
`limit=1`을 사용하면 반환되는 모델의 개수를 1개로 제한할 수 있다.

![2.3 Query 파라미터](./images/2.3%20Query%20파라미터.png)

---

## 1.3 섹션 5 - 모델 추론 엔드포인트 구현 및 테스트

Day 1에서 준비한 MNIST 모델과 추론 함수를 FastAPI 서버에 연결하였다.

Pydantic 스키마를 이용하여 입력과 출력 형식을 정의하고,
`POST /predict` 엔드포인트를 통해 모델 추론 요청을 받을 수 있도록 구현하였다.

### Predict 요청

Swagger UI의 `POST /predict`에서 Request Body에
MNIST 이미지의 `pixel_values`를 입력하여 추론을 요청하였다.

![5.1 predict 요청](./images/5.1%20predict%20요청.png)

### Predict 결과

요청을 실행한 결과 모델의 예측값과 confidence,
model_version 등의 응답이 정상적으로 반환되는 것을 확인하였다.

![5.2 predict 결과](./images/5.2%20predict%20결과.png)

---

# 2. 체크포인트 답변

## 체크포인트 1 - Path / Query / Request Body

### Q1. `/models/sentiment-v1`에서 `sentiment-v1`은 어떤 종류의 파라미터입니까?

**답변:** Path Parameter입니다.

URL 경로 자체에 포함되어 특정 모델을 지정하는 값입니다.

예:

```text
/models/sentiment-v1
```

여기에서 `sentiment-v1`은 조회하려는 모델을 나타냅니다.

---

### Q2. `/models?status=running&limit=5`에서 `status`와 `limit`은 어떤 종류의 파라미터입니까?

**답변:** Query Parameter입니다.

URL 뒤의 `?` 다음에 조건을 전달할 때 사용합니다.

```text
status=running
limit=5
```

`status`는 모델의 상태를 조건으로 사용하고,
`limit`은 반환할 결과의 개수를 제한합니다.

---

### Q3. 모델 추론 요청에 Request Body를 사용하는 이유는 무엇입니까?

**답변:** 모델 추론에는 이미지 픽셀값처럼 많은 데이터를 전달해야 하기 때문입니다.

Path나 Query Parameter는 간단한 값을 전달하는 데 적합하지만,
복잡하고 많은 데이터는 JSON 형태의 Request Body로 전달하는 것이 적합합니다.

---

### Q4. FastAPI에서 함수의 파라미터가 Path, Query, Body 중 어디서 오는지 어떻게 판별합니까?

**답변:**

- URL 경로 `{}` 안에 정의된 값 → **Path Parameter**
- 함수의 일반 파라미터 → **Query Parameter**
- Pydantic 모델로 정의된 객체 → **Request Body**

FastAPI는 함수의 선언과 타입 힌트를 이용하여 이를 자동으로 판별합니다.

---

## 체크포인트 2 - Swagger UI

### Q1. FastAPI에서 Swagger UI에 접속하려면 어떤 URL로 이동합니까?

**답변:**

일반적인 로컬 실행 환경에서는 다음 주소로 접속합니다.

```text
http://127.0.0.1:8000/docs
```

Google Colab에서는 로컬 브라우저의 `localhost`가 아니라
Colab에서 제공하는 포트 연결 기능을 이용하여 Swagger UI에 접속하였다.

---

### Q2. Swagger UI가 코드와 항상 동기화될 수 있는 이유는 무엇입니까?

**답변:**

FastAPI가 작성된 API 코드를 기반으로 OpenAPI 문서를 자동으로 생성하기 때문입니다.

API 엔드포인트, 입력 형식, 출력 형식 등이 변경되면
Swagger UI에도 자동으로 반영됩니다.

---

### Q3. Pydantic 모델의 `Field(description=, examples=)`는 Swagger UI의 어디에 반영됩니까?

**답변:**

Swagger UI의 Request Body 스키마와 API 문서에 반영됩니다.

`description`은 해당 필드에 대한 설명을 제공하고,
`examples`는 사용자가 입력 형식을 이해할 수 있도록 예제 값을 보여줍니다.

---

### Q4. Swagger UI와 ReDoc의 핵심 차이는 무엇입니까?

**답변:**

Swagger UI는 API를 문서로 확인하면서 직접 요청을 실행하여 테스트할 수 있습니다.

ReDoc은 API 문서를 구조적으로 읽고 확인하는 데 더 중점을 둔 문서화 화면입니다.

즉,

**Swagger UI → API 테스트 중심**

**ReDoc → API 문서 확인 중심**

---

## 체크포인트 3 - Pydantic

### Q1. `text: str`과 `text: str = "기본값"`의 차이는 무엇입니까?

**답변:**

```python
text: str
```

은 사용자가 반드시 값을 입력해야 하는 필수 필드입니다.

```python
text: str = "기본값"
```

은 값을 입력하지 않았을 때 `"기본값"`이 자동으로 사용되는 필드입니다.

---

### Q2. `Field(..., min_length=1, max_length=5000)`에서 `...`은 무엇을 의미합니까?

**답변:**

해당 필드가 **필수 입력값(required)**이라는 의미입니다.

즉, 사용자가 반드시 값을 제공해야 합니다.

---

### Q3. 422 에러 응답에서 `loc` 필드는 어떤 정보를 담고 있습니까?

**답변:**

`loc`은 입력 데이터에서 오류가 발생한 위치를 알려줍니다.

예를 들어 Request Body의 특정 필드에 문제가 발생했다면,
어떤 필드에서 검증 오류가 발생했는지를 확인할 수 있습니다.

---

### Q4. `response_model`을 지정하면 어떤 이점이 있습니까?

**답변:**

API가 반환하는 응답 데이터의 구조를 명확하게 정의할 수 있습니다.

또한 FastAPI가 응답 데이터를 검증하고,
Swagger UI에 응답 형식을 자동으로 문서화할 수 있습니다.

---

## 체크포인트 4 - Day 2 모델 추론

### Q1. 모델을 서버 시작 시 한 번만 로드해야 하는 이유는 무엇입니까?

**답변:**

모델을 요청이 들어올 때마다 다시 로드하면 시간이 오래 걸리고
메모리와 시스템 자원을 불필요하게 사용하게 됩니다.

따라서 서버가 시작될 때 모델을 한 번만 메모리에 로드하고,
이후 들어오는 추론 요청에서 동일한 모델을 재사용하는 것이 효율적입니다.

---

### Q2. `pixel_values`가 784개가 아닌 요청이 들어오면 어떤 일이 발생합니까?

**답변:**

MNIST 이미지는 `28 × 28 = 784`개의 픽셀값이 필요합니다.

따라서 `pixel_values`가 784개가 아니라면 정상적인 MNIST 이미지 형태로
변환할 수 없으므로 입력 검증 과정에서 오류로 처리해야 합니다.

Pydantic 스키마 또는 추론 전 검증 코드를 이용하여
잘못된 입력을 차단할 수 있습니다.

---

### Q3. `HTTPException(status_code=503)`은 어떤 상황에서 사용했습니까? 왜 500이 아니라 503입니까?

**답변:**

모델이 정상적으로 로드되지 않아 현재 추론 서비스를 제공할 수 없는 경우
`503 Service Unavailable`을 사용할 수 있습니다.

`500 Internal Server Error`는 일반적인 서버 내부 오류를 의미하지만,
`503`은 서버가 실행 중이더라도 현재 일시적으로 요청을 처리할 수 없는
상태임을 나타냅니다.

---

### Q4. Swagger UI에서 PredictRequest의 `description`과 `examples`가 어디에 표시됩니까?

**답변:**

Swagger UI의 `POST /predict` 항목을 펼쳤을 때
Request Body의 스키마와 예제 영역에서 확인할 수 있습니다.

이를 통해 API 사용자가 `pixel_values`에 어떤 데이터를 입력해야 하는지
쉽게 이해할 수 있습니다.

---

# 3. Day 2 학습 정리

이번 실습을 통해 다음 내용을 학습하였다.

- FastAPI 서버 실행
- HTTP 요청과 응답 확인
- Path Parameter와 Query Parameter 구분
- Request Body 사용
- Pydantic을 이용한 입력 및 출력 스키마 정의
- Swagger UI를 이용한 API 문서 확인 및 테스트
- MNIST 모델과 FastAPI 연결
- `/predict` 추론 엔드포인트 구현
- 입력값 검증과 HTTP 상태 코드 처리
- 실제 모델 추론 요청 및 응답 확인

최종적으로 **학습된 MNIST 모델 → FastAPI → `/predict` API → Swagger UI 테스트**까지
모델을 API 서비스 형태로 연결하는 전체 흐름을 확인하였다.