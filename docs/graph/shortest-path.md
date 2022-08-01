# 最短路

## Dijkstra 算法

???+ note "参考代码"

    ```cpp
    #include<bits/stdc++.h>
    using namespace std;
    struct Graph{
        int n;
        struct Edge{int to,w;};
        vector<vector<Edge>> graph;
        Graph(int _n){n=_n;graph.assign(n+1,vector<Edge>());};
        void add(int u,int v,int w){graph[u].push_back({v,w});}
    };
    void dij(Graph &graph,vector<int> &dis,int t){
        vector<int> visit(graph.n+1,0);
        priority_queue<pair<int,int>> que;
        dis[t]=0;
        que.emplace(0,t);
        while(!que.empty()){
            int u=que.top().second;que.pop();
            if(visit[u])continue;
            visit[u]=1;
            for(auto &[to,w]:graph.graph[u]){
                if(dis[to]>dis[u]+w){
                    dis[to]=dis[u]+w;
                    que.emplace(-dis[to],to);
                }
            }
        }
    }
    void solve(){
        int n,m,t;
        cin>>n>>m>>t;
        Graph g(n);
        for(int i=1,u,v,w;i<=m;i++){
            cin>>u>>v>>w;
            g.add(u,v,w);
        }
        vector<int> dis(n+1,0x3f3f3f3f);
        dij(g,dis,t);
        for(int i=1;i<=n;i++)cout<<dis[i]<<" ";
    }
    int main(){
        ios::sync_with_stdio(false);
        cin.tie(0);cout.tie(0);
        int t=1;
        // cin>>t;
        while(t--)solve();
    }
    ```
