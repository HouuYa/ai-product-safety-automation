# GitHub 포트폴리오 업로드 가이드 (초보자용)

## 방법 1: 웹 브라우저로 업로드 (가장 쉬움)

### Step 1: 파일 다운로드
1. 아래 파일들을 컴퓨터에 다운로드하세요:
   - `/home/claude/ai-recall-automation/` 폴더의 모든 파일

### Step 2: GitHub 웹사이트에서 업로드

1. **GitHub 레포지토리 접속**
   - https://github.com/your-username/ai-recall-automation 으로 이동

2. **파일 업로드**
   - "Add file" 버튼 클릭 → "Upload files" 선택
   - 다운로드한 파일들을 드래그 앤 드롭
   - 또는 "choose your files" 클릭하여 선택

3. **커밋 메시지 작성**
   ```
   Initial commit: AI recall automation portfolio
   ```

4. **"Commit changes" 클릭**

---

## 방법 2: GitHub Desktop 사용 (권장)

### Step 1: GitHub Desktop 설치
1. https://desktop.github.com 에서 다운로드
2. 설치 후 GitHub 계정으로 로그인

### Step 2: 레포지토리 클론
1. GitHub Desktop 열기
2. File → Clone Repository
3. `ai-recall-automation` 선택
4. 로컬 경로 선택 (예: `C:\Users\YourName\Documents\GitHub\ai-recall-automation`)

### Step 3: 파일 복사
1. 다운로드한 파일들을 위 경로에 복사

### Step 4: 커밋 & 푸시
1. GitHub Desktop에서 변경사항 확인
2. Summary 입력: `Initial portfolio commit`
3. "Commit to main" 클릭
4. "Push origin" 클릭

---

## 방법 3: Git 명령어 사용 (고급)

### Step 1: Git 설치
```bash
# Windows: https://git-scm.com/download/win
# Mac: brew install git
# Linux: sudo apt-get install git
```

### Step 2: 레포지토리 클론
```bash
cd ~/Documents
git clone https://github.com/your-username/ai-recall-automation.git
cd ai-recall-automation
```

### Step 3: 파일 추가
```bash
# 파일 복사 후
git add .
git commit -m "Initial commit: AI recall automation portfolio"
git push origin main
```

---

## 폴더 구조 확인

업로드 후 GitHub에서 이렇게 보여야 합니다:

```
ai-recall-automation/
│
├── README.md                   ✅ 메인 소개
├── ARCHITECTURE.md             ✅ 시스템 구조
├── LICENSE                     ✅ 라이센스 (MIT)
│
├── docs/
│   ├── 01-domestic-recall-oecd.md
│   ├── 02-overseas-recall-collection.md
│   ├── 03-marketplace-surveillance.md
│   └── 04-news-monitoring.md
│
├── workflows/
│   ├── domestic-recall-oecd-sample.json
│   ├── overseas-recall-sample.json
│   └── marketplace-surveillance-sample.json
│
└── diagrams/
    └── system-architecture.png
```

---

## 다음 단계

### 1. README 개인화
`README.md` 파일을 열어서 수정:
- LinkedIn URL 업데이트
- 이메일 주소 업데이트
- 개인 사진 추가 (선택사항)

### 2. GitHub Pages 활성화 (선택)
1. Repository Settings → Pages
2. Source: main branch
3. Save
4. https://your-username.github.io/ai-recall-automation 으로 접속 가능

### 3. 링크 공유
- LinkedIn 프로필에 추가
- 이력서에 GitHub 링크 포함
- 면접 시 포트폴리오로 활용

---

## 자주 묻는 질문

### Q1: 파일을 수정하려면?
**A:** GitHub 웹사이트에서 파일 클릭 → 연필 아이콘(Edit) 클릭 → 수정 → Commit changes

### Q2: 실수로 잘못 올렸어요
**A:** 
1. File → 휴지통 아이콘 클릭 → Commit changes
2. 또는 전체 삭제 후 다시 업로드

### Q3: 비공개로 만들 수 있나요?
**A:** Settings → Danger Zone → Change visibility → Private
(단, 포트폴리오는 Public이 좋습니다)

### Q4: README 이미지 추가하려면?
**A:**
```markdown
![시스템 구조](./diagrams/system-architecture.png)
```

---

## 문제 해결

### 업로드 실패 시
1. 파일 크기 확인 (100MB 이하)
2. 파일명에 특수문자 없는지 확인
3. 인터넷 연결 확인

### 한글 깨짐 현상
1. 파일 인코딩을 UTF-8로 변경
2. 메모장 → 다른 이름으로 저장 → 인코딩: UTF-8

---

## 추가 도움말

- GitHub 공식 가이드: https://docs.github.com
- Markdown 문법: https://www.markdownguide.org
- Git 튜토리얼: https://git-scm.com/book/ko/v2

---

**준비 완료!** 이제 팔란티어에 지원할 때 이 포트폴리오 링크를 제출하세요.
