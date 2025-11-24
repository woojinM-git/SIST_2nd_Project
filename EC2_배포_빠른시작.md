# EC2 배포 빠른 시작 가이드

## 5분 안에 EC2 배포 설정하기

---

## Step 1: EC2 인스턴스 준비 (5분)

### 1.1 Tomcat 설치

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install tomcat9 tomcat9-admin
sudo systemctl start tomcat9
sudo systemctl enable tomcat9
```

**Amazon Linux 2:**
```bash
sudo yum install tomcat9-webapps tomcat9-admin-webapps
sudo systemctl start tomcat9
sudo systemctl enable tomcat9
```

### 1.2 배포 디렉토리 생성
```bash
sudo mkdir -p /opt/deploy
sudo chown ec2-user:ec2-user /opt/deploy
# 또는 ubuntu 사용자인 경우
sudo chown ubuntu:ubuntu /opt/deploy
```

### 1.3 보안 그룹 설정
- EC2 콘솔 → 인스턴스 → 보안 그룹
- 인바운드 규칙 추가:
  - SSH (22): GitHub Actions IP 또는 특정 IP
  - HTTP (80): 0.0.0.0/0

---

## Step 2: GitHub Secrets 설정 (2분)

1. GitHub 저장소 → **Settings** → **Secrets and variables** → **Actions**
2. **New repository secret** 클릭하여 다음 3개 추가:

### 필수 Secrets

| Secret 이름 | 설명 | 예시 |
|------------|------|------|
| `EC2_HOST` | EC2 IP 주소 | `13.125.123.45` |
| `EC2_USER` | EC2 사용자명 | `ec2-user` 또는 `ubuntu` |
| `EC2_SSH_KEY` | SSH Private Key 전체 내용 | PEM 파일 내용 복사 |

### SSH 키 가져오기

**Windows PowerShell:**
```powershell
Get-Content C:\path\to\your-key.pem | Out-String
```

**Linux/Mac:**
```bash
cat ~/.ssh/your-key.pem
```

출력된 전체 내용을 복사하여 `EC2_SSH_KEY` Secret에 붙여넣기

---

## Step 3: 코드 푸시 (1분)

```bash
git add .github/workflows/ci.yml
git commit -m "Add EC2 deployment to CI/CD pipeline"
git push origin main
```

---

## Step 4: 배포 확인

1. GitHub 저장소 → **Actions** 탭
2. 워크플로우 실행 확인
3. `deploy` job이 성공했는지 확인
4. 브라우저에서 `http://EC2_IP` 접속하여 애플리케이션 확인

---

## 문제 발생 시

### "Permission denied" 오류
```bash
# EC2에서 실행
sudo chown -R ec2-user:ec2-user /opt/deploy
sudo chown -R ec2-user:ec2-user /var/lib/tomcat9/webapps
```

### Tomcat이 시작되지 않음
```bash
# EC2에서 실행
sudo systemctl status tomcat9
sudo journalctl -u tomcat9 -n 50
```

### 404 오류
- Tomcat 로그 확인: `sudo tail -f /var/log/tomcat9/catalina.out`
- WAR 파일 확인: `ls -la /var/lib/tomcat9/webapps/`

---

## 자세한 가이드는 `EC2_배포_가이드.md` 참고

