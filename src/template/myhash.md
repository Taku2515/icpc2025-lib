ハッシュの生成は以下のコマンドで行う
```
g++ -dD -E -P -fpreprocessed - < {ファイル名} | tr -d '[:space:]' | md5sum | cut -c-6
```
必ずmarkdownに記載されているコードのみを記述したcppファイルに対してハッシュを生成すること．