# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 조희연
- 리뷰어 : 리뷰어의 이름을 작성하세요.

## 프로젝트
- 주제 : Day 2 실습 — Advanced·Modular RAG + RAGAS 평가
- 제출물 : [`Advanced_Modular_RAG_RAGAS.ipynb`](./Advanced_Modular_RAG_RAGAS.ipynb) (전체 셀 실행 출력 포함)
- 내용 : KorQuAD v1 Naive RAG 베이스라인 → Multi-Query / RAG-Fusion(RRF) / HyDE / Cross-encoder Reranking / Self-RAG → RAGAS 4대 지표 비교 → KLUE-MRC(뉴스) 도메인 추가 실습
- 실행 : OpenAI API 키는 Colab 보안 비밀 `OPENAI_KEY` 또는 환경 변수 `OPENAI_API_KEY` 로 주입 (노트북에 하드코딩하지 않음)

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
