layout: post
title: "Java search engine - Lucene"
date: 2014-08-15 08:24:30 +0800
comments: true
Category: Java
Tags: Lucene
Has_Math: True


昨天聽 [qing](http://www.slideshare.net/qingwang/presentations) 的速記，因為內容豐富，有些來不及 key in，所以有些片片斷斷。

qing 十年前就已經在這方面研究了，這次的內容，是希望幫助入門的人，能快速了解基本的東西。
<!--More-->

##Search Engine 相關應用

* Web Crawler
* 文件格式，語系：各種格式的文件搜尋
* 進階搜尋，檢索功能
* 分散式 `資料量` `計算量`
* 自然語言處理
* Text Mining

##Solr

* 基於 Lucene 的平台
* 全文索引
* HighLighting
* Faceted Search
* 近乎即時的索引速度
* 標準化的界面 XML,JSON

##Tika

* 內容分析工具組：內容偵測：檔案格式偵測，文字語言偵測，Parser.
* Tica 支援格式：HTML, XML....

##Nutch

* Web Crawler
* Highly modulized

##Components

Ijector -> Web Graph ->

##OpenNLP

* Natual Language Processing
* OpenNLP 是 Apache 的自然語言處理城市庫
* 句子偵測
* Name Finder 名稱搜尋
* Document Categroizer 文件分類
* Part of Speech 詞性偵測

##基於搜尋引擎的進階應用

* 搜尋引擎建立文件相等的資料

##進階的搜尋功能實作

* 以文找文
* 關聯詞分析
* 關鍵詞分析
* 文件分類
* 文件群集

##Lucene 上建立多份索引

* bi-gram

## 同音字搜尋
資料建立時，至少有 全文索引，注音索引

## 同義詞查詢

人工維護
繁體是簡體的 Superset

## 詞庫索引

* bi-gram, tri-gram,.. n-gram?
* 不需要依靠事先建好的詞庫
* 缺點：索引變大，降低搜尋效率
* 搜尋出非目標的結果
* 詞庫索引可以視為一種縮小索引內容及範圍的方式：更精准，但需事先建立


## Lucene內建中文斷詞
"我是台灣人"

* SmartChineseAnalyzer"我""是"台灣""人"
* CJKTokenizer
* ChineseTokenizer

## Text Mining

* 文字探勘，從文字資訊中發掘出有用資訊的程序
* Data mining Vs. Text Mining
* Data Mining 假設處理據結構的資料
* Text Mining 的資源輸入為自然語言形式的文件資料

## 生活中的應用

	blog
	FB
	Twitter
	Youtube 的文字敘述
	購物網站

## 文字探勘的例子

	blog/news/自動分類
	Facebook/Twitter 近況更新
	口碑分析
	使用者偏好分析
	廣告投效
	推薦興趣相同的使用者
	推薦投其所好的社團

## 大部份的 Search Engine 都在做 Inverted Index

從詞出發，找出相關文章

## 建立 Vector Space Model

## Turn Frequency - Inverse Document Frequency

* TF 詞在文件中出現的頻率，衡量特定詞，在單一文件中的重要性
* IDF 衡量特定詞所含的資訊重要程度
* IDF 跟 Entropy 的公式很相似

## Term Frequency Vector

## 取得 TF-IDF 的下一步：Apache Mahout

* Mahout is a scalable, extensible machine learning lib
* Mahout 在 Apache Hadoop 使用 map/reduce 實作 clustering

## Mahout 提供的各種演算法

* 分類演算
* Cluster K-Means, Fuzzy K-Means
* 提供計算相似度的各種算法

## 文件的分群

取得文件的 Vector 後套入 Mahout

	K-Means
	Fuzzy K-Means
	Meanshift
	Centroid Generation
	Direchlet Clustering

## 關鍵詞推薦

先取得文件的 Term Vector
送入 Hahout 的 ItemBasedRecommender

## 關鍵詞與文字屬性

文章的關鍵詞是 TF-IDF 分數較高者

為使用者或商品標示關鍵詞：

把人當做 Document，按贊後關鍵詞就是這個人的特徵

加入購物車，觀看商品細節，都可以從關鍵詞中，推出使用者的偏好

##處理 Big Data 的文件

文字探勘： 轉換成向量：現在用 Lucene


## Mahout 與 Lucene 已經整合的很好

Mahout 允許你撰寫城市，利用其 API 來處理文件資料
文字轉向量時可套用 TF-IDF

## 結語

輪子都有了

Lucene 相關技術
