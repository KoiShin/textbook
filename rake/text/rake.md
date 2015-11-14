
## Install Rake

rakeはrubyのライブラリです。なので、まずrubyがすでにインストールされている必要があります。
rubyのインストールについては省略します。

rakeのインストール

```
$ gem install rake
```

## Execute Rake

rakeを実行するためには、Rakefileがあるディレクトリに移動してから次のコマンドを打ちます。

```
$ rake <taskname>
```

taskname を省略すると、defaultタスクが実行されます。

## Rake Tasks

rakeが行うタスクはRakefileに記述します。Rakefileがなければ新規ファイルとして新たに作成します。
Rakefileに書くことのできるタスクは次のようなものがあります。

name           | Description
:------------- | :-------------
task           | 一番基本となるタスク
rule           | 拡張子を指定してファイルを作成するためのタスク
multitask      | 複数の事前条件を並列で行うタスク
file           | ファイルを作成するためのタスク
directory      | ディレクトリを作成するためのタスク


### FileUtils

FileUtils にはシェル上での操作を行う関数が定義されています。
rake は実行時に FileUtils モジュールを require していると思われるので、FileUtils を require する必要はありません。

特に重要なのは `sh` で、引数の文字列をシステムコマンドとして実行する関数です。

FileUtils の詳細は rake の説明から離れるので、興味があれば FileUtils モジュールを参照してください。


### Task

一番基本となるタスク。タスクの処理内容や、依存関係を書くことができる。

書式

```ruby
# タスクの定義
task :taskA do
  # taskAが行う処理
end

# 前提条件を持つタスク
task :taskB => :taskA do
  # taskAが実行された後、taskBが実行される
end

# 前提条件のみ
task :taskC => :taskA

# 複数の前提条件を持つタスク
task :taskD => [:taskA, :taskB] do
  # taskAとtaskBが実行された後、taskDが実行される
end

# task関数の前にdesc関数を置くことで、そのタスクの説明をする
desc 'sample task E'
task :taskE do
  # taskEが行う処理
end

# defaultを定義すると、タスク名を指定しないでrakeを実行した場合の動作を指定できる
task :default => :taskA
```

### Rule

拡張子を指定してファイルを作成するためのタスク。

書式

``` ruby
rule '.html' => '.md' do |t|
  sh "pandoc -s #{t.source} -o #{t.name}"
end
```


### Multitask

タスクをより高速に実行できるように、依存する複数のタスクを並列して処理するタスク。

書式

```ruby
multitask :setup => [:install_ruby, :install_rake, :install_git] do
  puts 'The build is completed'
end
```

**注意**

複数のタスクを並列して処理するので、
「あるタスクを一番最初に実行する」とか
「このタスクは一番最後に終了させる」というのは指定できない。
指定する必要がある場合は、前述のtaskを使用すること。


### File

ファイルを作成するためのタスク。基本的には必ず、依存するファイルを伴う

書式

```ruby
file 'report.dvi' => 'report.tex' do |t|
  sh "latex #{t.source}"
end
```


### Directory

ディレクトリを作成するためのタスク。

書式

```ruby
directory 'path/to/dir'

file 'config.yml' => 'path/to/dir' do |t|
  sh "some-command > path/to/dir/config.yml"
end
```


























EOF
