// myhash: 908dcc
```
using ll = long long;
using pll = pair<ll, ll>;
const ll INF = 2e18;
vector<ll> dijkstra(long long start, vector<vector<pll>> &g){
    priority_queue<pll, vector<pll>, greater<pll>> pq;
    int n = g.size();
    vector<ll> dist(n, INF);
    dist[start] = 0; pq.push({0, start});
    while(!pq.empty()){
        auto [d, pos] = pq.top(); pq.pop();
        if(dist[pos] < d) continue;
        for(int i=0; i<g[pos].size(); i++){
            auto [cost, to] = g[pos][i];
            if(dist[to]>dist[pos]+cost){
                dist[to] = dist[pos] + cost;
                pq.push({dist[to], to});
            }
        }
    }
    return dist;
}
```
---

| 内容 | コード | 計算量 |
| --- | --- | --- |
| 頂点数nのグラフの宣言 | `vector<vector<pll>> g; g.resize(n);` | |
| 頂点sからグラフgに対してダイクストラの実行 | `dijkstra(s, g)` | $O(\|E\|log \|V\|)$ |
| u→v(コスト: c)の辺を追加 | `g[u].push_back({c, v})` | |

(ただし$|E|$は辺の数, $|V|$は頂点の数.)
