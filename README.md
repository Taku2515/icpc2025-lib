# ICPC notebook

## 環境構築

### 最低限やること
```bash
git clone git@github.com:Taku2515/icpc2025-lib.git
cd icpc2025-lib
```

### ローカルでpdfを作成する場合
`src/`（と必要であれば`build/`）に変更を加えれば、mainブランチにpushした段階で、pdfは最新版になる（CIが自動でpdfを build・commit・push を行うため）。
そのため、ローカルでpdfを生成しない場合は以下のものをインストールしなくて良い。

- node.js (v18 以上)
- npm
    - `brew install node` / <https://learn.microsoft.com/ja-jp/windows/dev-environment/javascript/nodejs-on-wsl>
- clang-format
    - `brew install clang-format` / `sudo apt install clang-format`
- vivliostyle
    - `npm install -g @vivliostyle/cli`
- その他依存関係
    - `npm install`

- pdf作成方法
    - `make build`で作成できる。


## 作業フロー

1. [src/\*/\*](src/) の中身を変更する
2. [build/build.js](build/build.js) の設定項目を変更する
3. commit & push

### その他

- [Makefile](Makefile) で clang-format による自動フォーマットを行っている。フォーマットの設定は [.clang-format](.clang-format) で変更できる。
