# 🏐 Volley-RAG: 유소년 배구 특화 AI 코칭 솔루션 통합 가이드

> **"프로의 데이터가 아닌, 명장의 '직관'을 학습하다."**
> 인공지능공학부 졸업작품 프로젝트 팀 가이드라인

## 👥 팀 구성 및 역할 분담 (Team R&R)

### 🏢 Volley-RAG 프로젝트 조직도

| 직무 (Job Title) | 담당자 | 핵심 역할 요약 |
| :--- | :---: | :--- |
| **PM & AI Architect** | 최성원 | 기획 총괄, AI 모델 설계, 시스템 프롬프트 작성 |
| **Backend Engineer** | 허경준 | RAG 파이프라인 구축, DB 연동, 서버 로직 개발 |
| **Frontend & Designer** | 임세현 | 웹 화면(UI) 구현, 발표 자료(PPT/포스터) 제작 |
| **Data Manager & QA** | 배성찬 | 학습 데이터 구축(전처리), 품질 테스트, 시연 데이터 입력 |

---

### 📋 상세 업무 가이드 (Job Description)

#### 1. PM & AI Architect (최성원)
* **Project Management:** 전체 일정 관리, 이슈 트래킹, 지도자 인터뷰 컨택 및 진행.
* **AI Design:** sLLM 모델(Llama-3, Gemma-2 등) 선정 및 시스템 아키텍처 설계.
* **Prompt Engineering:** AI에게 배구 감독의 페르소나(말투, 성격)를 부여하고, 답변의 논리를 설계하는 시스템 프롬프트 작성.

#### 2. Backend Engineer (허경준 & 최성원)
* **RAG Pipeline:** LangChain을 활용하여 문서를 검색(Retrieve)하고 생성(Generate)하는 핵심 로직 구현.
* **Vector DB:** 수집된 텍스트 데이터를 임베딩하여 ChromaDB/FAISS 등에 저장 및 관리.
* **API Dev:** 사용자 입력값을 받아 처리를 수행하고 결과를 반환하는 내부 서버 로직 개발.

#### 3. Frontend & Designer (임세현)
* **UI Development:** Streamlit을 활용하여 태블릿 환경에 최적화된 웹 대시보드 화면 구현.
* **Documentation:** Github `README.md` 관리, 회의록 정리, 결과 보고서 작성.
* **Creative Works:** 중간/최종 발표용 PPT 제작, 포스터 디자인, 시연 영상 편집.

#### 4. Data Manager & QA (배성찬)
* **Data Processing:**
    * 감독 인터뷰 녹음 파일을 텍스트(Script)로 변환 및 정제.
    * 배구 규칙서, 전술 교본 등을 AI 학습용 포맷(`.txt`, `.json`)으로 가공.
* **QA (Quality Assurance):**
    * 개발된 모델에 다양한 경기 상황을 입력하여 답변 오류(Hallucination) 검수.
    * 버그 리포트 작성 및 개발팀 전달.
* **Demo Support:** 실제 경기 테스트나 발표 시연 시, 데이터 입력(Input) 및 오퍼레이팅 담당.

---

### 🔄 협업 워크플로우 (Workflow)
1.  **[Data Manager]**가 원천 데이터(인터뷰, 교본)를 텍스트로 가공하여 **[Backend]**에게 전달.
2.  **[Backend]**가 데이터를 DB에 적재하고 검색 로직을 구현.
3.  **[PM]**이 검색된 데이터를 기반으로 최적의 답변을 하도록 프롬프트를 튜닝.
4.  **[Frontend]**가 사용자가 조작할 수 있는 직관적인 화면을 구현.
5.  **[QA]**가 최종 화면에서 반복 테스트를 수행하며 완성도 점검.


