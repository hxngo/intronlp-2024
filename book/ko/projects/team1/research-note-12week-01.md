# 12주차 프로젝트 연구일지

## 기본 정보

- **팀명**: MakeMZ
- **프로젝트명**: 유튜브 아이디어 생성기
- **주차**: 12주차

## 팀 구성원 활동 요약

| 이름   | 역할                                 | 주요 활동                                                           | 다음 주 계획                                        |
| ------ | ------------------------------------ | ------------------------------------------------------------------- | --------------------------------------------------- |
| 문재현 | 팀장, NLP 모델 및 시스템 설계        | • 개발 및 오류 수정 계획 <br> • 보고서 자동화 코드                  | • 코드 성능 개선 마무리 <br> • 통합 테스트          |
| 장나겸 | UI/UX, 백엔드                        | • Streamlit을 이용한 ui 생성 테스트 <br> • STT & LLM 통합 작업 진행 | • 보고서 생성 최적화 <br> • 최종 보고서 제출 자동화 |
| 김태호 | 코드 피드백 및 오류 수정             | • 보고서 코드 생성                | • 오류 수정 코드 작성 <br> • 오류 검증 및 통합 작업 |
| 고다희 | 참고자료 및 회의록 작성, 코드 테스트 | • 참고자료 및 회의록 작성 <br> • 파이썬 코드 연습                   | • 추가 기능 테스트 <br> • 코드 리뷰 작성            |
| 김도현 | 코드 테스트 및 오류수정              | • 코드리뷰 및 피드백 <br> • 파이썬 코드 연습                        | • 추가 기능 테스트 <br> • 코드 리뷰 작성            |

## 주간 목표 달성도

| 목표                               | 상태   | 비고                                          |
| ---------------------------------- | ------ | --------------------------------------------- |
| Speech-to-Text 기능 성능 개선      | 완료   | 코드 오류 보완           |
| 보고서 자동 생성 | 완료 | LLM 기반 보고서 자동 생성 기능 추가 보완 필요 |
| 보고서 자동 생성 개선| 진행중 | 내용에 맞게 이미지 생성기능 추가 기능 필요 |
| streamlit 코드 테스트              | 완료 | streamlit 테스트                              |

## 주요 성과 및 결과물

1. 기존 코드 streamlit으로 통합.
   ### 주요 기능을 단계별로 설명

   1. 검색 기능:
   - 사용자가 키워드를 입력하고 영상 길이를 선택할 수 있습니다
   - 영상 길이는 1분 이하부터 1시간 이상까지 다양한 옵션이 있습니다
   - 검색 결과는 썸네일, 제목, 작성자, 재생시간, 조회수를 포함하여 표시됩니다
   2. 영상 선택:
   - 검색 결과에서 원하는 영상들을 선택할 수 있습니다
   - 선택된 영상은 목록으로 관리되며 제거도 가능합니다
   3. 아이디어 생성 프로세스:
       - 선택된 영상들의 오디오를 다운로드 (yt-dlp 사용)
       - Whisper AI를 사용하여 오디오를 텍스트로 변환 (음성-텍스트 변환)
       - OpenAI의 GPT-3.5를 사용하여 변환된 텍스트를 기반으로 프로젝트 아이디어 생성
       - 생성된 아이디어는 SQLite 데이터베이스에 저장됩니다
   4. 데이터 저장:
   - 프로젝트 아이디어는 다음 정보와 함께 데이터베이스에 저장됩니다:
       - 사용된 영상 URL들
       - 영상의 변환된 텍스트(transcript)
       - 생성된 아이디어
       - 생성 시간
2. 사용자 인터페이스:
- Streamlit을 사용한 깔끔한 웹 인터페이스
- 단계별로 구분된 프로세스 (검색 → 선택 → 아이디어 생성)
- 생성된 아이디어를 확인하고 새로운 아이디어를 다시 생성할 수 있는 기능
1. youtube 썸네일 이미지 자동 생성 추가

## 기술적 도전 및 해결 방안

1. 가상환경에서 torch cuda버전에 맞는 torch gpu 버전으로 설치해야 gpu에서 whisper를 사용할 수 있음.

## 학습 내용

1. **[Stream.io](https://streamlit.io/)**
   - 주요 내용: streamlit
   - 적용 방안: ui 코드로서 작성 및 테스트
2. **OpenAI API 통합 및 활용**
   - 주요 내용: LLM 기반 자동 보고서 생성 및 성능 최적화 연구
   - 적용 방안: 프로젝트에 자동화 및 최적화 기능 추가 적용

## 다음 주 계획

1. 기존 아이디어 코드에서 이전 내용을 참고하는 부분을 추가해야함.
2. rag 시스템을 활용하여 보다 사용자에게 맞춘 대답을 나오게 할 예정.

## 팀 미팅 요약

- **일시:** 2024년 11 월 13일
- **참석자:** 문재현, 장나겸, 김태호, 고다희, 김도현
- **주요 논의 사항**:

### 개발

- 콘다 가상환경 yml 파일 부분 추가
- README.md 파일에 추가적인 설명 부분 추가
  - 사용법, conda 가상환경 만드는법.
- streamlit 코드 테스트 및 오류 수정
- 개발 및 피드백 코드 테스트 및 수정


- **결정 사항**:
  1. 보고서에 이미지 넣기
  2. 스트림릿 기반 코드 UI 향상
  3. 기존 아이디어 코드에서 이전 내용을 참고하는 부분을 추가해야함.
  4. rag 시스템을 활용하여 보다 사용자에게 맞춘 대답을 나오게 할 예정.
  5. 시연 영상을 계속해서 업데이트하여 공유하면 서로 이에 대해 피드백 해주기로 함.


## 첨부 자료

- https://www.w3schools.com/
- https://www.boostcourse.org/opencourse
- https://streamlit.io/

---

작성일: 2024-11-13
작성자: 문재현