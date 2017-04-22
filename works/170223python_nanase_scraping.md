Title: 西野七瀬ブログをスクレイピング
Date: 2017-02-23-19:00
Tags: python, crawler, scraping
Slug: works/170223python_nanase_scraping
Author: one_meets_seven
Summary: 西野七瀬ブログをスクレイピング
Status: draft

# 西野七瀬ブログをスクレイピング
西野七瀬のブログをスクレイピングする.  
[西野七瀬 公式ブログ](http://blog.nogizaka46.com/nanase.nishino/)  
生物界で一番可愛い.  

関連記事: [PythonでWebクローラー+スクレイピング その1](170223python_crawler_scraping.html)  

## 準備
```python
import requests
from bs4 import BeautifulSoup

url = 'http://blog.nogizaka46.com/nanase.nishino/'
headers = {'User-Agent': 'Mozilla/5.0'}

# Responseオブジェクト生成
response = requests.get(url, headers=headers)

# BeautifulSoupオブジェクト生成
soup = BeautifulSoup(response.text, 'html.parser')
```

## 記事タイトルとURLをスクレイピング
Google Chromeのデベロッパーツールで要素を検証したところ,  
記事タイトルを持つクラスは``<div class='entrytitle'>``である.  

```python
entrytitle_contents = soup.find_all(attrs={'class': 'entrytitle'})
```

このページは記事が5つあるので``entrytitle``も5つ取得できる.  

```html
[<span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2017/01/036710.php" rel="bookmark">冷たい爪</a></span>,
 <span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2017/01/036476.php" rel="bookmark">りんご風呂</a></span>,
 <span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2017/01/036348.php" rel="bookmark">賀正</a></span>,
 <span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2016/12/036220.php" rel="bookmark">紙とペン</a></span>,
 <span class="entrytitle"><a href="http://blog.nogizaka46.com/nanase.nishino/2016/11/035603.php" rel="bookmark">定食好き</a></span>]
</span>]
```

タイトルとURLを標準出力してみる.  

```python
for content in entrytitle_contents:
	text = content.text
	url = content.a['href']
	print('title: {0}, url: {1}'.format(text, url))
```

以下のように出力される.  

```sh
title: 冷たい爪, url: http://blog.nogizaka46.com/nanase.nishino/2017/01/036710.php
title: りんご風呂, url: http://blog.nogizaka46.com/nanase.nishino/2017/01/036476.php
title: 賀正, url: http://blog.nogizaka46.com/nanase.nishino/2017/01/036348.php
title: 紙とペン, url: http://blog.nogizaka46.com/nanase.nishino/2016/12/036220.php
title: 定食好き, url: http://blog.nogizaka46.com/nanase.nishino/2016/11/035603.php
```