## 📑 목차 (Table of Contents)
1. [프로젝트 개요 (Overview)](#1-프로젝트-개요-overview)
2. [Action 1: 지도자 인터뷰 가이드](#2-action-1-지도자-인터뷰-가이드-rag-데이터-구축용)
3. [Action 2: 데이터 수집 프로토콜](#3-action-2-데이터-수집-프로토콜-입력-변수)
4. [Developer: 시스템 아키텍처 & 폴더 구조](#4-developer-시스템-아키텍처--폴더-구조)
5. [협업 규칙 (Collaboration Rules)](#5-협업-규칙-collaboration-rules)

---

## 1. 프로젝트 개요 (Overview)

본 프로젝트는 데이터 장비가 부족한 아마추어(중·고등학교) 배구 환경을 위한 **RAG(검색 증강 생성) 기반의 AI 감독 시스템**입니다. 
현직 지도자의 경험과 노하우를 학습한 AI가 경기 상황에 맞춰 실시간으로 전술을 지시하고, 선수의 멘탈까지 케어하는 것을 목표로 합니다.

### 🎯 핵심 목표
- **Human-Centered AI:** 수치 데이터보다 '지도자의 철학'과 '직관'을 우선시합니다.
- **Real-World Test:** 실제 중학교 배구부와의 경기를 통해 **[인간 감독 vs AI 감독]**의 퍼포먼스를 검증합니다.
- **Tech Stack:** sLLM (Llama-3/Gemma-2), RAG (LangChain, ChromaDB), Streamlit

---

## 2. [Action 1] 지도자 인터뷰 가이드 (RAG 데이터 구축용)

**목적:** AI의 '판단 로직(Brain)'을 만들기 위해 감독님의 암묵지를 데이터화합니다.
**준비물:** 녹음기(필수), 선물(음료수), 질문지 출력물

### Q1. 평가 기준 (Threshold Setting)
> "AI가 '잘했다/못했다'를 판단하는 기준을 잡기 위함입니다."

1. **핵심 지표:** "감독님, 중·고등학생 경기 기록 중 승패에 직결되는 가장 중요한 지표 2가지만 꼽는다면 무엇인가요? (예: 서브 범실, 리시브 정확도)"
2. **칭찬 기준:** "학생들의 공격 성공률이나 리시브가 대략 몇 % 정도 되면 '아주 훌륭하다'고 칭찬해 주시나요?"

### Q2. 상황별 대처 (Scenario Logic)
> "특정 상황에서 AI가 어떤 작전을 내야 할지 규칙을 만듭니다."

1. **위기 관리:** "3점 연속 실점해서 분위기가 처졌을 때, 작전 타임에서 **전술 지시**를 먼저 하시나요, **멘탈 케어**를 먼저 하시나요?"
2. **약점 공략:** "상대 팀 리시브가 흔들릴 때, 우리 팀 서브 주자에게 어떤 구체적인 사인을 주시나요? (예: 목적타, 짧은 서브)"
3. **세터 코칭:** "우리 팀 세터가 긴장해서 토스 실수가 잦을 때, 가장 단순하게 어떤 지시를 내리시나요?"

### Q3. 페르소나 (Persona)
1. **말투 설정:** "선수들이 실수했을 때 따끔하게 혼내시는 편인가요, 아니면 격려 위주이신가요? (AI 말투 설정용)"

---

## 3. [Action 2] 데이터 수집 프로토콜 (입력 변수)

**[⚠️ 매우 중요]** 우리는 고속 카메라 같은 정밀 장비가 없습니다. 
모든 데이터는 매니저가 눈으로 보고 판단 가능한 **'3등급제'**로 단순화하여 입력합니다.

### ✅ 3등급 입력 규칙표

| 항목 (Category) | 등급 코드 | 판단 기준 (Description) |
| :--- | :---: | :--- |
| **서브 (Serve)** | `S` (Strong) | 강하고 날카로움 (상대 리시브 흔들림) |
| | `N` (Normal) | 평범하게 넘어감 (랠리 시작) |
| | `X` (Miss) | 네트 걸림 / 아웃 (실점) |
| **리시브 (Receive)** | `A` (Perfect) | 세터 머리 위로 정확히 배달 |
| | `B` (High) | 일단 띄워놓음 (오픈 공격 강제) |
| | `C` (Bad) | 바로 넘어가거나 실점 (공격 불가) |
| **공격 (Attack)** | `O` (Success) | 득점 성공 |
| | `K` (Keep) | 수비에 걸리거나 연타 처리 (랠리 지속) |
| | `X` (Fail) | 범실 (공격 아웃/네트) |

> **Note:** 경기장에서는 오직 위 코드(S, N, X...)로만 데이터를 기록합니다. (km/h 사용 금지)

---

## 4. [Developer] 시스템 아키텍처 & 폴더 구조

### 🏗️ Architecture

mermaid
graph LR
    A[경기 상황 입력 (태블릿)] -->|Query| B(RAG 검색 엔진)
    B -->|Retrieve| C{지식 베이스 DB}
    C -->|Context| D[sLLM AI 모델]
    D -->|Generation| E[실시간 코칭 출력]

Volley-RAG/
├── 📂 data/                 # 데이터 저장소
│   ├── 📄 raw_interview.txt   # [보안] 감독님 인터뷰 녹취록
│   ├── 📄 volleyball_rule.pdf # 배구 규정집
│   └── 📊 prompt_samples.json # 프롬프트 테스트용 예제
├── 📂 src/                  # 소스 코드
│   ├── 📂 rag/                # RAG 파이프라인 (Vector DB 구축)
│   ├── 📂 llm/                # sLLM 모델 로딩 및 추론 엔진
│   └── 📂 app/                # Streamlit 웹 화면 코드
├── 📂 docs/                 # 기획서 및 발표 자료
├── 📄 requirements.txt      # 필요 라이브러리 목록
└── 📄 README.md             # 프로젝트 설명서

## 5. 협업 규칙 (Collaboration Rules)

팀원 간의 코드 충돌을 방지하고, 프로젝트의 보안을 유지하기 위한 절대 규칙입니다.

### 🐙 Git Flow 전략
1.  **Main Branch:** 배포 가능한 상태(Production Ready)만 유지합니다. **절대 직접 Push 금지.**
2.  **Feature Branch:** 기능 개발 시 `main`에서 브랜치를 따서 작업합니다.
    - 네이밍 규칙: `feature/기능명` (예: `feature/rag_pipeline`, `feature/ui_design`)
3.  **Pull Request (PR):** 작업이 끝나면 PR을 날리고, 팀원 1명 이상의 리뷰(Approve)를 받아야 Merge 합니다.

### 📝 Commit Message Convention
- `[FEAT]` : 새로운 기능 추가
- `[FIX]` : 버그 수정
- `[DOCS]` : 문서 수정 (README 등)
- `[REFACTOR]` : 코드 리팩토링 (기능 변화 없음)

### 🔒 Security (보안 필수)
- **.gitignore 설정:** 감독님 인터뷰 녹음 파일(`*.mp3`, `*.wav`), 녹취 텍스트(`raw_data.txt`), API Key 파일 등은 절대 깃허브에 올리지 않습니다.
- **개인정보:** 학생들의 실명이나 민감한 정보는 `Player_A`, `Player_B` 등으로 익명화하여 처리합니다.

---

## 6. [Challenge] 영상 분석 확장 (Advanced Features)

**"텍스트로 코칭하고, 영상으로 증명한다."**
기본 RAG 시스템이 안정화된 후, 추가 점수(Bonus)를 위해 도입하는 하이브리드 기능입니다.

### 📡 A. 히트맵(Heatmap) 시각화
전술 지시의 근거를 시각적으로 제시하기 위해, 경기장 내 선수들의 밀집도를 분석합니다.
* **Tech:** YOLOv8 (객체 탐지) + OpenCV (좌표 변환)
* **Goal:** "상대 수비가 좌측에 쏠려있음"을 텍스트가 아닌 **빨간색 히트맵 이미지**로 보여줌.

### 🔢 B. 점수판 자동 인식 (Auto Scoreboard)
매니저의 입력 수고를 덜기 위해 점수판을 자동으로 읽어옵니다.
* **Tech:** EasyOCR or Tesseract
* **Goal:** 점수 변동 시 자동으로 RAG 시스템의 'Current Score' 변수 업데이트.

> **Note:** 위 기능은 'Optional'이며, 핵심 기능(RAG) 구현이 완료된 후 진행합니다.

---

## 7. 프로젝트 로드맵 (Roadmap)

학부 졸업작품 일정에 맞춘 개발 단계입니다.

### 📅 Phase 1: 기획 및 데이터 구축 (Weeks 1-3)
- [x] 지도자 섭외 및 인터뷰 진행 (녹음/녹취)
- [ ] 데이터 프로토콜 확정 (3등급제)
- [ ] RAG 지식 베이스(Knowledge Base) 문서화

### 📅 Phase 2: 코어 엔진 개발 (Weeks 4-8)
- [ ] LangChain 기반 RAG 파이프라인 구축
- [ ] ChromaDB 벡터 저장소 연동
- [ ] sLLM (Llama-3/Gemma-2) 프롬프트 엔지니어링 (페르소나 적용)

### 📅 Phase 3: 인터페이스 및 통합 (Weeks 9-12)
- [ ] Streamlit 웹 대시보드 구현 (태블릿 터치 최적화)
- [ ] 텍스트 음성 변환 (TTS) 연동
- [ ] [Challenge] 영상 분석 모듈 테스트

### 📅 Phase 4: 필드 테스트 및 발표 (Weeks 13-16)
- [ ] 실제 중학교 연습경기 투입 및 데이터 수집
- [ ] 인간 감독 vs AI 감독 비교 분석 리포트 작성
- [ ] 최종 발표 및 시연
