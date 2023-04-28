
### `develop` 브랜치로 이동하고 싶을 때

```bash
git switch develop
```

### `develop` 브랜치가 없을 때, 브랜치를 생성 후 이동하고 싶다면

```bash
git switch -c new-branch
```

### 특정 시작 시점으로부터 새 브랜치 생성 후 이동하고 싶다면

```bash
git switch -c new-branch2 <커밋 번호 ex) 515c633a>
```

```bash
git switch -c new-branch2 <특정 브랜치 이름 ex) upstream/main>
```
https://velog.io/@kms8571/git-switch-restore