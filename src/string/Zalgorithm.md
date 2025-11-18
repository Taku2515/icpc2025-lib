```
vector<long long> z_algorithm(const string &s){
    vector<long long> z(s.size(), 0);
    z[0] = s.size();
    int i = 1, j = 0;
    while(i<s.size()){
        while(i+j<s.size() && s[j]==s[i+j]) j++;
        z[i] = j;
        if(j==0){ i++; continue; }
        int k = 1;
        while(i+k<s.size() && k+z[k]<j) z[i+k] = z[k], k++;
        i += k; j -= k;
    }
    return z;
}
```
---

| 内容 | コード | 計算量 |
| --- | --- | --- |
| `z[i]`=「S」と「Sのi文字目以降」の最長共通接頭辞の長さを計算 | `z = z_algorithm(S)` | $O(\|S\|)$ |
