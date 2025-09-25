# README

粉体工学会（SPTJ）の **Word 体裁を LaTeX（LuaLaTeX + LuaTeX-ja）で再現**したテンプレートです.   
A4／10pt／二段（22 文字 × 48 行の目安）. 見出し・キャプション・フッタなどの見た目をまとめて設定しています.   
**LuaLaTeX 専用**です（`ltjsarticle` 使用）. 

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

## LaTeX で作成した理由
粉体工学会の Word 体裁を, LuaLaTeX と LuaTeX-ja で忠実に再現するフォーマットを有志で作成しました. これは単なるテンプレートではありません. 論文作成を道具の不安定さから解き放ちたい――その意思表示です. 率直に言えば, Word は学術論文に向いていません. 図表・数式・参考文献といった基盤で, 私はあまりにも多くの時間と注意力を失いました. 

まず参照です. 節や図表の位置を少し動かしただけでリンクが壊れ, 図表番号が更新されない場面に何度も遭遇しました. 共同編集では環境差が平然と紛れ込み, 本文に「図 3」と書いてあるのに実体は図 2, という事故が起きます. 版が進むほど整合性チェックのクリック作業が積み上がり, 提出直前まで神経を削ります. これは執筆ではなく, 復旧作業です. 

数式も悩ましいところでした. 上付き・下付きが小さく, 行間や字面のバランスが崩れ, 複雑な分数や行列ほど読みにくくなります. ページをまたぐと式番号の位置が揺れ, 主張の可読性が落ちます. 参考文献も同様で, 引用順を入れ替えたのに番号が揃わない, といった齟齬が平気で起きます. 本来は仕様で機械的に保証されるべき部分が, 人間の手作業に委ねられていると感じます. 

レイアウトは非決定的です. 同じファイルでも端末が変わるだけで改ページや段組がずれ, 見出しやキャプションが孤立します. 図のアンカーは些細な編集で暴れ, 本文との距離が勝手に開きます. 形式が混在したファイルは保存や貼り付けのたびに挙動が重くなり, 最悪は固まります. 「同じ入力から同じ出力が得られるのか」という基本的な信頼が揺らぎます. 形式が混在したファイルは保存や貼り付けのたびに挙動が重くなり, 最悪は突然落ちます. 有償ソフトのくせに, ここまで基本的な安定性を信頼できない状況に何度も悩まされました.

だから私は LaTeX を使っています. 余白・行送り・段間・キャプション・ラベル・フォントといった仕様をプリアンブルで固定します. 本文は \label と \ref で参照を一貫させ, 図表・式・節番号は移動や追加・削除に自動で追随します. 引用もコマンドで一括管理し, 本文と末尾の対応がずれません. 数式は amsmath などの成熟した仕組みで字面と行間の均衡が保たれ, 読みやすさと美しさが両立します. 入力が同じなら, どこでビルドしても同じ PDF が得られます. 

このフォーマットは, 怒りから生まれました. 論文の価値は内容にあり, 体裁は機械が守るべきです. にもかかわらず, 私は長く, 図表番号の手直しや参照の突合, 崩れるレイアウトの鎮火に時間を奪われてきました. Word の運任せな組版から降り, 体裁を仕様として固定し, LaTeX で共有する道を選びます. 論文は内容で戦い, 体裁は機械に任せる. その当たり前を取り戻したくて, このフォーマットを置きます. 




## 特徴

- `ltjsarticle` による **日本語二段組**
- **フォント自動切替**（Times New Roman / TeX Gyre Termes, 和文は HaranoAji / ヒラギノ / IPAex）
- 余白・行送り・段間・キャプション・ラベル表記（`Fig.` / `Table`）を学会体裁に合わせて調整
- `\refFig`, `\refEq`, `\refTab`, `\refSec` の **参照マクロ**
- 表づくりを助ける `booktabs`, `siunitx`, `threeparttable`
- `cite` による **数値引用**（全角括弧対応）
- 1 ページ目だけフッタを切り替えるページスタイル（事務局名・連絡先）

![Word と LaTeX の体裁比較](img/word_vs_latex.png)

