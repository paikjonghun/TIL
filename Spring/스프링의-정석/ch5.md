> 패스트캠퍼스 - 스프링의 정석(남궁성) 강의를 보며 배운 것들 정리

# Ch. 05 Spring MVC로 웹사이트 만들어보기

# 1. 웹 프로젝트 기획하기


# 2. git의 기본 명령어와 원리

### 1. git이란?

- "Git is a free and open source distributed version control system designed to handle everything from small to very larger projects with speed and efficiency."

### 2. 지역 저장소와 원격 저장소

- 깃은 지역 저장소와 원격 저장소가 있다.
- 파일 관리를 할 때, 로컬에서만 이력 관리를 해도 되고, 여러 사람이 함께 관리할 때는 원격 저장소에 올려서 관리를 할 수도 있다.

### 3. Staging Area란?

- git add 파일 이름 ← Staging Area에 파일 추가
- CMD에서 원하는 폴더로 이동해 `git init` 명령어를 실행하면 .git 이라는 Local Repository가 생긴다.
    - email 설정 - `git config --global [user.email](http://user.email) "example@example.com"`
    - name 설정 - `git config --global [user.name](http://user.name) "castello"`
    - 설정 내용 확인 - `git config --global --list`
    - 명령어 확인 - `git --help`
- Staging Area는 장바구니라고 볼 수 있다. Repository에 넣기 전에 담아 놓는 공간. 추가할 수도 있고, 삭제할 수도 있다. `git status` 명령어로 Working Directory와 Staging Area의 상태를 확인할 수 있다.

### 4. Staging Area 관련 명령

- Staging Area에 추가
    - `git add`
- Staging Area에서 삭제
    - `git rm --cached`
    - `git reset`
- Working Directory와 Staging Area의 상태를 확인
    - `git status`
    - 주의 : 파일을 삭제할 때, working directory에서 직접 삭제하지 말고, git을 통해 삭제해야 이력이 관리된다.

### 5. 커밋 관련 명령

- 커밋 생성하기
    - `git commit -m “initial commit”` : 커밋에 대한 설명 적기
    - `git show HEAD` : 최근 커밋에 대한 상세 정보를 보여준다.
    - `git log`
- 커밋 수정하기
    - `git commit --amend` : 최근 커밋의 내역을 볼 수 있다.(커밋 메시지 수정 가능)
    - `git commit --amend -sm "변경된 메시지"` : 커밋 메시지만 수정하고 싶을 때.
    - 커밋을 수정해도 새로운 커밋이 생성된다.
- 취소 커밋 생성하기
    - `git revert HEAD` : 최근 커밋을 취소하는 새로운 커밋을 생성

### 6. 커밋 수정하기

- 최근 커밋에 파일을 추가 또는 삭제하는 방법
    - `git reset --soft`
    - 커밋은 작업 단위로 쪼개는 것이 좋다. 내가 한 작업을 기능 단위로 묶으면 좋다.

### 7. HEAD와 reset

- HEAD : 여러 개의 커밋 중에 현재 내가 보고 있는 커밋이 무엇인지 알게 해주는 것.
- reset —hard : working directory에도 영향을 준다. 다시 되돌리고 싶으면 또 `reset --hard`를 사용.
- 커밋은 자기 이전 커밋을 가리키고 있다.
- 커밋을 수정하면 새로운 커밋이 생성된다! 그 커밋을 HEAD가 가리키게 된다.
- tracked file : git이 관리하는 파일
- untracked file : git이 관리하지 않는 파일

### 8. git log와 커밋 ID

- `git log` : 자세한 로그
- `git log --online` : 간략한 로그
- `git reflog` : HEAD가 가리켰던(ref) 커밋의 이력(log)을 보여줌

### 10. reflog

- HEAD가 이동한 history를 보여줌
    - `git reset --hard HEAD@{4}` : `git checkout c4ceb61` 과 동일
    - `git checkout HEAD@{4}`

### 11. git이 파일을 관리하는 방식

- 파일 이름 대신 object idfㅗ 파일을 관리.
- 커밋 당시의 실제 파일 대신 object id의 목록만 저장.(파일은 별도로 저장 - 파일 목록으로 관리 - snap shot 이라고도 함. zip 파일처럼 파일 전체를 매번 압축하는 것이 아니라 별도로 파일 목록을 만들어서 각 커밋들이 목록에 있는 파일을 참조하도록 한다. 용량 효율적)

### 12. branch

- 커밋을 가리키는 포인터가 두 개 있다.
    - 기본적으로 main이라는 브랜치가 만들어져있고 HEAD가 그것을 가리키게 되어있다.
- dev라는 브랜치를 새로 만들기 : `git branch dev`
- 다른 브랜치로 옮길 때 checkout 명령어 사용
    - main 브랜치로 이동 : `git checkout main`

### 13. fast-forward merge

- main 브랜치를 dev와 합치기(merge)
    - `git merge dev --ff` : fast-forward 는 전진. 앞쪽으로 merge 하는 것
- 새로운 커밋이 안 만들어짐.

### 14. no fast-forward merge

- 무조건 새로운 커밋이 만들어짐.
- main 브랜치로 이동
    - `git merge dev --no-ff`

### 15. merge conflict

- 같은 파일을 서로 다른 브랜치에서 수정하고 합치려고 할 때 충돌(conflict)이 발생.
- main 브랜치를 dev와 merge
    - `git merge dev`

### 16. reset vs. checkout

- reset은 HEAD와 branch가 함께 이동
- checkout은 HEAD만 이동

### 17. git의 브랜칭 전략

- 어떻게 이력(history)을 남길 것인가?
- undo / redo 하기 편리하도록.
- main 브랜치에는 큰 변화만 이력을 남기고, 세부적인 변화는 새로운 브랜치에서 남기는 것이 이력 이동하기 수월하다.
    - master(main) 브랜치에서는 제품을 릴리즈할 때에만 머지.
    - dev 브랜치에서는 개발을 하는데 feature 별로 별도의 브랜치에서 작업
    - hotfix 브랜치에서는 릴리즈하려고 했는데 문제가 발생했을 경우, 바로 고치기 위한 브랜치. 작업 끝나면 master와 dev 브랜치에 머지한다.