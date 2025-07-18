# 📘 Git TIL - `git restore --staged` vs `git rm --cached`

## 1. 기본 개념 비교

| 명령어 | 역할 | Git 추적 제거 (Untrack) | 워킹 디렉토리 영향 |
|--------|------|--------------------------|---------------------|
| `git rm --cached <file>` | Git 추적 제거 + Staging 제거 | ✅ | ❌ |
| `git restore --staged <file>` | Staging 제거만 (Unstage) | ❌ | ❌ |
| `git restore --staged --source=HEAD <file>` | 이전 커밋 기준으로 Staging 롤백 | ❌ | ❌ |

---

## 2. `git rm --cached` 

### 🔹 목적  
Git이 해당 파일을 **추적하지 않도록 설정**함.  
일반적으로 `.gitignore`에 추가하기 전에 사용함.

### 🔹 특징
- 파일을 Git에서 **완전히 제거(Untrack)** 하지만 워킹 디렉토리에는 **남겨둠**
- 이후 커밋 시 해당 파일은 Git에서 사라짐

### 🔹 예시
```bash
git rm --cached secrets.env
echo "secrets.env" >> .gitignore
```

---

## 3. `git restore --staged` 

### 🔹 목적  
`git add`로 staging한 파일을 **다시 unstage**함.

### 🔹 특징
- **워킹 디렉토리 수정은 그대로** 둠
- Git 추적 유지
- 커밋 대상에서만 제거됨

### 🔹 예시
```bash
git add app.py
git restore --staged app.py
```

---

## 4. `git restore --staged --source=HEAD` 

### 🔹 목적  
Staging 영역을 현재 커밋 상태로 **되돌림(롤백)**함.

### 🔹 특징
- 워킹 디렉토리는 그대로 유지
- 실수로 수정 + add까지 한 파일을 **현재 커밋 기준 상태로 staging만 복원**

### 🔹 예시
```bash
git restore --staged --source=HEAD config.yml
```

---

## 5. Git 버전별 권장 명령어 비교

| 작업 | Git < 2.23 (과거 방식) | Git ≥ 2.23 (현재 권장 방식) |
|------|------------------------|------------------------------|
| Staging 제거 (Unstage) | `git reset <file>` | `git restore --staged <file>` |
| 워킹 디렉토리 롤백 | `git checkout -- <file>` | `git restore <file>` |
| Git 추적 제거 (Untrack) | `git rm --cached <file>` | `git rm --cached <file>` |

### 🔹 설명
- Git 2.23부터 `git restore`, `git switch` 명령이 새로 도입됨
- `checkout`, `reset`은 기능이 많아 헷갈려서, **역할을 나누기 위해 명령어를 분리**함
- `restore`는 파일 롤백/unstage 전용, `switch`는 브랜치 이동 전용

---

## 6. 요약 및 느낀점

- `git restore` 시리즈는 Git UX 개선을 위한 명령어
- **`restore --staged`는 staging 제거만** 담당 → Git 추적 유지
- **Git 추적을 제거하려면 `rm --cached`를 써야 함** → 대체 불가능

💡 *직관성 증대/기능 분리는 쉽지만 강력한 UX 개선책* \
💡 *버전별 커맨드 변화 과정을 트래킹하면서 개발자의 의도를 파악하는 것도 재미있을 듯*