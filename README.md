# 📖 단어 암기장 (Vocab Flashcard App)

모바일 최적화 영단어 암기 웹앱 — GitHub Pages + Google Sheets 연동

---

## 📁 파일 구조

```
vocab-app/
├── index.html       # 메인 앱 (플래시카드, 깜빡이, 퀴즈)
├── words.json       # 단어 데이터 (GAS로 자동 생성)
├── gas_upload.js    # Google Apps Script (시트→JSON→GitHub)
└── README.md
```

---

## 🚀 GitHub Pages 배포

### 1단계 — 리포지토리 생성
1. GitHub에서 새 리포지토리 생성 (예: `vocab-app`)
2. `index.html`, `words.json` 업로드

### 2단계 — GitHub Pages 활성화
1. 리포지토리 → **Settings** → **Pages**
2. Source: `Deploy from a branch`
3. Branch: `main` / `/ (root)` 선택 → **Save**
4. 약 1~2분 후 `https://[username].github.io/vocab-app` 접속 확인

---

## 📊 구글 시트 구조

| A열 (Day) | B열 (영어 단어) | C열 (한글 뜻) |
|-----------|--------------|--------------|
| 1         | apple        | 사과          |
| 1         | banana       | 바나나        |
| 2         | abandon      | 버리다, 포기하다 |

- **1행은 헤더** (자동 스킵됨)
- A열은 반드시 숫자

---

## ⚙️ GAS 자동화 설정

### 1단계 — GAS 프로젝트 생성
1. 구글 시트 열기
2. **확장 프로그램** → **Apps Script**
3. `gas_upload.js` 내용 전체 붙여넣기

### 2단계 — CONFIG 수정
```javascript
const CONFIG = {
  SHEET_ID:      'YOUR_GOOGLE_SHEET_ID',    // 시트 URL의 /d/ 뒤 값
  SHEET_NAME:    'Sheet1',                  // 탭 이름
  GITHUB_TOKEN:  'YOUR_GITHUB_TOKEN',       // 아래 발급 방법 참고
  GITHUB_OWNER:  'your-username',
  GITHUB_REPO:   'vocab-app',
  GITHUB_BRANCH: 'main',
  JSON_PATH:     'words.json',
};
```

### 3단계 — GitHub Token 발급
1. GitHub → **Settings** → **Developer settings**
2. **Personal access tokens** → **Tokens (classic)**
3. **Generate new token** → `repo` 권한 체크 → 생성
4. 발급된 토큰을 `GITHUB_TOKEN`에 붙여넣기

### 4단계 — 실행
- 시트 메뉴 **📚 단어 암기장** → **✅ GitHub에 업로드**
- 또는 GAS 편집기에서 `exportToGitHub` 함수 실행

### 자동 업로드 (선택)
- 시트 메뉴 → **⏰ 자동 업로드 트리거 설정**
- 매일 자정 자동 업로드

---

## 📱 앱 기능

| 모드 | 설명 |
|------|------|
| 🃏 플래시카드 | 탭하면 뒤집힘 · ✅/❌ 분류 · 셔플 |
| ⚡ 깜빡이 모드 | 카운트다운 후 뜻 자동 공개 · 속도 조절 |
| 🎯 퀴즈 모드 | 4지선다 · 즉시 피드백 · 점수 표시 |
| 🔁 틀린 단어만 | ❌ 표시 단어만 모아서 재도전 |

- 🔊 **발음**: Web Speech API (무료, 브라우저 내장)
- ⭐ **진도 저장**: localStorage (기기에 자동 저장)
- 결과 화면에서 틀린 단어 목록 + 재도전 가능

---

## 🔧 words.json 수동 교체 방법 (GAS 없이)

1. 구글 시트 데이터를 아래 형식의 JSON으로 작성
2. `words.json` 파일을 GitHub에서 직접 수정하거나 업로드

```json
[
  {"day": 1, "word": "apple",  "meaning": "사과"},
  {"day": 1, "word": "banana", "meaning": "바나나"},
  {"day": 2, "word": "abandon","meaning": "버리다, 포기하다"}
]
```
