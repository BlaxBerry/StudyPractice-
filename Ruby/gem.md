# gem基础

-  **gem** ：

  是Ruby中的第三方库文件，存放于 [RubyGems.org](https://rubygems.org/) 

- **RubyGems** ：

  gem 安装卸载等的工具

- **bundler**：

  

## gem语法

#### 下载安装

```bash
gem install xxx
```

```bash
gem install rails
```

---

#### 下载指定版本

```bash
gem install rails --version 版本
#或
gem install rails -v 版本
```

```bash
gem install rails --version 5.0
#或
gem install rails -v 5.0
```

---

#### 卸载

```
gem uninstall xxx
```

```bash
gem uninstall rails
```

---

#### 列出下载的gem

所有

```
gem list -l
```

指定搜索关键字

```bash
gem list 包含的关键字
```

```bash
gem list rail

*** LOCAL GEMS ***

autoprefixer-rails (10.2.5.1)
jquery-rails (4.4.0)
rails (6.1.4)
rails-dom-testing (2.0.3)
rails-html-sanitizer (1.3.0)
railties (6.1.4)
sass-rails (6.0.0)
sassc-rails (2.1.2)
sprockets-rails (3.2.2)
```







## bundler

以前是用**RubyGems**工具管理 gem 安装卸载，但现在更多是用 **bundler** 工具

> bundlerはgem同士の依存関係を管理できるツールです。bundlerを使うと、依存関係にある複数のgemを一括インストールすることがが出来



### 为何使用bundler

> 様々なgemを組み合わせて使っていると、互換性の問題が出てくる場合がある
>
> 「A包 1.0」と「B包 1.0」はうまく動くけど、
>
> 「A包 2.0」と「B包 2.0」最新バージョン同士が動かない。
>
> また、複数人、複数環境で開発を行う場合は、各環境で使うライブラリの名前、バージョンを合わせる必要がある
>
> こういった場合に、gem同士の互換性を保ちながら各gemの導入、管理を行ってくれるのが**Bundler**



### 使用步骤

#### 1. 下载bundler

bundler本身也是一个 gem包，需要下载安装后使用

```bash
gem install bundler
```

检查下载的bundler的版本

```bash
bundler -v
```

---

#### 2. 记入Gemfile中

将想要下载的 gem 记入 **Gemfile文件** 中，

```bash
gem '包'
```

```bash
gem 'bootstrap'
```

指定 gem 的版本

```bash
gem '包', '~> 版本'
```

```bash
gem 'rails', '~> 6.1.4'
gem 'bootstrap', '~> 5.0.2'
```

#### 3. 下载

下 **Gemfile文件** 中记载的所有 gem

```bash
bundle install
```







```bash

```

