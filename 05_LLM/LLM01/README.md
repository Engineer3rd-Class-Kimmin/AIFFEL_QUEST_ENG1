# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 조희연
- 리뷰어 : 리뷰어의 이름을 작성하세요.

## 프로젝트: GPT-1 — Generative Pre-Training 논문 구현

`gpt1_generative_pretraining.ipynb`에서 번역기·챗봇에 사용한 Transformer를
decoder-only 구조로 변경해 GPT-1의 학습 프레임워크를 구현했습니다.

- Causal Multi-Head Self-Attention
- 학습 가능한 토큰/위치 임베딩과 embedding weight tying
- 사전학습 언어 모델 목적함수 \(L_1\)
- 지도학습 분류 목적함수 \(L_2\)
- 보조 언어 모델 목적함수를 포함한 \(L_3=L_2+\lambda L_1\)
- `<start>`, `<delimiter>`, `<extract>` 기반 task-aware input transformation
- 분류, 문장쌍/함의, 유사도, 객관식 task head
- SentencePiece BPE와 작은 한국어 데이터로 실행 가능한 end-to-end 예제

기본 하이퍼파라미터는 CPU 데모용이며, 노트북 마지막에 GPT-1 논문의
12-layer/768-hidden/12-head 설정과 실제 대규모 학습 시 고려사항을 정리했습니다.

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
