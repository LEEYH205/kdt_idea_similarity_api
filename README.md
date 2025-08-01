# 아이디어 유사도 측정 API

창업 아이디어의 의미적 유사도를 측정하고, 사용자 피드백(좋아요/싫어요)을 반영한 지능형 추천 시스템입니다.

## 🚀 빠른 시작

### 설치

```bash
# Git에서 직접 설치
pip install git+https://github.com/LEEYH205/kdt_idea_similarity_api.git

# 또는 로컬에서 설치
git clone https://github.com/LEEYH205/kdt_idea_similarity_api.git
cd idea-similarity-api
pip install -e .
```

### API 서버 실행

```bash
# 기본 포트(8000)로 실행
idea-api

# 특정 포트로 실행
idea-api --port 8080

# 특정 호스트로 실행
idea-api --host 0.0.0.0 --port 8000
```

### API 사용 예시

```python
import requests

# 유사 아이디어 검색
response = requests.post("http://localhost:8000/search", 
                        json={
                            "query": "AI 반려동물 훈련 서비스",
                            "top_k": 5,
                            "use_popularity": True,
                            "min_similarity": 0.3
                        })
results = response.json()

# 새로운 아이디어 추가
response = requests.post("http://localhost:8000/add-idea", 
                        json={
                            "idea_id": "new_001",
                            "title": "AI 반려동물 훈련사",
                            "body": "인공지능을 활용한 반려동물 행동 분석 서비스",
                            "좋아요": 25,
                            "싫어요": 5
                        })
result = response.json()

# 통계 정보 조회
response = requests.get("http://localhost:8000/statistics")
stats = response.json()
```

## 📋 API 엔드포인트

### 1. 헬스 체크
- **GET** `/health`
- 서버 상태 및 모델 정보 확인

### 2. 유사 아이디어 검색
- **POST** `/search`
- **파라미터**: `query`, `top_k`, `use_popularity`, `min_similarity`

### 3. 새로운 아이디어 추가
- **POST** `/add-idea`
- **파라미터**: `idea_id`, `title`, `body`, `좋아요`, `싫어요`

### 4. 통계 정보
- **GET** `/statistics`
- 데이터셋 통계 정보 조회

### 5. 아이디어 목록
- **GET** `/ideas`
- **파라미터**: `limit`, `offset`, `sort_by`

### 6. 특정 아이디어 조회
- **GET** `/ideas/{idea_id}`
- 특정 아이디어 상세 정보

## 🔧 설정

환경 변수로 설정 가능:
- `IDEA_API_HOST`: 서버 호스트 (기본값: 0.0.0.0)
- `IDEA_API_PORT`: 서버 포트 (기본값: 8000)

## 📦 패키지 구조

```
idea-similarity-api/
├── idea_similarity_api/
│   ├── __init__.py
│   ├── core.py              # 핵심 유사도 측정 엔진
│   ├── api_server.py        # FastAPI 서버
│   └── data/                # 아이디어 데이터
├── setup.py
├── requirements.txt
└── README.md
```

## 🤝 팀원 사용법

### 1. 설치
```bash
pip install git+https://github.com/LEEYH205/kdt_idea_similarity_api.git
```

### 2. 서버 실행
```bash
idea-api --port 8000
```

### 3. API 호출
```python
import requests

# 예시: AI 관련 아이디어 검색
url = "http://localhost:8000/search"
data = {
    "query": "AI 기반 서비스",
    "top_k": 5,
    "use_popularity": True,
    "min_similarity": 0.3
}

response = requests.post(url, json=data)
results = response.json()

for idea in results["results"]:
    print(f"제목: {idea['title']}")
    print(f"유사도: {idea['similarity_score']}")
    print(f"최종점수: {idea['final_score']}")
    print(f"좋아요: {idea['likes']}, 싫어요: {idea['dislikes']}")
    print("---")
```

## 🎯 주요 기능

### 1. **의미적 유사도 측정**
- 한국어 SBERT 모델 사용 (`jhgan/ko-sbert-sts`)
- FAISS를 통한 고속 벡터 검색
- 의미적 유사도와 키워드 매칭 결합

### 2. **사용자 피드백 반영**
- 좋아요/싫어요 비율 기반 인기도 점수
- 유사도와 인기도를 결합한 최종 점수
- 인기도 가중치 조절 가능 (기본값: 20%)

### 3. **실시간 아이디어 추가**
- 새로운 아이디어 즉시 검색 가능
- 모델 상태 자동 업데이트
- 배치 처리 지원

### 4. **고급 검색 기능**
- 최소 유사도 임계값 설정
- 인기도 점수 포함/제외 선택
- 결과 수 조절 가능

## 📊 성능 특징

### 검색 성능
- **FAISS**: 밀리초 단위 고속 검색
- **Sentence Transformers**: 의미적 유사도 측정
- **한국어 최적화**: `jhgan/ko-sbert-sts` 모델 사용

### 확장성
- **실시간 추가**: 새 아이디어 즉시 검색 가능
- **모델 저장**: 학습된 모델 상태 저장/로드
- **배치 처리**: 대량 데이터 처리 지원

## 🚀 향후 개선 방향

### 1. **고급 기능**
- [ ] 카테고리별 필터링
- [ ] 시계열 인기도 변화 추적
- [ ] 협업 필터링 추가

### 2. **성능 최적화**
- [ ] 벡터 압축 (PQ, SQ)
- [ ] GPU 가속 지원
- [ ] 캐싱 레이어 추가

### 3. **사용자 경험**
- [ ] 웹 인터페이스
- [ ] 실시간 추천
- [ ] 개인화 설정

## 🐛 문제 해결

### 일반적인 오류

#### 1. **모델 로딩 실패**
```bash
# 의존성 재설치
pip install --upgrade sentence-transformers
```

#### 2. **메모리 부족**
```bash
# 배치 크기 조정
# core.py에서 BATCH_SIZE 수정
```

#### 3. **API 연결 실패**
```bash
# 서버 상태 확인
curl http://localhost:8000/health
```

## 📝 라이선스



## 🤝 기여하기

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

---

**개발자**: LEEYH205 ejrdkachry@gmail.com

**버전**: 1.0.0  

**최종 업데이트**: 데이터 : 2024년 12월
