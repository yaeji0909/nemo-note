```
code ~/.zshrc
. /opt/homebrew/opt/asdf/libexec/asdf.sh

asdf plugin 설치
asdf plugin add nodejs [https://github.com/asdf-vm/asdf-nodejs.git](https://github.com/asdf-vm/asdf-nodejs.git)

-----------------------

node package 검증
brew install gpg

nodejs 버전 확인
asdf list-all nodejs

nodejs 설치
asdf install nodejs 18.15.0

asdf current nodejs

설치중에 에러 발생시
asdf reshim nodejs

local nodejs 설정
asdf local nodejs 18.15.0

global nodejs 설정
asdf global nodejs 18.15.0

설치된 nodejs version 확인
asdf list nodejs

```