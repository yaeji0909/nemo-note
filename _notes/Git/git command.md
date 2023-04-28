### 🚀 git pull 취소하고 되돌리기

(ex.git pull origin main)

```text
git reset --hard ORIG_HEAD
```

위 코드로 간단하게 깃 풀 취소가 가능합니다.

### 🔻 git merge 취소하고 되돌리기

```text
git reset --merge ORIG_HEAD
```

### [](https://2vup.com/git-cancel/#-git-commit-%EC%B7%A8%EC%86%8C%ED%95%98%EA%B3%A0-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0)📮 git commit 취소하고 되돌리기

```text
git reset --hard HEAD
```

한 단계 앞 commit이나 commit을 실행하기 전 상태로 되돌리는 것이 가능합니다.

### [](https://2vup.com/git-cancel/#-git-add-%EC%B7%A8%EC%86%8C%ED%95%98%EA%B3%A0-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0)➕ git add 취소하고 되돌리기

```text
git reset HEAD
```