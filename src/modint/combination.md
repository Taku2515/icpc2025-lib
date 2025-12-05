// hash: 81258e
```
const long long MAX = 5e5; // 関数comの引数の最大値
const long long mod = 1e9+7; // 何で割った余りか
vector<long long> fac(MAX), finv(MAX), inv(MAX);

void COMinit(){
    fac[0] = 1; fac[1] = 1;
    finv[0] = 1; finv[1] = 1;
    inv[1] = 1;
    for(int i=2; i<MAX; i++){
        fac[i] = fac[i-1] * i % mod;
        inv[i] = mod - inv[mod%i] * (mod / i) % mod;
        finv[i] = finv[i-1] * inv[i] % mod;
    }
}

long long com(int n, int k){
    if(n<k) return 0;
    if(n<0 || k<0) return 0;
    return fac[n] * (finv[k] * finv[n-k] % mod) % mod;
}
```
---


| 内容 | コード | 計算量 |
| --- | --- | --- |
| 二項係数を求める前処理 | `COMinit()` | $O(MAX)$ |
| 二項係数${}_nC {}_k$ mod $p$の計算 | `com(n, k)` | $O(1)$ |

`com(n, k)`を実行する前に必ず`COMinit()`を実行する（一度だけで良い）。
$1 \leq k \leq n \leq 10^7$ また、mod $p$の$p$は素数かつ $n<p$ となる必要がある。