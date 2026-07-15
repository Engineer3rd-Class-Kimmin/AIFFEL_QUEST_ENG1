# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 조희연
- 리뷰어 : 리뷰어의 이름을 작성하세요.

## 프로젝트: mini BERT 만들기

`mini_bert.ipynb`에서 vocab size를 **8000**으로 줄이고 전체 파라미터가 **약 1M**인
아주 작은 mini BERT를 **MLM + NSP** 두 task로 **10 Epoch** 사전학습(pretrain)했습니다.

- SentencePiece BPE 토크나이저 (`vocab_size=8000`, `[PAD] [UNK] [CLS] [SEP] [MASK]` 포함)
- MLM: 전체 토큰의 15% 마스킹(80% `[MASK]` / 10% 랜덤 / 10% 원본), 특수토큰 제외
- NSP: 두 문장을 50% 확률로 IsNext/NotNext, `[CLS] A [SEP] B [SEP]` + segment(0/1)
- `np.memmap` + JSON 메타로 메모리 효율적 데이터셋 저장
- pad mask / ahead mask, gelu, parameter initializer, JSON config 유틸리티
- Embedding · Transformer encoder · BERT · pretrain 모델 (MLM head weight tying)
- warmup + linear-decay LR 스케줄링, MLM/NSP loss·accuracy 동시 학습

한국어 코퍼스는 나무위키 대신 공개 **NSMC**를 stand-in으로 사용하며(네트워크 실패 시
합성 코퍼스로 자동 대체), CPU에서 end-to-end로 실행되도록 작은 데모 규모로 구성했습니다.

## 학습훈련 기록

전체 파라미터·10 Epoch 학습 로그·시각화는 노트북에 실행 결과로 포함되어 있고,
학습 곡선은 [`training_records/training_curves.png`](training_records/training_curves.png)에 저장했습니다.

- 총 파라미터: **1,025,218 (~1.03M)** — token embedding(8000×96 ≈ 0.77M)이 대부분
- MLM loss: `8.211 → 5.949` (acc `0.030 → 0.164`)
- NSP loss: `0.685 → 0.224` (acc `0.529 → 0.910`)

NSP는 안정적으로 수렴하고, MLM은 8000-way 분류 + 작은 모델·데이터라 loss가 천천히
내려가며 accuracy가 낮게 유지됩니다(과제 안내대로 "작은 모델은 수렴이 어려울 수 있음"을
그대로 관찰). 정성 평가에서 마스킹된 단어가 그럴듯하게 예측되는 것도 함께 기록했습니다.

# PRT(Peer Review Template)
- [ ]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
        - 중요! 해당 조건을 만족하는 부분을 캡쳐해 근거로 첨부

- [ ]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭을 왜 핵심적이라고 생각하는지 확인
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드의 기능, 존재 이유, 작동 원리 등을 기술했는지 확인
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 중요! 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부

- [ ]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나 새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 문제 해결 과정(시도/실패/재시도)을 기록했는지 확인
        - 중요! 해당 내용이 기록된 부분을 캡쳐해 근거로 첨부

- [ ]  **4. 회고를 잘 작성했나요?**
    - 배운 점/개선점/다음 계획이 포함되어 있는지 확인
