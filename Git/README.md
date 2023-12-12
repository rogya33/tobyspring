# GIT
---
## GIT 시작
## 1. GIT의 기초
   * 스냅샷
     Subversion과 Subversion 비슷한 놈들과 Git의 가장 큰 차이점은 데이터를 다루는 방법에 있다.
     큰 틀에서 봤을 때 VCS 시스템 대부분은 관리하는 정보가 파일들의 목록이다.
     CVS, Subversion, Perforce, Bazaar 등의 시스템은 각 파일의 변화를 시간순으로 관리하면서 파일들의 집합을 관리한다
     (보통 델타 기반 버전관리 시스템이라 함).
  * ![](https://git-scm.com/book/en/v2/images/snapshots.png)
    시간순으로 프로젝트의 스냅샷을 저장한다.
    이것이 GIT이 다른 VCS와 구분되는 점이다.

  * 세 가지 상태
    * Committed : 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미한다.
    * Modified : 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것을 말한다.
    * Staged : 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태를 의미한다.
      ![](https://git-scm.com/book/en/v2/images/areas.png)

  Linux : 
  ``` bash
  $ cd /home/user/my_project
  ```
  Mac : 
  ``` bash
  $ cd /Users/user/my_project
  ```
  Windows :
  ``` bash
  $ cd /c/user/my_project
  ```

  ``` bash
  $ git init
  ```
위 코드는 ```.git``` 이라는 하위 디렉토리를 생성한다. 저장소에 필요한 뼈대 파일(Skeleton)이 들어있다.
이 명령만으로는 아직 프로젝트의 어떤 파일도 관리하지 않는다.

GIT이 파일을 관리하기 위해서는 저장소에 파일을 추가하고 커밋해야 한다.
``` bash
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

** 기존 저장소를 Clone 하기
``` bash
$ git clone https://github.com/libgit2/libgit2 mylibgit
```
위의 방법은 libgit2가 아닌 mylibgit라는 디렉토리로 clone하는 방법이고, mylibgit를 빼면 그대로 clone할 수 있다.


---

## 2. 수정 및 저장소에 저장하기
   워킹 디렉토리의 모든 파일은 크게 Tracked(관리대상임)와 Untracked(관리대상이 아님)로 나눈다.
   Tracked 파일은 이미 스냅샷에 포함돼 있던 파일이다. Tracked 파일은 또 Unmodified(수정하지 않음)와 Modified(수정함)
   그리고 Staged(커밋으로 저장소에 기록할) 상태 중 하나이다. 간단히 말하자면 Git이 알고 있는 파일이라는 것이다.

   그리고 나머지 파일은 모두 Untracked 파일이다. Untracked 파일은 워킹 디렉토리에 있는 파일 중
   스냅샷에도 Staging Area에도 포함되지 않은 파일이다. 처음 저장소를 Clone 하면 모든 파일은 Tracked이면서 Unmodified 상태이다.
   파일을 Checkout 하고 나서 아무것도 수정하지 않았기 때문에 그렇다.

   마지막 커밋 이후 아직 아무것도 수정하지 않은 상태에서 어떤 파일을 수정하면 Git은 그 파일을 Modified 상태로 인식한다.
   실제로 커밋을 하기 위해서는 이 수정한 파일을 Staged 상태로 만들고, Staged 상태의 파일을 커밋한다.
   이런 라이프사이클을 계속 반복한다.
   ![](https://git-scm.com/book/en/v2/images/lifecycle.png)

   ### 파일의 상태 확인하기
   ``` bash
   $ git status
   On branch master
   nothing to commit, working directory clean
   ```
   수정이 없는 상태에서 위와 같은 메시지가 출력된다. 저기서, Clone을 한 후 status를 확인하면
   ```Your branch is up-to-date with 'origin/master'.```
   가 추가되는것 같다.

   ### 파일을 새로 추적하기
   ``` bash
   $ git add FILENAME
   ```
   위의 명령어를 수행하면 아래와 같은 메시지를 출력한다
   ```bash
   $ git status
   On branch master
   Your branch is up-to-date with 'origin/master'.
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

       new file:   README
   ```
   ```git add```를 실행하면 Staged 상태가 된다.
   이 때, 또 한번 파일을 수정하고 status를 확인하면 동시에 Staged상태이면서 Unstaged 상태로 나오게 된다.
   ```git add```를 실행하면 GIT은 파일을 바로 Staged 상태로 만든다. 이 시점에서 ```git commit```을 하면,
   명령어를 실행한 시점의 버전이 커밋되는 것이 아닌, 마지막으로 ```git add```된 버전을 커밋하게 되는것이다.
   따라서 최종 수정한 파일을 ```git add```명령어를 통해 다시 최신버전으로 Staged 상태로 만들어줘야 한다.

   ### 파일을 무시하기
   Git이 관리할 필요가 없는 보통 로그 파일이나 빌드 시스템이 자동으로 생성한 파일을 무시하려면
   ```.gitignore``` 파일을 만들고 그 안에 무시할 파일 패턴을 적는다.

   - ```touch ./.gitignore``` 명령어를 통해 생성하고
     ``` plaintext
     .settings/
     bin/
     target/
     .classpath
     .project

     ```
     등을 작성해 넣는다.
   
   ```.gitignore``` 파일에 입력하는 패턴은 아래 규칙을 따른다.
   * 아무것도 없는 라인이나, # 로 시작하는 라인은 무시한다.
   * 표준 Glob 패턴을 사용한다. 이는 프로젝트 전체에 적용된다.
   * 슬래시(/)로 시작하면 하위 디렉토리에 적용되지(Recursivity) 않는다.
   * 디렉토리는 슬래시(/)를 끝에 사용하는 것으로 표현한다.
   * 느낌표(!)로 시작하는 패턴의 파일은 무시하지 않는다.
  
   ### 변경사항 커밋하기
   ```git add```를 통해 Staging Area에 파일을 정리했다면, 이제 ```$ git commit```명령어를 통해 커밋을 할 차례이다.
   Git 설정에 지정된 편집기로 커밋메시지가 실행되고 아래와 같은 텍스트가 자동으로 포함된다
   ``` plaintext
   #
   # Please enter the commit message for your changes. Lines starting
   # with '#' will be ignored, and an empty message aborts the commit.
   # On branch master
   # Your branch is up-to-date with 'origin/master'.
   #
   # Changes to be committed:
   #	new file:   README
   #	modified:   CONTRIBUTING.md
   #
   ~
   ~
   ~
   ".git/COMMIT_EDITMSG" 9L, 283C
   ```
   자동으로 생성되는 커밋 메시지의 첫 라인은 비어 있다. 보통 이곳에 코멘터리를 적고 종료하면 새 커밋이 완성된다.

   ### git diff
   ``` bash
   $ git diff
   diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
   index 8ebb991..643e24f 100644
   --- a/CONTRIBUTING.md
   +++ b/CONTRIBUTING.md
   @@ -65,7 +65,8 @@ branch directly, things can get messy.
   Please include a nice description of your changes when you submit your PR;
   if we have to read the whole diff to figure out why you're contributing
   in the first place, you're less likely to get feedback and have your change
   -merged in.
   +merged in. Also, split your changes into comprehensive chunks if your patch is
   +longer than a dozen lines.

   If you are starting to work on a particular area, feel free to submit a PR
   that highlights your work in progress (and note in the PR title that it's
   ```
   ```CONTRIBUTING.md``` 파일의 변경 사항을 보기 위해 ```git diff```명령어를 사용했다.
   1. diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
      - 비교되는 파일이 ```CONTRIBUTING.md```임을 나타낸다.
        ```a/CONTRIBUTING.md```와 ```b/CONTRIBUTING.md```는 변경 전후의 파일 상태를 나타낸다.
   2. index 8ebb991..643e24f 100644
      - 파일 이전 버전과 새 버전의 해시 값을 보여준다. 8ebb991은 변경 전의 파일을,
        643e24f는 변경 후의 버전을 나타낸다. 100644는 파일의 권한(모드)를 나타낸다.
   3. ---a/CONTRIBUTING.md
      - 변경 전의 파일 상태
   4. +++b/CONTRIBUTING.md
      - 변경 이후의 파일 상태
   5. @@ -65, 7 +65, 8 @@
      - 이 부분은 변경이 발생한 코드의 위치를 나타낸다.
        여기서 -65, 7은 원본 파일에서 65번째 줄부터 7줄이 변경의 대상이었음을,
        +65, 8은 새 파일에서는 65번째 줄부터 8줄이 되었음을 의미한다.
   6. 변경된 내용:
      - 원래 텍스트 :
        ``` bash
        -merged in.
        ```
      - 변경된 텍스트 :
        ``` bash
        +merged in. Also, split your changes into comprehensive chunks if your patch is
        +longer than a dozen lines.
        ```
      
---
## 3. Git branch & Merge
![](https://git-scm.com/book/en/v2/images/basic-branching-1.png)
```master```브랜치에 커밋을 몇 번 하게되면 위와 같은 상태의 히스토리가 생성된다.
이 때,
``` bash
$ git branch iss53
$ git checkout iss53
```
의 명령어를 통해 브랜치를 생성, checkout하게 된다. 히스토리는 다음과 같다.
![](https://git-scm.com/book/en/v2/images/basic-branching-2.png)

```iss53``` 브랜치를 checkout했기 때문에 HEAD는 iss53 브랜치를 가리킨다. 따라서 뭔가 일을 하고 커밋하면
```iss53``` 브랜치가 앞으로 나아간다.
![](https://git-scm.com/book/en/v2/images/basic-branching-3.png)
해당 상태에서 master 브랜치와 iss53 브랜치를 merge하기 전, 해결해야할 핫픽스가 생겼을 때,
```hotfix```라는 브랜치를 만들게 된다
``` bash
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
```
![](https://git-scm.com/book/en/v2/images/basic-branching-4.png)

```hotfix```브랜치를 ```master```브랜치에 합쳐보자.
``` bash
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```
![](https://git-scm.com/book/en/v2/images/basic-branching-5.png)
위의 사진에서, C4 커밋이 C2 커밋에 기반한 브랜치 이기 때문에 Merge 과정 없이 최신 커밋인 C4로 이동하게 되고,
이런 Merge 방식을 "Fast forward" 라고 부른다.

이제 ```hotfix```는 ```master```브랜치에 포함되었고 적용가능한 상태가 되었다면 ```iss53```에서 작업을 이어나간다.
![](https://git-scm.com/book/en/v2/images/basic-branching-6.png)
```master```브랜치와 별개로 진행이 되었기 때문에 위와 같은 사진의 형태가 되고
Merge할 수 있는 수준이 되었다면 합칠 차레이다.

``` bash
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```
![](https://git-scm.com/book/en/v2/images/basic-merging-1.png)
현재 브런치가 가리키는 커밋이 Merge할 브랜치의 조상이 아니므로 ```master```와 ```iss53```브랜치를 Merge 하게 되면 "fast-forward"로 Merge하지 않는다.
![](https://git-scm.com/book/en/v2/images/basic-merging-2.png)
이 경우에는 위와 같은 사진의 형태로, 각 브랜치가 가리키는 커밋 두개와 공통 조상 하나를 사용하여 3-way Merge를 하게된다.

### 충돌
가끔 3-way Merge가 실패할 때도 있는데, Merge 하는 두 브랜치에서 같은 파일의 한 부분을
동시에 수정하게 되면 Git은 해당 부분을 Merge하지 못한다.
``` bash
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```
Git은 자동으로 Merge하지 못했기 때문에 새 커밋이 생기지 않았고, 변경사항의 충돌을
개발자가 해결하도록 둔다.
```plaintext
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```
```index.html```파일에서 충돌이 일어났고, 충돌이 난 부분을 표준 형식에 따라 표시해준다.
```=======``` 위쪽의 내용은 HEAD 버전(보통은 ```master```브랜치)의 내용이고 아래는
```iss53```의 내용이다. 이를 해결하기 위해 둘 중 하나를 고르거나 새로 작성하여야 한다.
해결 후, ```git add```로 저장, commit하게 되면 Merge한 것을 커밋한다.


