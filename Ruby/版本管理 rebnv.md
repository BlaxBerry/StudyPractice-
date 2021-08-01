# rbenv

轻量级的Ruby的版本管理工具

> rbenvは、複数のRubyのバージョンを管理し、プロジェクトごとにRubyのバージョンを指定して使うことを可能としてくれるツールです。
>
> Ruby environmentの略でしょう。
>
> 読み方は「アールビー・エンブ」または「アールベンブ」

<img src="https://blogenist.jp/wp-content/uploads/2018/01/rbenvInstallEye.png" style="zoom:50%;" />



Mac自带 ruby，但是直接通过 **gem install xxx** 下载会报错，

通过系统自带的ruby会因为权限不够，无法通过gem下载

```bash
# You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

此时需要通过配置 rbenv 管理Ruby版本

并修改环境变量修改ruby的路径，

使运行的ruby的路径由系统自带的变为rbenv





## 安装和配置

1. **下载rbenv**

```bash
brew install rbenv ruby-build
```

2. **打开 .zshrc， 并追加写入下面内容**

```bash
open ~/.zshrc
```

```bash
export PATH="$HOME/.rbenv/bin:$PATH" 
eval "$(rbenv init - zsh)"
export PATH="$HOME/.rbenv/shims:$PATH"
```

若没有 .zshrc，则先创建后再追加内容

```absh
touch ~/.zshrc
```

3. **下载指定版本ruby**

```bash
rbenv install 2.6.6

rbenv global 2.6.6

rbenv rehash
```

```bash
rbenv version
# 2.6.6 (set by /Users/用户名/.rbenv/version)
ruby -v
# ruby 2.6.6p146 (2020-03-31 revision 67876) [x86_64-darwin20]
```

4. **验证**

运行的ruby和gem的路径，由系统自带的变为rbenv

- 配置rbenv之前

```bash
which ruby
# /usr/bin/ruby
which gem
#/usr/bin/gem
```

- 配置rbenv后

```bash
which ruby
# /Users/用户/.rbenv/shims/ruby

which gem
# /Users/用户/.rbenv/shims/gem
```

