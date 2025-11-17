```
vector<long long> manacher(const string &s){
    int i = 0, j = 0;
    vector<long long> r(s.size(), 0);
    while(i<s.size()){
        while(i-j>=0 && i+j<s.size() && s[i-j]==s[i+j]) j++;
        r[i] = j;
        int k = 1;
        while(i-k>=0 && k+r[i-k]<j) r[i+k] = r[i-k], k++;
        i += k; j -= k;
    }
    return r;
}
```

---
| 内容 | コード | 計算量 |
| --- | --- | --- |
| $i$文字目を中心とする最長の回文半径を記録した配列の取得 | `r = manacher(S)` | $O(\|S\|)$ |

偶数長の回文を判定する場合はダミーを交互に挿入する。
(例: `abba` → `#a#b#b#a#`)