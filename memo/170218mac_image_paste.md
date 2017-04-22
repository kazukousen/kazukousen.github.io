Title: Mac OS Xでコピーした画像を画像ファイルにする方法
Date: 2017-02-18
Category: Memo
Tags: Mac
Slug: 170218mac_image_paste
Author: one_meets_seven
Summary: Toyviewer使うかpngpaste使う
Status: published

# Mac OS Xでコピーした画像を画像ファイルにする方法

## tl;dr
- アプリToyViewerを使う。
- それかCUIのpngpaste使う。


## ToyViewer
[Toy Viewer](https://itunes.apple.com/jp/app/toyviewer/id414298354?mt=12)  

無料のMac OS Xアプリ.  
起動してメニューの「ファイル」→「ペーストボードから開く(Shift+Cmd+V)」  
するとクリップボードにコピーされていたイメージを扱える  

「ファイル」→「形式を保存」でpngとかで保存する(結構フォーマットの種類が豊富)  

## pngpaste
- いちいちGUIはいやだ,ターミナルのコマンドで完結したい
- フォーマットはpngのみでいい

という人はbrewからpngpasteをインストール  

```sh
$ brew install pngpaste
```

``pngpaste ファイル名``で、そのファイル名で画像ファイルが生成される.  

```sh
$ pngpaste hogehoge.png
```
