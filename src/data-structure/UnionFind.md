```
struct UnionFind {
    vector<int> parents; // parents[i] := 頂点iのroot
    vector<int> sizes; // sizes[i] := 頂点iが属するグループの要素数
    UnionFind(int n){
        parents = vector<int>(n);
        for(int i=0; i<n; i++) parents.at(i) = i;
        sizes = vector<int>(n, 1);
    }
    int find(int i){
        if(parents.at(i)==i) return i; // 自身のroot == iのとき自分がroot
        return (parents.at(i) = find(parents.at(i))); // 経路圧縮
    }
    void merge(int a, int b){
        a = find(a);
        b = find(b);
        if(a!=b){
            sizes.at(a) += sizes.at(b);
            parents.at(b) = a;
        }
    }
    bool connected(int a, int b){
        return (find(a)==find(b));
    }
    long long size(int i){
        return (long long)sizes.at(find(i));
    }
};
```
---

| 内容 | コード | 計算量 |
| --- | --- | --- |
| 宣言 | `UnionFind uf(N)`| $O(N)$ |
| aの根の取得 | `uf.find(a)` | $O(\alpha(N))$ |
| aとbを統合 | `uf.merge(a, b)` | $O(\alpha(N))$ |
| aとbが同じグループか判定 | `uf.connected(a, b)` | $O(\alpha(N))$ |
| aと同じグループの要素数 | `uf.size(a)` | $O(\alpha(N))$ |

$\alpha(N)$はアッカーマンの逆関数