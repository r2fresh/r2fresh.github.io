---
title: Mac and Ubuntu에 zsh Setting
date: 2020-04-22 02:40:28 -0400
categories: jekyll update
---

## Mac

### zsh 설치

```bash
# zsh 설치
> brew install zsh

# 설치경로 확인
> which zsh
#=> /usr/local/bin/zsh

# 기본 sh 변경
> chsh -s $(which zsh)
```

### oh my zsh 설치

```bash
> sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Theme Change

`~/.zshrc` 파일 안에서 `ZSH_THEME`를 찾아서 `agnoster`로 변경

```bash
> vim ~/.zshrc
```

```bash
# AS-IS
ZSH_THEME="robbyrussell"

# TO-BE
ZSH_THEME="agnoster"
```

### Powerline font 설치

먼저 아래와 같이 Powerline font를 설치한다

```bash
# clone
> git clone https://github.com/powerline/fonts.git --depth=1

# install
> cd fonts
> ./install.sh

# clean-up a bit
> cd ..
> rm -rf fonts
```

### Command Tool Setting

**기본 터미널을 사용할 경우**

터미널 앱에서 위의 `터미널>환경설정` 버튼을 클릭하거나, `command+,`을 클릭하여 설정화면으로 이동

- 폰트는 `Source Code Pro for Powerline`으로 선택하고 폰트 사이즈는 14px로 설정
- `ANSI 색상' 폰트 색상을 보면서 설정
- 터미널을 종료 했다가 다시 실행

**Iterm2를 사용 할 경우**

- brew cask install iterm2
- Iterm2의 `Preferences (cmd + ,) -> Colors 에서 Color Presets`를 설정
- Text 에서 Change Font로 가서 `Source Code Pro for Powerline` 찾아서 설정

## Ubuntu

### zsh 설치

```bash
# zsh 설치
sudo apt-get install zsh

# 설치경로 확인
which zsh
#=> /usr/bin/zsh

# 기본 sh 변경
chsh -s $(which zsh)
```

### oh my zsh 설치

```bash
> sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh"
```

### Theme Change

`~/.zshrc` 파일 안에서 `ZSH_THEME`를 찾아서 `agnoster`로 변경

```bash
> vim ~/.zshrc
```

```bash
# AS-IS
ZSH_THEME="robbyrussell"

# TO-BE
ZSH_THEME="agnoster"
```

### Powerline font 설치

먼저 아래와 같이 Powerline font를 설치한다

```bash
# clone
> git clone https://github.com/powerline/fonts.git --depth=1

# install
> cd fonts
> ./install.sh

# clean-up a bit
> cd ..
> rm -rf fonts
```

## 기타 옵션

### 멀티라인으로 변경 시

`~/.oh-my-zsh/themes/agnoster.zsh-theme`을 열어 아래의 부분을 수정 추가

```bash
## Main prompt
build_prompt() {
  RETVAL=$?
  prompt_status
  prompt_virtualenv
  prompt_context
  prompt_dir
  prompt_git
  prompt_bzr
  prompt_hg
  prompt_newline # 멀티라인 적용
  prompt_end
}

# 멀티라인 적용, 커버모양 변경
prompt_newline() {
  if [[ -n $CURRENT_BG ]]; then
    echo -n "%{%k%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR
%(?.%F{$CURRENT_BG}.%F{red})❯%f"

  else
    echo -n "%{%k%}"
  fi

  echo -n "%{%f%}"
  CURRENT_BG=''
}
```

### Vscode에서 사용시

**Mac일 경우**

`기본설정 > 설정 > 사용자 탭`을 클릭하고 json을 검색 후 settings.json에 아래의 코드를 입력한다.

```javascript
"terminal.integrated.shell.osx": "/bin/zsh",
"terminal.integrated.fontSize": 12,
"terminal.integrated.fontFamily": "Source Code Pro for Powerline",
```

**Ubuntu일 경우**

`기본설정 > 설정 > 사용자 탭`을 클릭하고 json을 검색 후 settings.json에 아래의 코드를 입력한다.

```javascript
"terminal.integrated.fontSize": 12,
"terminal.integrated.fontFamily": "Source Code Pro for Powerline",
```
