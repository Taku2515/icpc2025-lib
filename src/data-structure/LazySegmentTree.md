// myhash: 0d1da5
```
template <typename X, typename M>
struct LazySegTree{

    using FX = function<X(X, X)>;
    using FA = function<X(X, M)>;
    using FM = function<M(M, M)>;
    int n;
    FX fx; FA fa; FM fm;
    const X ex; const M em;
    vector<X> dat; vector<M> lazy;

    LazySegTree(int n_in, FX fx_in, FA fa_in, FM fm_in, X ex_in, M em_in)
    : n(), fx(fx_in), fa(fa_in), fm(fm_in), ex(ex_in), em(em_in), dat(n_in*4, ex), lazy(n_in*4, em){
        int x = 1;
        while(n_in>x) x *= 2;
        n = x;
    }

    void set(int i, X x) {dat[i+n-1] = x;}
    void build(){
        for(int k=n-2; k>=0; k--) dat[k] = fx(dat[2*k+1], dat[2*k+2]);
    }

    void eval(int k){
        if(lazy[k]==em) return;
        if(k<n-1){
            lazy[2*k+1] = fm(lazy[2*k+1], lazy[k]);
            lazy[2*k+2] = fm(lazy[2*k+2], lazy[k]);
        }
        dat[k] = fa(dat[k], lazy[k]);
        lazy[k] = em;
    }

    void update_sub(int a, int b, M x, int k, int l, int r){
        eval(k);
        if(a<=l && r<=b){
            lazy[k] = fm(lazy[k], x);
            eval(k);
        }
        else if(a<r && l<b){
            update_sub(a, b, x, 2*k+1, l, (l+r)/2);
            update_sub(a, b, x, 2*k+2, (l+r)/2, r);
            dat[k] = fx(dat[2*k+1], dat[2*k+2]);
        }
    }
    void update(int a, int b, M x) {update_sub(a, b, x, 0, 0, n);}

    X query_sub(int a, int b, int k, int l, int r){
        eval(k);
        if(r<=a || b<=l) return ex;
        else if(a<=l && r<=b) return dat[k];
        else{
            X vl = query_sub(a, b, 2*k+1, l, (l+r)/2);
            X vr = query_sub(a, b, 2*k+2, (l+r)/2, r);
            return fx(vl, vr);
        }
    }
    X query(int a, int b) {return query_sub(a, b, 0, 0, n);}
    
};
```
---
| 内容 | コード | 計算量 |
| --- | --- | --- |
| 宣言(要素数n) | `LazySegTree<X, M> seg(n, fx, fa, fm, ex, em);`| $O(N)$ |
| i番目の値をaに設定(初期化用) | `seg.set(i, 0)` | $O(1)$ |
| 設定した値を適用(初期化用) | `seg.build()` | $O(n)$ |
| [l, r)に対して定義したクエリの実行 | `seg.query(l, r)` | $O(\log_{}n)$ |
| [l, r)に対して定義した更新処理をxを使って実行 | `seg.update(l, r, x)` | $O(\log_{}n)$ |

---
区間変更・区間最小値取得
---
```
using X = long long;
using M = long long;
const long long INF = 2e18;
auto fx = [](X x1, X x2) -> X{return min(x1, x2);};
auto fa = [](X x, M m) -> X{return m==INF ? x : m;};
auto fm = [](M m1, M m2) -> M{return m2==INF ? m1 : m2;};
long long ex = INF;
long long em = INF;
LazySegTree<X, M> seg(n, fx, fa, fm, ex, em);
```
---
区間加算・区間合計取得
---
```
struct S{long long value; int size;};
using X = S;
using M = long long;
auto fx = [](X x1, X x2) -> X{return {x1.value+x2.value, x1.size+x2.size};};
auto fa = [](X x, M m) -> X{return {x.value+m*x.size, x.size};};
auto fm = [](M m1, M m2) -> M{return m1+m2;};
X ex = {0, 0};
M em = 0;
LazySegTree<X, M> seg(n, fx, fa, fm, ex, em);
```
`seg.set`は`seg.set(インデックス, {設定したい値, 1})`とする。

---
区間加算・区間最小値取得
---
```
using X = long long;
using M = long long;
const long long INF = 2e18;
auto fx = [](X x1, X x2) -> X{return min(x1, x2);};
auto fa = [](X x, M m) -> X{return x+m;};
auto fm = [](M m1, M m2) -> M{return m1+m2;};
long long ex = INF;
long long em = 0;
LazySegTree<X, M> rmq(n, fx, fa, fm, ex, em);
```
---
区間変更・区間合計取得
---
```
struct S{long long value; int size;};
using X = S;
using M = long long;
const long long INF = 8e18;
auto fx = [](X x1, X x2) -> X{return {x1.value+x2.value, x1.size+x2.size};};
auto fa = [](X x, M m) -> X{return (m==INF ? x : S{m*x.size, x.size});};
auto fm = [](M m1, M m2) -> M{return (m2==INF ? m1 : m2);};
X ex = {0, 0};
M em = INF;
LazySegTree<X, M> seg(n, fx, fa, fm, ex, em);
```
---
区間変更・区間最大値取得
---
```
using X = long long;
using M = long long;
const long long N_INF = -2e18; 
const long long ID = 8e18; 
auto fx = [](X x1, X x2) -> X{return max(x1, x2);};
auto fa = [](X x, M m) -> X{return (m == ID ? x : m);};
auto fm = [](M m1, M m2) -> M{return (m2 == ID ? m1 : m2);};
X ex = N_INF;
M em = ID;
LazySegTree<X, M> seg(n, fx, fa, fm, ex, em);
```
---
区間加算・区間最大値取得
---
```
using X = long long;
using M = long long;
const long long N_INF = -2e18;
auto fx = [](X x1, X x2) -> X{return std::max(x1, x2);};
auto fa = [](X x, M m) -> X{return x + m;};
auto fm = [](M m1, M m2) -> M{return m1 + m2;};
X ex = N_INF;
M em = 0;
LazySegTree<X, M> seg(n, fx, fa, fm, ex, em);
```