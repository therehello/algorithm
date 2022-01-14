# 最短路

## Dijkstra 算法

???+ note "参考代码"
    ```cpp
    #include<bits/stdc++.h>
    using namespace std;
    struct dd{
        int ne,to,w;
    }edge[505050];
    int n,m,s,head[101001],tail,dis[101001];
    bool v[101001];
    #define add(x,y,z) edge[++tail]=(dd){head[x],y,z};head[x]=tail;
    void dij(){
        priority_queue<pair<int,int>,vector<pair<int,int> >,greater<pair<int,int> > >q;
        q.push(make_pair(0,s));
        while(!q.empty()){
            int x=q.top().second;
            q.pop();
            if(v[x])continue;
            v[x]=1;
            for(int i=head[x];i;i=edge[i].ne){
                int diss=dis[x]+edge[i].w;
                if(dis[edge[i].to]>=diss){
                    dis[edge[i].to]=diss;
                    q.push(make_pair(diss,edge[i].to));
                }
            }
        }
    }
    int main(){
        scanf("%d%d%d",&n,&m,&s);
        for(int i=1,x,y,z;i<=m;++i){
            scanf("%d%d%d",&x,&y,&z);
            add(x,y,z);
        }
        for(int i=1;i<=n;++i)
        dis[i]=0x7fffffff;
        dis[s]=0;
        dij();
        for(int i=1;i<=n;++i)
        printf("%d ",dis[i]);
    }
    ```