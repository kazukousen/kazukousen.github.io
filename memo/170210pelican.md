Title: PelicanとGitHub-pagesでブログ開設
Date: 2017-02-11 00:00
Modified: 2017-02-11 11:45
Category: Memo
Tags: pelican, GitHub, python
Slug: python-pelican-install
Author: one_meets_seven
Summary: Python3系の環境構築をまとめた. 静的サイトジェネレータPelicanでGitHub-pagesデビューした.

# PelicanとGitHub-pagesでブログ開設

GitHub-pagesを使ってみたかった  
HTML5, CSS, JavaScriptが動くということで静的サイトジェネレータを使うことにした  

どうせならPython製の[Pelican](http://docs.getpelican.com/en/stable/)
Markdownでモダンな静的サイトでも作ることにした  

## GitHub Pages

まずは自分のGitHubアカウントで  
``Repository name``に``username.github.io``と入力して新規作成  
つくったらとりあえず今は放っておく  

## pythonで環境構築 
怖がる必要はない. python3系を構築しましょう.  

- pyenvでバージョン管理
- virtualenvでプロジェクト管理

の手段をとる.  
 

### pythonにおけるプロジェクト管理 
プロジェクトディレクトリを作ってその中にvirtualenvの環境づくり.  
virtualenvはpipでインストールすればいいらしい.  
python3系にはvenvがあるらしいが結局みんなvirtualenvにお世話になってるぽい.  

```sh
$ pip install virtualenv
$ cd ~
$ mkdir Blog && cd Blog
$ virtualenv venv
$ source venv/bin/activate
(venv)$
```

``venv``という名前で環境を管理する.  
activateを実行で``venv``環境に入る.  
抜ける時は  

```sh
(venv)$ deactivate
$
```

作った環境でモジュールたちをpipでインストールしまくっても  
グローバル環境が汚れずに済む.  

**ここから先は(venv)と明記しない.**  
 

## pelican 
以下をpipで一気にインストール.  
``requirements.txt``  

```sh
% requirements.txt
pelican
Markdown
ghp-import
```

``ghp-import``はGitHub-Pagesリポジトリへの遠隔操作をいい感じに楽にしてくれる.  


```sh
$ pip install -r requirements.txt
$ pip list
```

``pelican-quickstart``コマンドでpelicanの雛形をつくる.  
いろいろ対話形式で設定をすることになる.  

```sh
> Where do you want to create your new web site? [.] .
> What will be the title of this web site? your blog
e Who will be the author of this web site? yourname
> What will be the default language of this web site? [en] ja
> TimeZone的なこと [Europe/Paris] Asia/Tokyo
> Do you want to specify a URL prefix? e.g., http://example.com   (Y/n) Y
> What is your URL prefix? (see above example; no trailing slash) https://yourname.github.io
> Do you want to enable article pagination? (Y/n) Y
> How many articles per page do you want? [10] 10
> Do you want to generate a Fabfile/Makefile to automate generation and publishing? (Y/n) Y
> Do you want an auto-reload & simpleHTTP script to assist with theme and site development? (Y/n) Y
> Do you want to upload your website using FTP? (y/N) N
> Do you want to upload your website using SSH? (y/N) N
> Do you want to upload your website using Dropbox? (y/N) N
> Do you want to upload your website using S3? (y/N) N
> Do you want to upload your website using Rackspace Cloud Files? (y/N) N
> Do you want to upload your website using GitHub Pages? (y/N) y
```
 

## Markdown 
早速記事を書いてみる.  
``content``ディレクトリ下にmdファイルを作る  

````md
% 0210first_post.md
Title: ブログタイトル
Date: 2017-02-10 23:00
Category:	Memo
Tags: pelican, python
Slug: 0210first_post
Author: yourname
Summary: ブログの要約
 

# ブログのタイトル 

- List
- List
- List

```python
print('Hello World!')
```
````

``make html``でHTMLにコンパイルし``make serve``でローカル環境で確認.  
HTMLは``output``ディレクトリの中に作成される.  

```sh
$ make html
$ make serve
```

``http://localhost:8000``にアクセス  

## GitHub PagesにPush
```sh
$ git init
```

``.gitignore``でpushを除外するファイルを記述する.  

```rb
## Pelican
output/*
*.pid

## Python
*.py[cod]

## Mac OS X
.DS_Store
.*~
*~
```

push  

```sh
$ ghp-import output
$ git push https://github.com/yourname/yourname.github.io.git gh-pages:master
```

``https://yourname.github.io``に反映されるはず.  
(少しラグあり)  
 

## おまけメモ
### テーマ変更

テーマを変更してみる.  
[Pelican Themes](http://www.pelicanthemes.com)

```sh
$ mkdir themes && cd themes
$ git clone https://github.com/gilsondev/pelican-clean-blog
$ cd ..
```
 

テーマを導入  
```sh
$ pelican-themes -i themes/pelican-clean-blog
```
 

入ったか確認  
```sh
$ pelican-themes -l
```
 

設定ファイル``pelicanconf.py``に以下を追加.  

```python
THEME = 'pelican-clean-blog'
```

### プラグインを導入
```sh
$ mkdir plugins && cd plugins
$ git clone https://github.com/getpelican/pelican-plugins.git
$ cd ..
```

#### MathJax
LaTeXみたいな数式を書きたい  
依存モジュールをインストール  
 

```sh
$ pip install typogrify
```

``pelicanconf.py``を書き換える.  

```python
PLUGINS = ['render_math']
```

これで使えるようになる.  

```md
$$
{\mathcal L}(x) = -{\rm KL}[q(z|x)||p(z)] + {\mathbb E}_{q(z|x)}[p(x|z)]
$$
```
 

$$
{\mathcal L}(x) = -{\rm KL}[q(z|x)||p(z)] + {\mathbb E}_{q(z|x)}[p(x|z)]
$$
 

#### Jupyter Notebook
なんとJupyterのセルも貼り付けるプラグインがある  
依存モジュールをインストール  

```sh
$ pip install ipython jupyter
```

つぎに``pelicanconf.py``を書き加える  

```python
PLUGINS = ['liquid_tags.notebook', 'render_math']
NOTEBOOK_DIR = 'notebooks'
```

```md
{% notebook yourfilename.ipynb cells[0:12] %}
```

指定したcellの分だけ表示できる.  

