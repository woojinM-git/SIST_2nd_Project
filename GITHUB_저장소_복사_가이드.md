# GitHub 저장소 복사 가이드

다른 사용자의 GitHub 저장소를 자신의 계정으로 복사하는 방법입니다.

---

## 방법 1: 새 저장소로 복사 (권장)

### Step 1: GitHub에서 새 저장소 생성

1. GitHub에 로그인
2. 우측 상단의 **"+"** 버튼 클릭 → **"New repository"** 선택
3. 저장소 설정:
   - **Repository name**: 원하는 이름 입력 (예: `SIST_2nd_Project`)
   - **Description**: 설명 입력 (선택사항)
   - **Visibility**: 
     - **Public**: 공개 저장소
     - **Private**: 비공개 저장소
   - **⚠️ 중요**: **"Initialize this repository with a README"** 체크 해제
   - **⚠️ 중요**: **"Add .gitignore"** 선택 안 함
   - **⚠️ 중요**: **"Choose a license"** 선택 안 함
4. **"Create repository"** 버튼 클릭

### Step 2: 로컬 저장소의 원격 저장소 변경

현재 원격 저장소를 제거하고 새로운 저장소를 추가합니다.

#### 방법 A: 원격 저장소 URL 변경 (기존 origin 유지)

```bash
# 현재 원격 저장소 확인
git remote -v

# 원격 저장소 URL을 새 저장소로 변경
git remote set-url origin https://github.com/당신의사용자명/저장소이름.git

# 변경 확인
git remote -v
```

#### 방법 B: 원격 저장소 제거 후 새로 추가

```bash
# 기존 원격 저장소 제거
git remote remove origin

# 새 원격 저장소 추가
git remote add origin https://github.com/당신의사용자명/저장소이름.git

# 확인
git remote -v
```

### Step 3: 모든 브랜치와 커밋 히스토리 푸시

```bash
# 모든 브랜치 푸시
git push -u origin --all

# 모든 태그도 함께 푸시 (선택사항)
git push -u origin --tags
```

---

## 방법 2: Fork 사용 (원본과 연결 유지)

원본 저장소와의 연결을 유지하면서 복사하려면 Fork를 사용합니다.

### Step 1: GitHub에서 Fork

1. 원본 저장소 페이지로 이동: `https://github.com/Zerolight90/SIST_2nd_Project`
2. 우측 상단의 **"Fork"** 버튼 클릭
3. Fork 대상 계정 선택
4. Fork 완료

### Step 2: 로컬 저장소 원격 설정 변경

Fork한 저장소를 origin으로 설정하고, 원본 저장소는 upstream으로 추가:

```bash
# 기존 origin 제거
git remote remove origin

# Fork한 저장소를 origin으로 설정
git remote add origin https://github.com/당신의사용자명/SIST_2nd_Project.git

# 원본 저장소를 upstream으로 추가 (선택사항)
git remote add upstream https://github.com/Zerolight90/SIST_2nd_Project.git

# 확인
git remote -v
```

### Step 3: 푸시

```bash
# 모든 브랜치 푸시
git push -u origin --all

# 태그 푸시 (선택사항)
git push -u origin --tags
```

---

## 실전 예제 (현재 프로젝트 기준)

### 현재 상황
- 원본 저장소: `https://github.com/Zerolight90/SIST_2nd_Project.git`
- 현재 브랜치: `WOOJIN`
- 새로 추가된 파일: `.github/workflows/ci.yml`, `GITHUB_ACTIONS_CI_CD_GUIDE.md`

### 실행 순서

#### 1. 새 파일 커밋 (선택사항)

```bash
# 새로 추가된 파일 스테이징
git add .github/
git add GITHUB_ACTIONS_CI_CD_GUIDE.md

# 커밋
git commit -m "Add GitHub Actions CI/CD pipeline"
```

#### 2. GitHub에서 새 저장소 생성
- 웹 브라우저에서 GitHub 접속
- 새 저장소 생성 (README, .gitignore, license 추가하지 않기)

#### 3. 원격 저장소 변경

```bash
# 원격 저장소 URL 변경
git remote set-url origin https://github.com/당신의사용자명/SIST_2nd_Project.git
```

