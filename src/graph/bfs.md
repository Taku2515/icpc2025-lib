// myhash: e8220a
```
using ll = long long;
vector<ll> bfs(ll start, vector<vector<ll>> &g){
    int n = g.size();
    queue<ll> q;
    vector<ll> dist(n, -1);
    q.push(start); dist.at(start) = 0;
    while(!q.empty()){
        ll v = q.front(); q.pop();
        for(ll nv: g.at(v)){
            if(dist.at(nv)!=-1) continue;
            dist.at(nv) = dist.at(v) + 1;
            q.push(nv);
        }
    }
    return dist;
}
```
---

| 内容 | コード | 計算量 |
| --- | --- | --- |
| 頂点数nのグラフの宣言 | `vector<vector<ll>> g; g.resize(n);` |  |
| 頂点sからグラフgに対してbfsの実行 | `bfs(s, g)` | $O(\|E\|+\|V\|)$ |
| u→vの辺を追加 | `g[u].push_back(v)` |  |

(ただし$|E|$は辺の数, $|V|$は頂点の数.)
