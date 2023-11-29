---
date: 2023-09-03 19:50:24.801348
---
# VSC+Tex Live + Latex

## 安装Tex Live

[link](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)

下载并进入texlive.ios

执行install-tl-windows.bat

安装配置。点击“Advance”，然后可以修改安装目录“Installation root”。点击“Customize”，左边语言包先点击“无”，然后只勾选“Chinese”和“US and UK English”，右边工程包不勾选“TeXworks editor; TL includes only the Windows binary”，即不勾选配置主菜单的“安装TeXworks前端”（工程包和配置主菜单其他项均勾选）。最后点击“确定”，再点击“安装”，耐心等待安装完成，安装时间将持续30分钟甚至更长.

配置系统环境 D:\ENV\texlive\2023\bin\windows

输入命令“xelatex -v”，验证安装。



## VSC

安装 LatexWorkshop

![image-20230903195421568](https://raw.githubusercontent.com/DingSJ101/picgo_hub/main/img/20230903195423.png)

打开LaTeX环境设置页面。点击设置图标，点击菜单中的“设置”，再点击右上角设置按钮，打开json配置文件。实际上，GUI设置页面和json设置页面作用相同。不同的是，GUI设置页面更直观，但项目需要去寻找，比较麻烦，且不能快速导入配置，而json设置页面以代码形式可以很快的导入配置。最后，删除“setting.json”配置文件默认的代码，复制粘贴以下标准配置代码，然后点击左上角“文件-保存”，再关闭“setting.json”。

```json
{
    // 设置是否自动编译
    "latex-workshop.latex.autoBuild.run":"never",
    //右键菜单
    "latex-workshop.showContextMenu":true,
    //从使用的包中自动补全命令和环境
    "latex-workshop.intellisense.package.enabled": true,
    //编译出错时设置是否弹出气泡设置
    "latex-workshop.message.error.show": false,
    "latex-workshop.message.warning.show": false,
    // 编译工具和命令
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
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
    // 用于配置编译链
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
        }
    ],
    //文件清理。此属性必须是字符串数组
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
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
    //设置为onFaild 在构建失败后清除辅助文件
    "latex-workshop.latex.autoClean.run": "onFailed",
    // 使用上次的recipe编译组合
    "latex-workshop.latex.recipe.default": "lastUsed",
    // 用于反向同步的内部查看器的键绑定。ctrl/cmd +点击(默认)或双击
    "latex-workshop.view.pdf.internal.synctex.keybinding": "double-click"
}
```

编译测试。正常情况下，新建一个文本文件，选择Latex语言，复制粘贴以下代码，右上角点击编译按钮或ctrl+alt+B进行编译，点击内部查看器或ctrl+alt+V，在右边进行预览，鼠标双击pdf可以定位相应代码位置。

```latex
\documentclass[a4paper]{article}
\usepackage[left=1.91cm,right=1.91cm,top=2.54cm,bottom=2.54cm]{geometry}  %设置页边距
\usepackage{ctex}
\usepackage{fontspec}
\setmainfont{Times New Roman}
\CJKfamily{zhsong}

\begin{document}
\title{This is a test for vscode}
\author{Tex\ Live}
\date{2022.06.01}
\maketitle

\begin{abstract}
这里是摘要. 

关键词:\ 这里是关键词\ 这里是关键词. 
\end{abstract}

\tableofcontents
\section{This is a section}
Hello world! 你好，世界！
\subsection{This is a subsection}

\begin{thebibliography}{99}
    \bibitem{a}作者. \emph{文献}[M]. 地点:出版社,年份.
    \bibitem{b}作者. \emph{文献}[M]. 地点:出版社,年份.
\end{thebibliography}

\end{document}
```

