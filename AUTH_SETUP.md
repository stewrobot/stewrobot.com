# AUTH_SETUP.md

GitHub 인증(자동 배포/푸시) 설정 가이드

> 목적: 로컬에서 `git push`가 되도록 GitHub 인증을 안전하게 구성합니다.

## 1) Personal Access Token(PAT) 방식 (권장)

1. GitHub에 로그인
2. 우측 상단 Avatar → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic) or Fine-grained tokens**
3. `Generate new token`
4. 최소 권한:
   - `repo`
5. 만료일/유효기간 설정 후 토큰 생성
6. 생성된 토큰을 바로 복사해 보관 (재확인 불가)

## 2) 현재 레포 URL을 HTTPS로 고정

```bash
cd /home/node/.openclaw/workspace/stewrobot.com

git remote -v
# 결과가 https://github.com/stewrobot/stewrobot.com.git 이어야 함
```

## 3) 자격증명 저장(한 번만)

```bash
git config --global --unset-all credential.helper 2>/dev/null || true
git config --global credential.helper store

git remote set-url origin https://github.com/stewrobot/stewrobot.com.git

# 테스트 푸시(요구되면 사용자명/비밀번호 대신 토큰 사용)
# 사용자명: github username
# 비밀번호: 위에서 생성한 토큰

git commit -m "chore: test commit" && git push
```

## 4) 더 안전한 대안: credential.manager (선택)

- 장기적으로는 OS credential manager 사용 권장
- 토큰 노출 위험을 줄이려면 토큰은 env file/secret 관리

## 5) SSH 방식 (원하면 이쪽 추천)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub
```

1. GitHub **Settings > SSH and GPG keys**에 등록
2. remote URL 변경:

```bash
git remote set-url origin git@github.com:stewrobot/stewrobot.com.git
```

3. 연결 테스트:

```bash
ssh -T git@github.com
```

4. 이후 푸시 테스트:

```bash
git push
```

## 6) 배포 확인

- main 브랜치에 푸시하면 GitHub Pages가 자동 배포
- 보통 몇십 초 내 반영 (캐시 따라 수 분 소요 가능)

## 7) 주의

- 토큰은 절대 채팅/코드에 직접 붙여넣지 마세요.
- `.gitignore`에는 시크릿 키를 절대 포함하지 마세요.
- 원하면 내가 다음 단계에서 바로 PAT/SSH 중 하나 골라서 설정까지 진행할게.
