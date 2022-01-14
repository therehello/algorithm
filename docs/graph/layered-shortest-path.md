# 分层最短路

???+ note "参考代码"

    ```cpp
    #include<bits/stdc++.h>
    using namespace std;
    struct dd{
        int ne,to,w;
    }edge[4105050];
    int n,m,k,head[212020],tail,dis[212020];
    bool v[212020];
    #define add(x,y,z) edge[++tail].ne=head[x];edge[tail].to=y;edge[tail].w=z;head[x]=tail;
    void dij(){
        priority_queue<pair<int,int> >q;
        q.push(make_pair(0,1));
        while(!q.empty()){
            int x=q.top().second;
            q.pop();
            if(v[x])continue;
            v[x]=1;
            for(int i=head[x];i;i=edge[i].ne){
                int diss=dis[x]+edge[i].w;
                if(dis[edge[i].to]>=diss){
                    dis[edge[i].to]=diss;
                    q.push(make_pair(-diss,edge[i].to));
                }
            }
        }
    }
    int main(){
        scanf("%d%d%d",&n,&m,&k);
        for(int i=1,x,y,z;i<=m;++i){
            scanf("%d%d%d",&x,&y,&z);
            add(x,y,z);
            add(y,x,z);
            for(int j=1;j<=k;++j){
                add(x+(j-1)*n,y+j*n,0);
                add(y+(j-1)*n,x+j*n,0);
                add(y+j*n,x+j*n,z);
                add(x+j*n,y+j*n,z);
            } 
        }
        for(int i=1;i<=n*(k+1);++i)
        dis[i]=0x7fffffff;
        dis[1]=0;
        dij();
        int ans=0x7fffffff;
        for(int i=n;i<=n*(k+1);i+=n)
        ans=min(ans,dis[i]);
        printf("%d",ans);
    }
    ```
