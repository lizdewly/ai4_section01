
# Git 브랜치 작업 및 Pull Request 가이드

이 문서는 `main` 브랜치의 최신 소스를 가져온 뒤, 개인 작업용 브랜치에서 코드를 수정하고 GitHub의 Pull Request(PR)를 통해 변경 사항을 `main`에 반영하는 과정을 정리한 실습 가이드입니다.

## 1. 현재 브랜치 확인

먼저 현재 어떤 브랜치에서 작업하고 있는지 확인합니다.

```bash
git branch
```

출력 결과에서 `*`가 표시된 브랜치가 현재 사용 중인 브랜치입니다.

## 2. main 브랜치로 이동 및 최신 소스 동기화

새로운 작업을 시작하기 전에 `main` 브랜치로 이동하고, 원격 저장소의 최신 변경 사항을 가져옵니다.

```bash
git switch main
git pull origin main
```

> **주의:** 브랜치를 변경하기 전에 커밋하지 않은 변경 사항이 있다면 `git status`로 확인하고, 필요에 따라 커밋하거나 임시 보관(`git stash`)하세요.

## 3. 개인 작업용 브랜치 생성 및 이동

`main`에서 직접 작업하지 않고, 기능 개발을 위한 별도의 브랜치를 생성합니다.

```bash
git switch -c feature/login
```

위 명령어는 `feature/login` 브랜치를 **생성하는 동시에 해당 브랜치로 이동**합니다.

브랜치가 정상적으로 생성되고 이동되었는지 확인합니다.

```bash
git branch
```

```text
* feature/login
  main
```

## 4. 개인 작업 진행

`feature/login` 브랜치에서 필요한 작업을 수행합니다.

- 기존 소스코드 수정
- 불필요한 파일 또는 코드 삭제
- 새로운 파일과 기능 추가
- 기능 테스트 및 오류 수정

작업 내용은 `main`에 바로 반영되지 않으며, 현재 작업 중인 `feature/login` 브랜치에 반영합니다.

## 5. SourceTree에서 변경 사항 커밋 및 Push

코드 작성 이후에는 **SourceTree의 UI**를 이용해 변경 사항을 원격 저장소에 반영합니다.

1. SourceTree에서 변경된 파일을 확인합니다.
2. 반영할 파일을 스테이징(Stage)합니다.
3. 변경 내용을 설명하는 커밋 메시지를 작성하고 **Commit**합니다.
4. **Push**를 클릭하여 `feature/login` 브랜치를 GitHub 원격 저장소에 업로드합니다.
   - 해당 브랜치를 처음 Push할 때는 원격 브랜치를 생성하거나 추적하도록 설정해야 할 수 있습니다.

> **참고:** Commit은 로컬 저장소에 변경 이력을 기록하는 작업이고, Push는 해당 커밋을 GitHub의 원격 저장소에 업로드하는 작업입니다.

## 6. GitHub에서 Pull Request 생성

Push가 완료되면 GitHub 저장소 페이지로 이동합니다.

새로 업로드한 브랜치에 **Compare & pull request** 버튼이 표시되면 이를 클릭합니다. 표시되지 않는 경우 **Pull requests → New pull request**를 선택합니다.

다음과 같이 브랜치를 지정합니다.

| 항목 | 브랜치 | 의미 |
|---|---|---|
| **base** | `main` | 변경 사항을 반영할 대상 브랜치 |
| **compare** | `feature/login` | 수정 작업이 완료된 브랜치 |

수정된 파일과 변경 내역을 확인하고 PR 제목 및 설명을 작성한 뒤 **Create pull request**를 클릭합니다.

## 7. Pull Request를 main에 병합(Merge)

PR의 변경 내용을 검토한 뒤, 문제가 없다면 아래 순서로 진행합니다.

1. **Merge pull request** 클릭
2. **Confirm merge** 클릭

병합이 완료되면 `feature/login` 브랜치의 변경 사항이 원격 저장소의 `main` 브랜치에 반영됩니다.

> **주의:** 저장소 설정에 따라 코드 리뷰 승인이나 자동화 검사 통과가 필요할 수 있습니다. 병합 버튼이 비활성화되어 있다면 표시된 조건을 먼저 확인하세요.

## 8. 로컬 main 브랜치 동기화 및 작업 브랜치 삭제

GitHub에서 병합한 내용을 로컬 PC에도 반영합니다.

```bash
git switch main
git pull origin main
```

더 이상 사용하지 않는 개인 작업 브랜치는 삭제할 수 있습니다.

```bash
git branch -d feature/login
```

`-d` 옵션은 병합되지 않은 변경 이력이 있을 때 브랜치 삭제를 막는 안전한 옵션입니다.

GitHub의 원격 브랜치도 필요 없다면 PR 병합 완료 화면의 **Delete branch** 버튼을 이용해 삭제할 수 있습니다.

---

## 전체 작업 흐름 요약

| 순서 | 작업 | 위치 / 도구 | 결과 |
|---:|---|---|---|
| 1 | 현재 브랜치와 변경 사항 확인 | 터미널 | 작업 상태 확인 |
| 2 | `main`으로 이동하고 최신 내용 가져오기 | 터미널 | 로컬 `main` 동기화 |
| 3 | 개인 작업 브랜치 생성 및 이동 (`feature/login`) | 터미널 | 독립된 작업 공간 준비 |
| 4 | 코드 수정·추가·삭제 및 테스트 | 편집기 | 작업 브랜치에 변경 사항 생성 |
| 5 | 변경 파일 Stage → Commit → Push | SourceTree | 변경 사항을 GitHub에 업로드 |
| 6 | PR 생성 (`base: main` ← `compare: feature/login`) | GitHub | `main`에 반영할 변경 사항 제안 |
| 7 | 변경 내역 검토 후 Merge | GitHub | PR 변경 사항을 원격 `main`에 병합 |
| 8 | 로컬 `main` 동기화 및 작업 브랜치 정리(선택) | 터미널 / GitHub | 병합 결과 반영 및 사용한 브랜치 정리 |

## 주요 Git 명령어 모음

| 목적 | 명령어 | 설명 |
|---|---|---|
| 현재 브랜치 확인 | `git branch` | `*` 표시가 현재 브랜치입니다. |
| 작업 트리 상태 확인 | `git status` | 커밋되지 않은 변경 사항을 확인합니다. |
| `main`으로 이동 | `git switch main` | 로컬 `main` 브랜치로 전환합니다. |
| 원격 `main` 최신 내용 가져오기 | `git pull origin main` | 원격 `main`의 변경 사항을 가져와 현재 `main`에 반영합니다. |
| 작업 브랜치 생성 및 이동 | `git switch -c feature/login` | `feature/login`을 만들고 해당 브랜치로 전환합니다. |
| 기존 작업 브랜치로 이동 | `git switch feature/login` | 이미 존재하는 작업 브랜치로 전환합니다. |
| 병합된 로컬 브랜치 삭제 | `git branch -d feature/login` | 병합이 끝난 로컬 작업 브랜치를 삭제합니다. |
