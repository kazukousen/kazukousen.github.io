Title: PythonでWebクローラー+スクレイピング その1
Date: 2017-02-23-15:00
Tags: python, crawler, scraping
Slug: works/170223python_crawler_scraping
Author: one_meets_seven
Summary: Python3 + requests + BeatifulSoup4 + MeCab + MongoDBを使う
Status: published

# PythonでWebクローラー+スクレイピング その1
Python3 + requests + BeatifulSoup4 + MeCab + MongoDB
を使って西野七瀬の公式ブログを解析してやるっ！  

## tl;dr
とりあえず各ライブラリのインストールと基本的な使い方を調べた.  

## 対象サイト
[西野七瀬 公式ブログ](http://blog.nogizaka46.com/nanase.nishino/)  
生物界最強の可愛さ.  

## 目的
西野七瀬の公式ブログを巡回して, 今までの記事を全件データベースに保存する.  
そして形態素解析をして, 西野七瀬botを作成し, 新世界の神となる.  

## BeautifulSoup4 + requests
BeautifulSoup4 + requestsでブログをHTML解析する.  

### requests
PythonのHTTPライブラリは
``urllib2``より``requests``がラクらしい.  
[Requests: 人間のためのHTTP](http://requests-docs-ja.readthedocs.io/en/latest/)  

```sh
pip install requests
```

```python
import requests
url = 'http://blog.nogizaka46.com/nanase.nishino/'
headers = {'User-Agent': 'Mozilla/5.0'}

response = requests.get(url, headers=headers) # <Response [200]>
```

``headers``を適当に付与しないと``403``が返ってくるページがある.  


### BeautifulSoup4
いわずとしれたHTML解析ライブラリ.  
他にpyqueryがある.  
[Beautiful Soup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)  

```sh
pip install beautifulsoup4
```

```python
import requests
from bs4 import BeautifulSoup

url = 'http://blog.nogizaka46.com/nanase.nishino/'
headers = {'User-Agent': 'Mozilla/5.0'}

response = requests.get(url, headers=headers) # <Response [200]>

soup = BeautifulSoup(response.text, 'html.parser')
```

``<div class="right2in"></div>``内を取得するときは,  

```python
right2in_content = soup.find(attrs={'class': 'right2in'})
```

この要素は一つしかないので``find_all``メソッドではなく``find``メソッドを利用.  

さらに入れ子の中の``<h1 class="clearfix"></h1>``も全件取得したい.  

```python
clearfix_contents = right2in_content.find_all(attrs={'class': 'clearfix'})
```

```html
[<h1 class="clearfix"><span class="date"><span class="yearmonth">2017/01</span><span class="daydate"><span class="dd1">31</span><span class="dd2">Tue</span></span></span><span class="heading"><span class="author">西野七瀬</span><span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2017/01/036710.php" rel="bookmark">冷たい爪</a></span></span></h1>,
 <h1 class="clearfix"><span class="date"><span class="yearmonth">2017/01</span><span class="daydate"><span class="dd1">13</span><span class="dd2">Fri</span></span></span><span class="heading"><span class="author">西野七瀬</span><span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2017/01/036476.php" rel="bookmark">りんご風呂</a></span></span></h1>,
 <h1 class="clearfix"><span class="date"><span class="yearmonth">2017/01</span><span class="daydate"><span class="dd1">08</span><span class="dd2">Sun</span></span></span><span class="heading"><span class="author">西野七瀬</span><span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2017/01/036348.php" rel="bookmark">賀正</a></span></span></h1>,
 <h1 class="clearfix"><span class="date"><span class="yearmonth">2016/12</span><span class="daydate"><span class="dd1">31</span><span class="dd2">Sat</span></span></span><span class="heading"><span class="author">西野七瀬</span><span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2016/12/036220.php" rel="bookmark">紙とペン</a></span></span></h1>,
 <h1 class="clearfix"><span class="date"><span class="yearmonth">2016/11</span><span class="daydate"><span class="dd1">25</span><span class="dd2">Fri</span></span></span><span class="heading"><span class="author">西野七瀬</span><span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2016/11/035603.php" rel="bookmark">定食好き</a></span></span></h1>]
</span></span></span></h>]
```


## Mecab
### Mecabをインストール

``brew``で入れる  

```sh
$ brew install mecab mecab-ipadic
```

### Pythonドライバー

``pip``で入れられる

```sh
pip install mecab-python3
```

### 実装例

```python
import MeCab

m = MeCab.Tagger('-Ochasen')
print(m.parse('テストです'))
```

```sh
テスト  テスト  テスト  名詞-サ変接続
です    デス    です    助動詞  特殊・デス      基本形
EOS
```

```python
import MeCab

m = MeCab.Tagger('-Ochasen')
node = m.parseToNode('テストです')

# node.surface 単語
# node.feature 単語の品詞などの情報

while node:
	print(node.surface)
	node = node.next
```

```sh
テスト
です
```

## まとめ
とりあえずインストールした.  

