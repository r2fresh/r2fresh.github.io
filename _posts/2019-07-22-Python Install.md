---
title: "Python Install"
date: 2019-07-23 11:37:28 -0400
categories: jekyll update
---

# Mac Version

## Install Pyenv

link : [https://github.com/pyenv/pyenv](https://github.com/pyenv/pyenv)

### brew update

>mac에 brew가 설치가 안 되어 있을 경우 참조
>link[https://brew.sh/](https://brew.sh/)

``` bash
$> brew update
```

### Pyenv Install

Python Vesion Manager를 설치

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


