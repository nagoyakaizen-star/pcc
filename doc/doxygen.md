# Q1 pcc フォルダの中にあるソースコードをdoxygenとGraphvizで表示させるスクリプトを示してください。
A1 by gemini

`pcc` フォルダ内にあるソースコードを Doxygen と Graphviz（`dot` ツール）を使って解析し、関数呼び出しグラフやクラス階層図を含んだドキュメントを自動生成する一連のスクリプト（シェルスクリプト例）を提示します。

事前に `doxygen` と `graphviz` がシステムにインストールされている必要があります。

---

### 自動実行スクリプト（`generate_docs.sh`）

このスクリプトは、設定ファイル（Doxyfile）を一時的に生成・書き換えて、Graphvizを有効にした状態で Doxygen を実行します。

```bash
#!/bin/bash

# 対象となるpccフォルダのパス（カレントディレクトリにpccがある場合）
TARGET_DIR="./pcc"
OUTPUT_DIR="./doxygen_output"

# 1. 各種ツールの存在チェック
if ! command -v doxygen &> /dev/null; then
    echo "エラー: doxygen がインストールされていません。"
    exit 1
fi

if ! command -v dot &> /dev/null; then
    echo "エラー: graphviz (dot) がインストールされていません。"
    exit 1
fi

if [ ! -d "$TARGET_DIR" ]; then
    echo "エラー: 対象の $TARGET_DIR フォルダが存在しません。"
    exit 1
fi

echo "--- Doxygen 設定ファイルを生成中 ---"
# ベースとなる Doxyfile を生成
doxygen -g temp_Doxyfile > /dev/null

# 2. Graphviz と日本語対応、各種グラフ用の設定を上書き・追記
cat << EOF >> temp_Doxyfile

# 基本設定
PROJECT_NAME           = "PCC Source Code Analysis"
OUTPUT_DIRECTORY       = ${OUTPUT_DIR}
OUTPUT_LANGUAGE        = Japanese
INPUT                  = ${TARGET_DIR}
RECURSIVE              = YES
EXTRACT_ALL            = YES
EXTRACT_PRIVATE        = YES
EXTRACT_STATIC         = YES

# C/C++向け最適化（もしpccがC系言語の場合。他言語ならNOに）
OPTIMIZE_OUTPUT_FOR_C  = YES

# Graphviz (dot) 連動設定
HAVE_DOT               = YES
DOT_NUM_THREADS        = 4
UML_LOOK               = YES

# 生成するグラフの選択
CLASS_DIAGRAMS         = YES
INCLUDE_GRAPH          = YES
INCLUDED_BY_GRAPH      = YES
CALL_GRAPH             = YES
CALLER_GRAPH           = YES
DIRECTORY_GRAPH        = YES

# その他グラフィック調整（インタラクティブなSVGにすると拡大縮小が便利）
INTERACTIVE_SVG        = YES
DOT_IMAGE_FORMAT       = svg
DOT_GRAPH_MAX_NODES    = 200
EOF

echo "--- Doxygen 解析を実行中 (Graphvizによる描画を含みます) ---"
# 3. Doxygen の実行
doxygen temp_Doxyfile

# 4. 一時ファイルの削除
rm temp_Doxyfile

echo "--------------------------------------------------"
echo "完了しました！"
echo "ブラウザで次のファイルを開いて結果を確認してください:"
echo " ${OUTPUT_DIR}/html/index.html"
echo "--------------------------------------------------"

```

---

### 使い方

1. **スクリプトファイルの作成:** Linux / macOS / WSL 環境.
上記のコードを `generate_docs.sh` という名前で、`pcc` フォルダと同じ階層に保存します。


2. **実行権限の付与:**
ターミナルを開き、以下のコマンドでスクリプトに実行権限を与えます。

```bash
chmod +x generate_docs.sh

```


3. **スクリプトの実行:**
スクリプトを実行します。

```bash
./generate_docs.sh

```


4. **結果の確認:**
処理が完了すると、新しく `doxygen_output` フォルダが作成されます。その中の `html/index.html` をダブルクリックしてブラウザで開いてください。


---

### 生成される図（グラフ）の見どころ

スクリプト内で `INTERACTIVE_SVG = YES` を指定しているため、ブラウザ上で関数やクラスの枠をクリックすると該当のソースコードやリファレンスへジャンプでき、ドラッグや拡大縮小もスムーズに行えます。

* **コールグラフ（Call Graph）**: 関数 A がどの関数を呼び出しているかが一目でわかります。
* **被呼び出しグラフ（Caller Graph）**: その関数が「どこから呼ばれているか」を逆引きできます。
* **インクルード依存関係図**: ヘッダーファイル（`.h`）の参照関係を可視化します。

> **大規模コードでの注意点:**
> もし `pcc` 内のコード量が膨大で解析が止まったり、画像が巨大化しすぎたりする場合は、設定した `DOT_GRAPH_MAX_NODES = 200`（描画する最大ノード数）の数値を小さく調整してみてください。

