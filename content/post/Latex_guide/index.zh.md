---
title: "My LaTeX user guide"
date: 2023-01-20T15:54:21+08:00
image: cover.jpg
categories:
    - Programming
    - Writing
tags:
    - LaTeX
# draft: true
---

<style>
.red {
  color: red;
}
.blue {
  color: blue;
}
</style>


# My LaTeX user guide

我的操作環境是安裝[texlive](https://tug.org/texlive/acquire-netinstall.html)，並使用VSCode擴充[LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)。

## LaTeX

LaTeX 是<span class="red">標記式語言(markup language)</span>，運作方式與HTML類似，用標記來指定排版，文字大小、顏色等。
 
以下是最簡單的LaTeX原始碼:
```
\documentclass{article}

%宣告區

\begin{document}

    %本文區
    My name is Sian!

\end{document}
```

## .tex 文件 - 基本架構

### 設定文件類型
```latex=1
\documentclass[options]{class_name}
```
<span class="blue">**{class_name}**</span> 為此篇文書的類型:

| class_name | 說明                                            |
|:----------:|:---------------------------------------------- |
|  article   | 用來排版學術論文、學術報告等                        |
|   report   | 格式有文章結構，用來排版回顧類、長篇論文、報告         |
|    book    | 排版出版書籍                                     |
|    proc    | 學術論文模板                                     |
|   slides   | 簡報格式的文檔                                   |
|  moderncv  | 用於個人簡歷                                     |
|   beamer   | 用於製作簡報                                     |

<span class="blue">**\[options]**</span> 為可選用參數(可省略):

|  options  | 說明                                                                                         |
|:---------:|:-------------------------------------------------------------------------------------------- |
|   10pt    | 字體大小，不輸入時預設為10pt                                                                 |
|  a4paper  | 紙張大小，預設為letterpaper，可選擇: a4paper、a5paper、b5paper、executivepaper 和 legalpaper  |
| titlepage | 是否有標題頁，article預設為notitlepage，report和book預設為titlepage                           |
| landscape | 令排版方向為橫向，不輸入時預設為縱向                                                           |
| onecolumn | 令單雙欄排版，預設為onecolumn，可選擇: twocolumn                                             |
|   fleqn   | 令公式向左對齊，不輸入時預設置中                                                              |
|   leqno   | 令公式編號在左，不輸入時預設在右                                                              |
|   final   | 文稿模式，預設為final(終稿)，可選擇: draft(草稿)                                              |
|  oneside  | 單雙面，預設為單面(oneside)，可選擇: twoside                                                 |


---

### 宣告區 (<span class="red">中文輸入使用方法</span>)

宣告區用於宣告使用的宏包(類似函式庫)、文章基本資訊與特殊功能(未來備註)。

```latex=+
%宣告使用宏包
\usepackage{ctex}
\usepackage{package_name} 

%文章基本資訊
\title{ 標題名稱 }
\author{ 作者名稱 }
\date{ \today }
```

<span class="blue">**xeCJK** 為一支持中文編輯的宏包</span>，允許在文檔中使用中文，且支援許多中文字體(詳情看範例)。以下為使用字體範例:
```latex=+
\begin{document}
    
    %中括號內打上使用字體(到電腦"字型"設定查詢)
    \newCJKfontfamily{\Kai}{標楷體}
    \setCJKmainfont{標楷體}
    

\end{document}
```
> <span class="red">使用**xeCJK**時:</span>
使用XeLaTeX編譯，XeLaTeX是一種使用Unicode的LaTeX排版引擎，預設輸入文件以utf-8編碼。使用命令 *xelatex .tex* 編譯產生PDF文件。

**文章基本資訊**可以設定<span class="blue">文章標題(\title)</span>、<span class="blue">作者(\author)</span>與<span class="blue">日期(\date)</span>等。<span class="blue">\and</span> 用於連接每個作者，<span class="blue">\thanks</span>命令會產生一個對應的腳註，用以致謝或標註聯絡方式。在文本區使用 **\maketitle** 即可產生標題。見範例:

```latex=1
\documentclass[UTF8]{article}

%宣告使用宏包
\usepackage{xeCJK}

%文章基本資訊
\title{ 成大校狗研究報告 }
\author{ 乖乖黃 \\ 國立成功大學, 雲平幫
\and 
白臉\thanks{E-mail} \\ 國立成功大學, 雲平幫 
\and 
呆呆 \\ 國立成功大學, 自強三傻
\and
麵線 \\ 國立成功大學, 自強三傻
\and
米香 \\ 國立成功大學, 自強三傻 }
\date{ \today }

\begin{document}

    %本文區
    \maketitle
    My name is Sian!

\end{document}

```

輸出結果:
![](https://i.imgur.com/6IVie10.jpg)


利用<span class="blue">**authblk**宏包</span>可以幫助標註多作者與研究單位，中括號 (\[\*]) 可幫作者與研究單位編號對應。在文本區使用 **\maketitle** 即可產生標題。見範例:


```latex=1
\documentclass[UTF8]{article}

%宣告使用宏包
\usepackage{xeCJK}
\usepackage{authblk}

%文章基本資訊
\title{ 成大校狗研究報告 }
\author[1]{ 乖乖黃 }
\author[2]{ 白臉 }
\author[3]{ 呆呆 }
\author[4]{ 麵線 }
\author[5]{ 米香 }
\affil[1,2]{ 雲平幫 }
\affil[3,4,5]{ 自強三傻 }
\date{ \today }

\begin{document}

    %本文區
    \maketitle
    My name is Sian!

\end{document}

```

輸出結果:
![](https://i.imgur.com/yjQyssY.jpg)

> 其實**文章基本資訊**也可以打在本文區。

### 本文區 (包括一些命令)

待續...

## Beamer: 如何利用LaTeX製作簡報

參考"自用Beamer Theme"。(待補)

# 數學公式參考

![](https://i.imgur.com/us6PNv0.png)
![](https://i.imgur.com/DDqcsDH.png)
![](https://i.imgur.com/nd2Qzuh.png)
![](https://i.imgur.com/uc0hpvK.png)