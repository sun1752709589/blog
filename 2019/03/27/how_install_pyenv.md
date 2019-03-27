# 手把手教你安装python版本管理工具pyenv

## macOS系统安装(use homebrew)
```shell
brew update
brew install pyenv
```

## ubuntu系统安装(use apt-get)
其他版本的shell需要将.bashrc改为对应的文件如zsh为.zshrc
```shell
1. git clone https://github.com/pyenv/pyenv.git ~/.pyenv
2. echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
3. echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
4. echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
5. 退出当前登录重新登录
6. pyenv install 3.7.2
```

## 如何升级
```shell
brew upgrade pyenv (macOS)

1. cd ~/.pyenv
2. git pull
```

## 基本命令使用
![基本命令使用](imgs/pyenv_use.png)

## 注意事项
在新安装某个python版本后需要执行:`pyenv rehash`