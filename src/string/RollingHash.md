// myhash: 409b35
```
struct RollingHash{
    
    using ll = long long;

    const ll mod = (1ll<<61)-1;
    ll n, rnd;
    vector<ll> hs, pw;
    RollingHash(string s): n(s.size()), hs(n+1), pw(n+1, 1) {
        // rnd = rand()%mod;
        rnd = 93842743347298748;
        for(int i=0; i<n; i++){
            pw[i+1] = mul(pw[i], rnd);
            hs[i+1] = add(mul(hs[i], rnd), s[i]);
        }
    };
    ll add(ll a, ll b){
        return (a+b)%mod;
    }
    ll mul(ll a, ll b){
        auto c = (__uint128_t)a*b;
        return add(c>>61, c&mod);
    }
    ll get(ll l, ll r){
        return add(hs[r], mod - mul(hs[l], pw[r-l]));
    }
};
```
---

| 内容 | コード | 計算量 |
| --- | --- | --- |
| 宣言 | `RollingHash rh(s)` | $O(s.size())$ |
| $[l, r)$のハッシュ値の取得 | `rh.get(l, r)` | $O(1)$ |