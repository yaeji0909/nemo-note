https://velog.io/@archivvonjang/Git-Commit-Message-Convention
https://kdjun97.github.io/git-github/commit-convention/


### 1. Commit Type

-   커밋의 타입 구성
    
    > type: Subject -> 제목 (한칸 띄우기) body(생략 가능) -> 본문 (한칸 띄우기) footer(생략 가능) -> 꼬리말
    

### 2. Commit Type

-   커밋의 타입 구성
    태그: 제목:(space)제목 으로 :뒤에만 space를 넣는다.


    ```
    ex)
     Feat(navigation)
     Fix(db)
    ```    

### 3. Subject

-   제목은 50글자 이내로 작성한다.
    
-   첫글자는 대문자로 작성한다.
    
-   마침표 및 특수기호는 사용하지 않는다.
    
-   영문으로 작성하는 경우 동사(원형)을 가장 앞에 명령어로 작성한다.
    
-   과거시제는 사용하지 않는다.
    
-   간결하고 요점적으로 즉, 개조식 구문으로 작성한다.
    
    ```
    ex)
    Fixed --> Fix
    Added --> Add
    Modified --> Modify
    ```
    

### 4. Body

-   72이내로 작성한다.
-   최대한 상세히 작성한다. (코드 변경의 이유를 명확히 작성할수록 좋다)
-   어떻게 변경했는지보다 무엇을, 왜 변경했는지 작성한다.

### 5. Footer

-   선택사항
-   issue tracker ID 명시하고 싶은 경우에 작성한다.
-   유형: #이슈 번호 형식으로 작성한다.
-   여러 개의 이슈번호는 쉼표(,)로 구분한다.
-   이슈 트래커 유형은 다음 중 하나를 사용한다.

> Fixes: 이슈 수정중 (아직 해결되지 않은 경우) Resolves: 이슈를 해결했을 때 사용 Ref: 참고할 이슈가 있을 때 사용 Related to: 해당 커밋에 관련된 이슈번호 (아직 해결되지 않은 경우)

```
ex)
Fixes: #45 Related to: #34, #23
```

### 6. Example

**예시 1**

```
Feat: 회원 가입 기능 구현

SMS, 이메일 중복확인 API 개발

Resolves: #123
Ref: #456
Related to: #48, #45
```

**예시 2**

```
feat: Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, preceded
by a single space, with blank lines in between, but conventions
vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789

```

**자주 쓰이는**

```
  Fix : 버그 수정
  Fix my test
  Fix typo in style.css
  Fix my test to return undefined
  Fix error when using my function

  Update : Fix와 달리 원래 정상적으로 동작했지만 보완의 개념
  Update harry-server.js to use HTTPS

  Add
  Add documentation for the defaultPort option
  Add example for setting Vary: Accept-Encoding header in zlib.md

  Remove(Clean이나 Eliminate) : ‘unnecessary’, ‘useless’, ‘unneeded’, ‘unused’, ‘duplicated’가 붙는 경우가 많음
  Remove fallback cache
  Remove unnecessary italics from child_process.md

  Refactor : 리팩토링

  Simplify : Refactor와 유사하지만 약한 수정, 코드 단순화

  Improve : 호환성, 테스트 커버리지, 성능, 검증 기능, 접근성 등의 향상
  Improve iOS's accessibilityLabel performance by up to 20%

  Implement : 코드 추가보다 큰 단위의 구현
  Implement bundle sync status

  Correct : 주로 문법의 오류나 타입의 변경, 이름 변경 등에 사용
  Correct grammatical error in BUILDING.md

  Prevent
  Prevent hello handler from saying Hi in hi.js

  Avoid : Prevent는 못하게 막지만, Avoid는 회피(if 등)
  Avoid flusing uninitialized traces

  Move : 코드나 파일의 이동
  Move function from header to source file

  Rename : 이름 변경
  Rename node-report to report
```
