# 粉体工学会 講演要旨 非公式 LaTeX フォーマット
粉体工学会（SPTJ）の講演要旨 Word 体裁を **LaTeX（LuaLaTeX + LuaTeX-ja）で忠実再現**したテンプレートです。  
A4／10pt／二段（22 文字 × 48 行基準）、見出し・キャプション・フッタなどをスタイル一括指定しています。  
**LuaLaTeX 専用**です（`ltjsarticle` 使用）。

---

## 目次

- [特徴](#特徴)
- [動作要件](#動作要件)
- [インストール（簡易）](#インストール簡易)
- [プロジェクト構成](#プロジェクト構成)
- [ビルド方法（コンパイル）](#ビルド方法コンパイル)
- [本文の書き方](#本文の書き方)
- [図・表の挿入](#図表の挿入)
- [参照（\refFig, \refEq, \refTab, \refSec）](#参照-reffig-refeq-reftab-refsec)
- [参考文献の書き方](#参考文献の書き方)
- [章・節の入れ方](#章節の入れ方)
- [Tips / よくある質問](#tips--よくある質問)
- [ライセンス](#ライセンス)

---

## 特徴

- `ltjsarticle` による **日本語二段組**テンプレート
- **フォント自動切替**（Times New Roman / TeX Gyre Termes、和文は HaranoAji / ヒラギノ / IPAex）
- 余白・行送り・段間・キャプション・ラベル表記（`Fig.` / `Table`）を学会体裁に最適化
- `\refFig`, `\refEq`, `\refTab`, `\refSec` による **参照マクロ**
- `booktabs`, `siunitx`, `threeparttable` など表支援パッケージ
- `cite` による **数値引用**（全角ブラケット対応）
- 1 ページ目のフッタ専用ページスタイル（事務局名・連絡先表示）

---

## 動作要件

- **TeX Live 2022 以降**（推奨：最新）  
  - `lualatex` / `latexmk`  
  - パッケージ：`luatexja`, `luatexja-fontspec`, `fontspec`, `amsmath`, `amssymb`, `bm`, `cite`, `etoolbox`,  
    `booktabs`, `siunitx`, `threeparttable`, `graphicx`, `caption`, `titlesec`, `fancyhdr`, `atbegshi`
- OS：Windows / macOS / Linux

---

## インストール（簡易）

### macOS（Homebrew）
```bash
brew install --cask mactex-no-gui
sudo tlmgr update --self --all
```

### Windows（TeX Live）
1. TeX Live インストーラから「TeX Live（フル推奨）」を導入  
2. 「TeX Live Manager」で `tlmgr update --self --all`

### Linux（Debian/Ubuntu 例）
```bash
sudo apt-get install texlive-full
```

> 日本語フォントは同梱の自動検出で HaranoAji／ヒラギノ／IPAex のいずれかを使います。

---

## プロジェクト構成

```
.
├─ main.tex            # あなたの論文本文（例：このリポのテンプレート）
├─ img/                # 図版フォルダ（\graphicspath{ {img/} } 済）
│  └─ voigt_model.pdf  # 図の例
└─ README.md
```

`main.tex` に本テンプレートのプリアンブルが含まれています。

---

## ビルド方法（コンパイル）

### 1) 1 回だけコンパイル（試すだけ）
```bash
lualatex -interaction=nonstopmode main.tex
```

### 2) 推奨：`latexmk`（自動再コンパイル・索引・参考文献対応）
```bash
latexmk -lualatex -interaction=nonstopmode -shell-escape main.tex
```

### 3) 生成物の掃除
```bash
latexmk -c
# さらに PDF 以外を全消し
latexmk -C
```

---

## 本文の書き方

- 二段組（`twocolumn`）が既定です。1 ページ目の二段ヘッダ（研究報告・題目・著者）はテンプレ内のコマンド群で出力。
- 英数字は半角、本文は 10pt、行数は 48 行基準。
- 図版は `img/` に置けば `\includegraphics{ファイル名}` で読み込めます。

**例：1 ページ目のヘッダ**
```latex
\thispagestyle{firstpage}
\twocolumn[{
  \vspace*{\GridBase}
  \ReportType{「研究報告」*10ポイント以上}
  \vspace{.5\baselineskip}
  \TitleLine{講演要旨集執筆要綱（14\,ポイント）}
  \vspace{.5\baselineskip}
  \AuthorsLine{所属\quad ○氏名，氏名，氏名}
  \vspace{.5\baselineskip}
  \vspace*{-7mm}
}]
```

---

## 図・表の挿入

### 単段幅の図（本文幅＝カラム幅）
テンプレではキャプション体裁を統一するため、`captionof{figure}` を用いた例を示します。
```latex
\begin{center}
  \vspace*{.25\baselineskip}
  \includegraphics[width=\columnwidth]{voigt_model}
  \captionof{figure}{Schematic of ...}
  \label{fig:voigt}
\end{center}
```

### 二段貫通図（紙面幅いっぱい）
LaTeX 標準の `figure*` も使えます。
```latex
\begin{figure*}[t]
  \centering
  \includegraphics[width=\textwidth]{big_figure}
  \caption{Wide figure over two columns.}
  \label{fig:wide}
\end{figure*}
```

### 表（三線表 + 単位）
```latex
\begin{table}[t]
  \centering
  \begin{threeparttable}
    \caption{Simulation parameters.}
    \label{tab:params}
    \begin{tabular}{@{}llSS@{}}
      \toprule
      Symbol & Parameter & {Value} & {Units} \\
      \midrule
      $\rho$ & density & 2.50e3 & \si{\kilogram\per\meter\cubed} \\
      \bottomrule
    \end{tabular}
  \end{threeparttable}
\end{table}
```

> `\figurename` を `Fig.`、`\tablename` を `Table` に統一済み。  
> キャプションは「Fig. 1. …」「Table 1. …」という英語風表記になります。

---

## 参照（`\refFig`, `\refEq`, `\refTab`, `\refSec`）

テンプレで以下の参照マクロを定義済みです：

- `\refSec{<label>}` → “Section 2”
- `\refFig{<label>}` → “Fig. 1”
- `\refTab{<label>}` → “Table 1”
- `\refEq{<label>}` → “Eq. (1)”

**例：**
```latex
% 図を参照
As shown in \refFig{fig:voigt}, ...

% 式のラベルと参照
\begin{equation}
  \bm F_n = \bigl(k_n\,\delta_n - \gamma_n\, v_n\bigr)\,\bm n .
  \label{eq:Fn}
\end{equation}
The normal force is given by \refEq{eq:Fn}.

% 表・節の参照
See parameters in \refTab{tab:params} and details in \refSec{sec:method}.
```

---

## 参考文献の書き方

### 1) テンプレの **thebibliography**（手書き・数値引用）
テンプレ既定は `\usepackage{cite}` による **数値引用**です（全角ブラケット化済み）。  
本文中で：
```latex
... as shown in DEM studies \cite{Cundall1979,Iwashita1998}.
```
本文末：
```latex
\begin{thebibliography}{99}
  \bibitem{Cundall1979}
  P. A. Cundall and O. D. L. Strack, G\'eotechnique, \textbf{29} (1979) 47--65.
\end{thebibliography}
```

### 2) （任意）**BibTeX** を使いたい場合
`refs.bib` を用意し、本文末を以下に変更：
```latex
\bibliographystyle{unsrt} % 数値順。学会規定に合わせて変更可
\bibliography{refs}
```
コンパイルは：
```bash
latexmk -lualatex main.tex    # latexmk が自動で bibtex を実行
```

---

## 章・節の入れ方

- 見出し体裁はテンプレで `titlesec` により調整済みです（行送りグリッド維持）。
- 基本は `\section{…}`, `\subsection{…}` を使用（番号付き）。

```latex
\section{緒言}\label{sec:intro}
本文...

\subsection{法線方向}
本文...
```

> 見出し前後の余白はグリッドを崩さないようゼロに設定済み。  
> さらに小さい粒度が必要なら `\subsubsection` も利用できます（体裁調整は各自）。

---

## Tips / よくある質問

- **LuaLaTeX 以外で通りません**：`pdflatex` や `xelatex` は非対応です。必ず `lualatex` を使ってください。
- **フォントが違う**：Times / HaranoAji などが無ければ、代替（TeX Gyre Termes / IPAex）に自動フォールバックします。
- **図が見つからない**：`img/` を既定の検索パスにしています。別フォルダを使う場合は `\graphicspath{{path/}}` を調整。
- **二段図が意図通り出ない**：`figure*` は配置制約が厳しいため、ページ上部（`[t]`）や文中の `\twocolumn[...]` 直後などで試してください。
- **段間が狭い／広い**：プリアンブルの `\setlength{\columnsep}{...}` と `\setlength{\ReduceCenterBy}{...}` を調整できます。

---

## ライセンス

- 本テンプレートは **MIT License** を推奨します（必要に応じて変更してください）。
- SPTJ/粉体工学会の公式テンプレートではありません。本リポは **研究者個人が Word 体裁を LaTeX で再現**したものです。

---

## クイックスタート（最短手順）

```bash
# 1) 依存を整える（TeX Live 更新推奨）
tlmgr update --self --all

# 2) コンパイル
latexmk -lualatex -interaction=nonstopmode -shell-escape main.tex

# 3) 出力: main.pdf
```

---

## 貢献

Issue / PR 歓迎です。体裁やマクロ追加（例：和文括弧や数式番号のカスタム等）も気軽に相談してください。
