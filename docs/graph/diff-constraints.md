# 差分约束

???+ note "参考代码"

    ```cpp
    #include<bits/stdc++.h>
    using namespace std;
    #define add(x,y,z) edge[++tail]=(dd){head[x],y,z},head[x]=tail;
    int n,ml,md;
    int head[1005],tail;
    int dis[1005],cnt[1005];
    bool v[1005];
    struct dd{
        int ne,to,w;
    }edge[100020];
    int spfa(int t){
        memset(dis,0x3f,sizeof(dis));
        memset(v,0,sizeof(v));
        memset(cnt,0,sizeof(cnt));
        queue<int> q;
        q.push(t);
        dis[t]=0;
        while (!q.empty())
        {
            int x=q.front();
            q.pop();
            cnt[x]++;
            if(cnt[x]>n)return -1;
            v[x]=0;
            for(int i=head[x];i;i=edge[i].ne){
                int val=dis[x]+edge[i].w;
                if(dis[edge[i].to]>val){
                    dis[edge[i].to]=val;
                    if(!v[edge[i].to]){
                        q.push(edge[i].to);
                        v[edge[i].to]=1;
                    }
                }
            }
        }
        if(dis[n]==0x3f3f3f3f)return -2;
        return dis[n];
    }
    int main(){
        scanf("%d%d%d",&n,&ml,&md);
        for(int i=1,x,y,z;i<=ml;++i){
            scanf("%d%d%d",&x,&y,&z);
            if(x>y)swap(x,y);
            add(x,y,z)
        }
        for(int i=1,x,y,z;i<=md;++i){
            scanf("%d%d%d",&x,&y,&z);
            if(x>y)swap(x,y);
            add(y,x,-z)
        }
        for(int i=2;i<=n;++i)
        add(i,i-1,0)
        for(int i=1;i<=n;++i)
        add(0,i,0)
        (spfa(0)<0)?printf("-1"):printf("%d",spfa(1));
        return 0;
    }
    ```
