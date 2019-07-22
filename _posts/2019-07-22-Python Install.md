---
title: "Python Install"
date: 2019-07-23 11:37:28 -0400
categories: jekyll update
---

# Mac Version

## Install Pyenv

mac에 brew가 설치가 안 되어 있을 경우 참조 [https://brew.sh/](https://brew.sh/)

brew update

``` bash
$> brew update
```

Python Vesion Manager를 설치 [https://github.com/pyenv/pyenv](https://github.com/pyenv/pyenv)

``` bash
$> brew install pyenv
```

.bash_profile (or .bashrc) 아래내용 입력
``` bash
$> echo '# pyenv Basic Github Checkout Setting (1,2,3)' >> ~/.bash_profile
$> echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
$> echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
$> echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile
```

## Install pyenv-virtualenv

Project마다 가상환경을 만들어서 버전의 충돌을 줄이고 버전에 따른 프로젝트를 진행하도록 Virtualenv 설치

### pyenv-virtualenv install

```bash
$> brew install pyenv-virtualenv
```

.bash_profile (or .bashrc) 아래내용 입력
``` bash
$> echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$> echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
```

## Install autoenv

Python으로 된 프로젝트 폴더로 입력하면 자동으로 virtualenv 환경을 설정

```bash
$ brew install autoenv
```

.bash_profile (or .bashrc) 아래내용 입력
```bash
$ echo 'source $(brew --prefix autoenv)/activate.sh' >> ~/.bash_profile
```