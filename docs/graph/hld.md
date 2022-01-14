# 树链剖分

???+ note "参考代码"
    ```cpp
    #include<bits/stdc++.h>
    using namespace std;
    #define add(x,y) edge[++tail]=(dd){head[x],y};head[x]=tail;
    int n,m,root_,mod;
    int val_[101010],head[101010],tail,deep[101010],fa[101010];
    int son[101010],size_[101010],idx[101010],fir[101010],time_,val_new[101010];
    struct dd{
        int ne,to;
    }edge[202020];
    struct tree{
        int sum;
        int tag;
    }tree[404040];
    inline int read(){
        int x=0,f=1;
        char ch=getchar();
        while(ch<'0'||ch>'9'){
            if(ch=='-')f=-1;
            ch=getchar();
        }
        while (ch>='0'&&ch<='9'){
            x=x*10+(ch^48);
            ch=getchar();
        }
        return f*x;
    }
    void dfs1(int t,int faa,int depth){
        deep[t]=depth;
        fa[t]=faa;
        size_[t]=1;
        for(int i=head[t];i;i=edge[i].ne){
            int to=edge[i].to;
            if(to==faa)continue;
            dfs1(to,t,depth+1);
            size_[t]+=size_[to];
            if(size_[to]>size_[son[t]])son[t]=to;
        }
    }
    void dfs2(int t,int fir_){
        idx[t]=++time_;
        val_new[time_]=val_[t];
        fir[t]=fir_;
        if(!son[t])return;
        dfs2(son[t],fir_);
        for(int i=head[t];i;i=edge[i].ne){
            if(!idx[edge[i].to]){
                dfs2(edge[i].to,edge[i].to);
            }
        }
    }
    void build(int pos,int l,int r){
        if(l==r){
            tree[pos].sum=val_new[l]%mod;
            return;
        }
        int mid=l+r>>1,lson=pos<<1;
        build(lson,l,mid);
        build(lson|1,mid+1,r);
        tree[pos].sum=(tree[lson].sum+tree[lson|1].sum)%mod;
    }
    inline void push_down(int pos,int lson,int l,int r,int mid){
        tree[lson].sum=(tree[lson].sum+(mid-l+1)*tree[pos].tag)%mod;
        tree[lson|1].sum=(tree[lson|1].sum+(r-mid)*tree[pos].tag)%mod;
        tree[lson].tag=(tree[lson].tag+tree[pos].tag)%mod;
        tree[lson|1].tag=(tree[lson|1].tag+tree[pos].tag)%mod;
        tree[pos].tag=0;
    }
    void tree_update(int pos,int l,int r,int ll,int rr,int x){
        if(l>=ll&&r<=rr){
            tree[pos].tag=(x+tree[pos].tag)%mod;
            tree[pos].sum=(tree[pos].sum+(r-l+1)*x)%mod;
            return;
        }
        int mid=l+r>>1,lson=pos<<1;
        if(tree[pos].tag)push_down(pos,lson,l,r,mid);
        if(ll<=mid)tree_update(lson,l,mid,ll,rr,x);
        if(mid<rr)tree_update(lson|1,mid+1,r,ll,rr,x);
        tree[pos].sum=(tree[lson].sum+tree[lson|1].sum)%mod;
    }
    int tree_sum(int pos,int l,int r,int ll,int rr){
        if(l>=ll&&r<=rr){
            return tree[pos].sum;
        }
        int ans=0;
        int mid=l+r>>1,lson=pos<<1;
        if(tree[pos].tag)push_down(pos,lson,l,r,mid);
        if(ll<=mid)ans=tree_sum(lson,l,mid,ll,rr)%mod;
        if(mid<rr)ans+=tree_sum(lson|1,mid+1,r,ll,rr)%mod;
        return ans%mod;   
    }
    inline void update(int x,int y,int z){
        while(fir[x]!=fir[y]){
            if(deep[fir[x]]<deep[fir[y]])swap(x,y);
            tree_update(1,1,n,idx[fir[x]],idx[x],z);
            x=fa[fir[x]];
        }
        if(deep[x]>deep[y])swap(x,y);
        tree_update(1,1,n,idx[x],idx[y],z);
    }
    inline int sum_x_y(int x,int y){
        int ans=0;
        while(fir[x]!=fir[y]){
            if(deep[fir[x]]<deep[fir[y]])swap(x,y);
            ans=(ans+tree_sum(1,1,n,idx[fir[x]],idx[x]))%mod;
            x=fa[fir[x]];
        }
        if(deep[x]>deep[y])swap(x,y);
        ans=(ans+tree_sum(1,1,n,idx[x],idx[y]))%mod;
        return ans;
    }
    int main(){
        scanf("%d%d%d%d",&n,&m,&root_,&mod);
        for(int i=1;i<=n;++i)
            val_[i]=read();
        for(int i=1,x,y;i<n;++i){
            x=read();y=read();
            add(x,y);add(y,x);
        }
        dfs1(root_,root_,1);
        dfs2(root_,root_);
        build(1,1,n);
        for(int i=1,x,y,z;i<=m;++i){
            char ch=getchar();
            while(ch<'1'||ch>'4')ch=getchar();
            if(ch=='1'){
                x=read();y=read();z=read();
                update(x,y,z);
            }
            if(ch=='2'){
                x=read();y=read();
                printf("%d\n",sum_x_y(x,y));
            }
            if(ch=='3'){
                x=read();z=read();
                tree_update(1,1,n,idx[x],idx[x]+size_[x]-1,z);
            }
            if(ch=='4'){
                x=read();
                printf("%d\n",tree_sum(1,1,n,idx[x],idx[x]+size_[x]-1));
            }
        }
    }
    ```