*Fig. 1. Word と LaTeX の体裁比較*

---

## 動作要件

- **TeX Live 2022 以降**（できれば最新）
  - エンジン：`lualatex`
  - 主なパッケージ：`luatexja`, `luatexja-fontspec`, `fontspec`, `amsmath`, `amssymb`, `bm`, `cite`,  
    `etoolbox`, `booktabs`, `siunitx`, `threeparttable`, `graphicx`, `caption`, `titlesec`, `fancyhdr`, `atbegshi`
- OS：Windows / macOS / Linux

---

## インストール（簡易）

### macOS（Homebrew）
```bash
brew install --cask mactex-no-gui
sudo tlmgr update --self --all
```

### Windows（TeX Live）
1. TeX Live インストーラで「TeX Live（フル推奨）」を導入  
2. 「TeX Live Manager」で更新（`tlmgr update --self --all`）

### Linux（Debian/Ubuntu 例）
```bash
sudo apt-get install texlive-full
```

> 日本語フォントは自動検出で HaranoAji／ヒラギノ／IPAex のいずれかを使います. 

---

## プロジェクト構成

```
.
├─ main.tex            # あなたの本文（このテンプレの例）
├─ img/                # 図フォルダ（\graphicspath{ {img/} } 済）
│  └─ voigt_model.pdf  # 図の例
└─ README.md
```

`main.tex` にテンプレのプリアンブルが入っています. 

---

## ビルド方法（コンパイル）

**LuaLaTeX を 2 回**流してください（参照や目次を正しく更新するため）. 

```bash
lualatex main.tex
lualatex main.tex
```

出力：`main.pdf`

---

## 本文の書き方

- 二段組（`twocolumn`）が既定です. 1 ページ目のヘッダ（研究報告・題目・著者）はテンプレ内のコマンドで出力します. 
- 英数字は半角, 本文は 10pt, 行数は 48 行の目安です. 
- 図は `img/` に置き, `\includegraphics{ファイル名}` で読み込みます. 

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

### 単段幅の図（カラム幅）
キャプション体裁をそろえるため, `captionof{figure}` を使った例です. 
```latex
\begin{center}
  \vspace*{.25\baselineskip}
  \includegraphics[width=\columnwidth]{voigt_model}
  \captionof{figure}{Schematic of ...}
  \label{fig:voigt}
\end{center}
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

> `\figurename` は `Fig.`, `\tablename` は `Table` に統一しています.   
> キャプションは「Fig. 1. …」「Table 1. …」の表記になります. 

---

## 参照（`\refFig`, `\refEq`, `\refTab`, `\refSec`）

テンプレで次の参照マクロを定義しています. 

- `\refSec{<label>}` → “Section 1”
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

### thebibliography（手書き・数値引用）
既定は `\usepackage{cite}` による **数値引用**です（全角括弧対応）. 

本文中：
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

---

## 章・節の入れ方

- 見出しの見た目は `titlesec` で調整済み（行送りのグリッドを維持）. 
- 基本は `\section{…}`, `\subsection{…}` を使います（番号付き）. 

```latex
\section{緒言}\label{sec:intro}
本文...

\subsection{法線方向}
本文...
```

> 見出し前後の余白はグリッドを崩さないよう最小化しています.   
> さらに細かく分けたいときは `\subsubsection` も使えます（体裁は各自で微調整）. 

---

## Tips / よくある質問

- **LuaLaTeX 以外は非対応**：`pdflatex` や `xelatex` ではビルドできません. `lualatex` を使ってください. 
- **フォントが違う**：Times / HaranoAji がなければ, TeX Gyre Termes / IPAex などに自動で切り替わります. 
- **図が見つからない**：`img/` を既定の検索パスにしています. 別フォルダを使う場合は `\graphicspath{{path/}}` を調整してください. 

---

## ライセンス

- 本テンプレートは **MIT License** . 
- 本テンプレートは SPTJ／粉体工学会の公式テンプレートではありません. Word の体裁を, 個人が LaTeX で再現した非公式版です. これまでに本テンプレートを用いた原稿が要旨集に採用された実績はありますが, ご利用は自己責任にてお願いいたします. 