#### 4. 푸시

```bash
# 모든 브랜치 푸시
git push -u origin --all

# 태그가 있다면
git push -u origin --tags
```

---

## 주의사항

### 1. 인증 문제

GitHub에서 2021년 8월부터 패스워드 인증이 비활성화되었습니다.

#### 해결 방법 A: Personal Access Token (PAT) 사용

1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. **"Generate new token (classic)"** 클릭
3. 권한 선택: `repo` (전체 권한)
4. 토큰 생성 후 복사
5. 푸시 시 패스워드 대신 토큰 사용

```bash
# 푸시 시 사용자명과 토큰 입력
Username: 당신의사용자명
Password: 생성한토큰
```

#### 해결 방법 B: SSH 키 사용 (권장)

1. SSH 키 생성 (이미 있다면 생략)
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

2. SSH 키를 GitHub에 등록
   - GitHub → Settings → SSH and GPG keys → New SSH key
   - 공개 키 내용 복사하여 등록

3. 원격 저장소 URL을 SSH 형식으로 변경
```bash
git remote set-url origin git@github.com:당신의사용자명/저장소이름.git
```

### 2. 브랜치 보호 규칙

원본 저장소에 브랜치 보호 규칙이 있다면, 새 저장소에서는 자동으로 해제됩니다.

### 3. 커밋 히스토리

- `--all` 옵션을 사용하면 모든 브랜치와 커밋 히스토리가 그대로 복사됩니다.
- 원본 저장소의 커밋 작성자 정보도 그대로 유지됩니다.

### 4. 라이선스 확인

원본 저장소의 라이선스를 확인하고 준수해야 합니다.

---

## 문제 해결

### 문제 1: "remote origin already exists" 오류

```bash
# 기존 origin 제거 후 다시 추가
git remote remove origin
git remote add origin https://github.com/당신의사용자명/저장소이름.git
```

### 문제 2: "Permission denied" 오류

- Personal Access Token 사용 확인
- 또는 SSH 키 설정 확인

### 문제 3: "refusing to merge unrelated histories" 오류

GitHub에서 README를 추가한 경우 발생할 수 있습니다.

```bash
# 강제 병합 (주의: 원본 저장소에 영향 없음)
git push -u origin --all --force
```

또는:

```bash
# unrelated histories 허용
git pull origin main --allow-unrelated-histories
git push -u origin --all
```

### 문제 4: 특정 브랜치만 푸시하고 싶은 경우

```bash
# 현재 브랜치만 푸시
git push -u origin WOOJIN

# 특정 브랜치 푸시
git push -u origin 브랜치이름
```

---

## 체크리스트

- [ ] GitHub에서 새 저장소 생성 (README, .gitignore, license 추가 안 함)
- [ ] 로컬에서 원격 저장소 URL 변경 확인
- [ ] Personal Access Token 또는 SSH 키 설정
- [ ] 모든 브랜치 푸시 완료
- [ ] GitHub에서 저장소 확인
- [ ] 브랜치 및 커밋 히스토리 확인

---

## 빠른 참조 명령어

```bash
# 원격 저장소 확인
git remote -v

# 원격 저장소 URL 변경
git remote set-url origin https://github.com/사용자명/저장소명.git

# 모든 브랜치 푸시
git push -u origin --all

# 태그 푸시
git push -u origin --tags

# 특정 브랜치만 푸시
git push -u origin 브랜치이름
```

---

## 다음 단계

저장소 복사 후:

1. **README.md 작성**: 프로젝트 설명 추가
2. **.gitignore 확인**: 불필요한 파일 제외 확인
3. **라이선스 추가**: 적절한 라이선스 파일 추가
4. **CI/CD 설정**: GitHub Actions 워크플로우 확인
5. **협업자 초대**: 필요시 다른 사용자 초대

---

## 참고 자료

- [GitHub 저장소 생성 가이드](https://docs.github.com/en/get-started/quickstart/create-a-repo)
- [Git 원격 저장소 관리](https://git-scm.com/book/ko/v2/Git의-기초-리모트-저장소)
- [Personal Access Token 생성](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

