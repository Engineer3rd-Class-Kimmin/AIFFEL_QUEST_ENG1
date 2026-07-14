# GPT-1 학습훈련 기록

`gpt1_generative_pretraining.ipynb`를 CPU 환경에서 처음부터 끝까지 실행한 결과를 보존합니다.

## 실행 환경

- TensorFlow CPU `2.15.0`
- SentencePiece `0.2.0`
- 사전학습 5 epoch
- 분류 미세조정 20 epoch
- 실행 시간 16초
- 코드 셀 14/14 실행, 오류 출력 0건

## 주요 결과

| 항목 | 관측값 |
|---|---:|
| 사전학습 `L1` | `5.596127 → 5.034127` |
| 분류 미세조정 `L2` | `0.977593 → 0.002342` |
| 학습 데이터 정확도 | `1.000` |
| 8문장 held-out 정확도 | `0.750` |
| held-out 긍정 recall | `0.750` |
| held-out 부정 recall | `0.750` |
| causal prefix 최대 차이 | `0.0` |
| 서로 다른 suffix 최대 차이 | `2.786854` |
| padding LM loss 차이 | `0.0` |
| 자동 검사 | `18/19` 통과 |

기존 긍정 probe 한 문장은 부정으로 오분류되었습니다.

```text
입력: 연출이 훌륭하고 아주 재미있는 영화였다.
P(negative)=0.950174
P(positive)=0.049826
```

작은 교육용 데이터와 모델의 일반화 한계로 기록하며, 실패 결과를 제거하지 않았습니다.

## 파일

- [`gpt1_executed.ipynb`](gpt1_executed.ipynb): 모든 셀 출력이 포함된 실행 노트북
- [`gpt1_executed.html`](gpt1_executed.html): 브라우저에서 바로 확인하는 실행 결과
- [`test_report.md`](test_report.md): assertion별 기대값·관측값·통과 여부
- [`metrics.json`](metrics.json): 손실, 정확도, 확률, 마스킹 차이, tensor shape
- [`checks.json`](checks.json): 19개 자동 assertion의 boolean 결과
- [`result_summary.json`](result_summary.json): 셀 실행 및 검사 결과 요약
- [`execution_summary.txt`](execution_summary.txt): nbconvert 종료 상태와 실행 시간
- [`nbconvert.log`](nbconvert.log): 전체 실행 명령 로그
- [`notebook_outputs.txt`](notebook_outputs.txt): 노트북 텍스트 출력 모음
- [`images/`](images): 학습 손실, 분류 결과, 마스킹 검사 시각화

학습된 checkpoint와 SentencePiece 파일은 노트북 실행 시 `artifacts/gpt1` 아래에 다시 생성되므로 저장소에는 포함하지 않았습니다. 실행 당시 checkpoint 크기 `1,814,576` bytes는 `metrics.json`에 기록되어 있습니다.

## 시각화

![학습 손실](images/training_loss.png)

![분류 probe 결과](images/classification_probe.png)

![런타임 assertion 요약](images/runtime_assertions.png)
