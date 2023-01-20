---
title: "搭建 VScode 的LaTeX 編寫環境"
date: 2023-01-20T15:46:22+08:00
slug: VScode_Latex_tutorial
image: cover.jpg
categories:
    - Programming
tags:
    - VScode
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

# 搭建 VScode 的LaTeX 編寫環境

## 安裝 texlive 與 VScode

### For Windows 系統

1. 進入[texlive 下載頁面](https://tug.org/texlive/acquire-netinstall.html)，點擊 「install-tl-windows.exe」。

![img](https://i.imgur.com/b15bjTj.jpg)

2. 使用「工作管理員」，開啟「install-tl-windows.exe」。

![img](https://i.imgur.com/DquIRDL.jpg)

3. 選擇「install」，點「next」。

![img](https://i.imgur.com/ikCWYJl.jpg)

4. 點「install」。

![img](https://i.imgur.com/tlAl6El.jpg)

5. 選擇安裝路徑，點「install」。

![img](https://i.imgur.com/NtLc4XD.jpg)

6. 直到看到這句話就代表安裝完成。<span class="red">(安裝時間可能高達數小時，這是正常的)</span>

![img](https://i.imgur.com/YAdgGov.jpg)

7. VScode 的安裝可參考: [C++ programming with Visual Studio Code (Using gcc/g++ with MinGW)](/ICMRU7uUQuebYsc_LeL-Ug)

## 在 VScode 上配置環境

1. 搜尋「LaTeX Workshop」擴充插件，安裝。

![](https://i.imgur.com/Wyjsq0C.jpg)

2. 打開VScode，按`crtl+P`或`F1`打開命令行。輸入`>settings (JSON)`。選擇「Open User Setting」。

![](https://i.imgur.com/6XHDv6X.jpg)

3. 複製以下程式碼加入「setting.json」。此部分定義:pdf的閱讀方式，自動編譯與訊息的表示方式等細項。
    
``` json=1
    // LaTeX 配置
    "latex-workshop.view.pdf.viewer": "tab",
    "latex-workshop.latex.autoBuild.run": "never",
    "latex-workshop.showContextMenu": true,
    "latex-workshop.intellisense.package.enabled": true,
    "latex-workshop.message.error.show": false,
    "latex-workshop.message.warning.show": false,
```

4. 複製以下程式碼加入「setting.json」。此部分定義:LaTeX 的編譯工具。LaTeX Workshop 默認的編譯工具為latexxmk。這裡加入中文環境常用的xelatex，IEEE用的pdflatex，交互參照參考文獻要用的bibtex。


``` json=1
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
```

5. 複製以下程式碼加入「setting.json」。此部分定義:LaTeX 的編譯鏈，決定編譯的順序與方式。<span class="red">放在首位的將被定義為默認的編譯方式。</span> 需要編譯bib檔案的情況下須使用`"xelatex -> bibtex -> xelatex*2"` 或 `"pdflatex -> bibtex -> pdflatex*2"` 編譯。
    
``` json=1
    "latex-workshop.latex.recipes": [
        {
            "name": "XeLaTeX",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "PDFLaTeX",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "BibTeX",
            "tools": [
                "bibtex"
            ]
        },
        {
            "name": "LaTeXmk",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "xelatex -> bibtex -> xelatex*2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdflatex -> bibtex -> pdflatex*2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        },
        {"name": "xelatex*2",
            "tools": [
                "xelatex","xelatex"
            ]
        },
    ],
```

6. 複製以下程式碼加入「setting.json」。此部分定義:自動清除編譯的中間產物。

``` json=1
    "latex-workshop.latex.clean.fileTypes": [
        // "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        // "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk"
    ],
    "latex-workshop.latex.autoClean.run": "onFailed",
    "latex-workshop.latex.recipe.default": "lastUsed",
    "latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",
```

> Remark: 有無最後的逗點取決於:此json檔在這段程式碼後，是否結束。如果接續其他設定，則要加逗號。

## 完成!

1. 到此為止基礎的環境配置已經完成。完成程式碼後，使用`crtl+alt+B`利用默認的方式編譯。或根據下圖指示編譯方式。使用`crtl+alt+V`可以快速打開pdf進行預覽。參考: [My LaTeX user guide](/fkOFnbjRRiuxtLj29koTgw)。

![image alt](https://i.imgur.com/zb28joA.jpg)

## 追加介紹: 使用SumatraPDF 外部程式閱讀編譯後的pdf並配置正向和反向搜索。

### Why SumatraPDF ? 
其open souce 與不鎖定pdf 的特性使我們可以自定義其操作環境，輕鬆配置與LaTeX 源碼的正反向搜索。並在開啟pdf 的狀況下依然可以編譯並修改pdf 文件。
> Remark: 如果使用內置的pdf閱讀器，不須進行額外配置，即可使用正反像搜索。

### 安裝與配置

1. 進入[下載頁面](https://www.sumatrapdfreader.org/download-free-pdf-viewer)並下載安裝檔。

![image alt](https://i.imgur.com/newkKcJ.jpg)

2. 開啟安裝檔，右下角「option」可以顯示安裝選項。完成設定後開始安裝。

![image alt](https://i.imgur.com/xfnWmwc.jpg)

3. 看到以下畫面代表安裝完成。

![link text](https://i.imgur.com/O6YeYYN.jpg)

4. 回到VScode， 按`crtl+P`或`F1`打開命令行。輸入`>settings (JSON)`。選擇「Open User Setting」。加入以下兩行。

``` json=1
"latex-workshop.view.pdf.viewer": "external",

"latex-workshop.view.pdf.external.viewer.command": "D:/SumatraPDF/SumatraPDF.exe",
```

5. 至此，SumatraPDF閱讀pdf的配置已完成。

### 正向搜索

1. 按`crtl+P`或`F1`打開命令行。輸入`>settings (JSON)`。選擇「Open User Setting」。將第一行與第八行的路徑分別改為SumatraPDF與VScode的安裝路徑。

``` json=1
"latex-workshop.view.pdf.external.synctex.args": [
        "-forward-search",
        "%TEX%",
        "%LINE%",
        "-reuse-instance",
        "-inverse-search",
        "\"D:\\Microsoft VS Code\\Code.exe\" \"D:\\Microsoft VS Code\\resources\\app\\out\\cli.js\" --ms-enable-electron-run-as-node -r -g \"%f:%l\"",
        "%PDF%"
],
```

2. 回到LaTeX 源碼，使用 `crtl+alt+J` 即可正向收尋到pdf中的相對位置。也可按照下圖操作。

![image alt](https://i.imgur.com/tahknvJ.jpg)

### 反向搜索

1. 進入SumatraPDF的高級設置文件。

![link text](https://i.imgur.com/h7en0cg.jpg)

2. 找到:`InverseSearchCmdLine` 與 `EnableTeXEnhancements` 這兩個參數。修改為下列所示，注意路徑要與先前安裝的一致。

```
InverseSearchCmdLine = "D:\Microsoft VS Code\Code.exe" "D:\Microsoft VS Code\resources\app\out\cli.js" --ms-enable-electron-run-as-node -r -g "%f:%l"
EnableTeXEnhancements = true
```

3. 使用`ctrl+alt+V`打開pdf文件後，雙擊左鍵即可反向跳轉至LaTeX 源碼的相對位置。


## 追加介紹: 使用Sioyek 外部程式閱讀編譯後的pdf並配置正向和反向搜索。

### Why Sioyek?
[Sioyek ](https://sioyek.info/)為技術性與學術性閱讀開發的pdf閱讀器，一樣擁有open souce 與不鎖定pdf 的特性。

### 安裝與配置正向收尋

1. 進入[官網](https://sioyek.info/)，點選「download」。選擇Windows版本下載。

![link text](https://i.imgur.com/W93yeKC.jpg)

2. 下載後解壓縮，並將資料夾放置於一個固定位置。

3. 回到VScode， 按`crtl+P`或`F1`打開命令行。輸入`>settings (JSON)`。選擇「Open User Setting」。加入以下程式碼，將第一、二、五行的路徑分別改為sioyek與VScode的安裝路徑。

``` json=1
"latex-workshop.view.pdf.external.viewer.command": "D:/sioyek_release_windows/sioyek.exe",
    "latex-workshop.view.pdf.external.synctex.command": "D:/sioyek_release_windows/sioyek.exe",
    "latex-workshop.view.pdf.external.synctex.args": [
        "--inverse-search",
        "\"D:/Microsoft VS Code/Code.exe\" \"D:/Microsoft VS Code/resources/app/out/cli.js\" --ms-enable-electron-run-as-node -r -g \"%1:%2\"",
        "--reuse-instance",
        "--forward-search-file",
        "%TEX%",
        "--forward-search-line",
        "%LINE%",
        "%PDF%"
],
```

4. 至此，sioyek閱讀pdf的配置已完成。回到LaTeX 源碼，使用 `crtl+alt+J` 即可正向搜尋到pdf中的相對位置。


### 反向搜尋

1. 進入放置sioyek的資料夾中，打開「prefs.config」檔案。找到`inverse_search_command`，並修改為:

```
inverse_search_command "D:\Microsoft VS Code\Code.exe" "D:\Microsoft VS Code\resources\app\out\cli.js" --ms-enable-electron-run-as-node -r -g "%1:%2"
```
2. 使用`ctrl+alt+V`打開pdf文件後，按`F4`進入「synctex mode」。右鍵點擊，即可反向跳轉至LaTeX 源碼的相對位置。

## 追加介紹: LaTeX 編譯的中間產物與注意事項

LaTeX 在編譯過程中會生成相當多的輔助文件和日誌(log)。如交互參照、參考文獻、目錄、索引等功能，需要先編譯生成輔助文件，再次編譯時讀入輔助文件得到正確的結果。因此，才需要定義編譯鏈，已得到較複雜的編譯結果。

* log:  排版引擎生成的日誌文件，供排查錯誤使用。
* aux: LaTeX 生成的主輔助文件，記錄交互參照、目錄、參考文獻的引用等。
* toc: LaTeX 生成的目錄記錄文件。
* lof: LaTeX 生成的圖片目錄記錄文件。
* lot: LaTeX 生成的表格目錄記錄文件。
* bbl: BibTeX 生成的參考文獻記錄文件。
* blg: BibTeX 生成的日誌文件。
* idx: LaTeX 生成的供 makeindex 處理的索引記錄文件。
* ind: makeindex 處理 .idx 生成的格式化索引記錄文件。
* ilg: makeindex 生成的日誌文件。
* out: hyperref 宏包生成的 PDF 書籤記錄文件。

<span class=red>大部分的中間產物可以透過先前的設定在編譯完成後自動刪除。但 `.log`、 `.aux`、 `.synctex.gz`建議留下，因為這些文件分別跟error code、交互參照、正反向搜索有關。刪除會導致正反向搜索功能失效或造成debug困難。</span>


## 追加介紹: 快捷鍵修改與自定義

1. 按`crtl+P`或`F1`打開命令行。輸入`>keyboard`，點選「Open Keyboard Shortcuts (JSON)」

![link text](https://i.imgur.com/GZYlv1s.jpg)

2. 可以根據下列範例自由定義快捷鍵。
``` json=1
{
    "key": "alt+b", //編譯
    "command": "latex-workshop.build",
    "when": "editorTextFocus && !isMac"
},
{
    "key": "alt+t", //終止編譯
    "command": "latex-workshop.kill",
    "when": "editorTextFocus && !isMac"
},
{
    "key": "alt+j", //正向搜索
    "command": "latex-workshop.synctex",
    "when": "editorTextFocus && !isMac"
},
{
    "key": "alt+e", //選擇編譯方式
    "command": "latex-workshop.recipes"
},
```



