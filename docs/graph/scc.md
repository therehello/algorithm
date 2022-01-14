# 强连通分量

## Tarjan算法

???+ note "参考代码"

    ```cpp
    #include<bits/stdc++.h>
    using namespace std;
    struct Mystruct {
        int ne,to;
    }edge[50020];
    int head[10020],tail;
    int dfn[10020],low[10020],tar_len[10020],tim_e,num;
    vector<int>tar[10020];
    int n,m;
    bool vis[10020];
    stack<int>q;
    inline void add(int x,int y){
        edge[++tail].ne=head[x];
        edge[tail].to=y;
        head[x]=tail;
    }
    void tarjan(int t){
        low[t]=dfn[t]=++tim_e;
        vis[t]=1;
        q.push(t);
        for(int i=head[t];i;i=edge[i].ne){
            if(!dfn[edge[i].to]){
                tarjan(edge[i].to);
                low[t]=min(low[t],low[edge[i].to]);
            }
            else if(vis[edge[i].to])low[t]=min(low[t],dfn[edge[i].to]);
        }
        if(low[t]==dfn[t]){
            ++num;
            int x=q.top();
            while(x!=t){
                vis[x]=0;
                tar[num].push_back(x);
                ++tar_len[num];
                q.pop();
                x=q.top();
            }
            tar[num].push_back(t);
            ++tar_len[num];
            vis[t]=0;
            q.pop();
        }
    }
    int main(){
        scanf("%d%d",&n,&m);
        for(int i=1,x,y;i<=m;++i){
            scanf("%d%d",&x,&y);
            add(x,y);
        }
        for(int i=1;i<=n;++i)
        if(!dfn[i])tarjan(i);
        int ans=0;
        for(int i=1;i<=num;++i)
            if(tar_len[i]>1)ans++;
        printf("%d",ans);
    }
    ```
