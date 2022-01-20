# 逆序对

???+ note "参考代码"

    ```cpp
    #include<bits/stdc++.h>
    using namespace std;
    int n,tree[555555];
    struct dd{
        int v,pos;
    }a[555555];
    long long ans=0;
    #define lowbit(x) ((x)&(-x))
    inline bool cmp(dd x, dd y){
        return x.v==y.v?x.pos>y.pos:x.v>y.v;
    }
    void update(int t,int x){
        while(t<=n){
            tree[t]+=x;
            t+=lowbit(t);
        }
    }
    int sum(int t){
        int tot=0;
        while(t){
            tot+=tree[t];
            t-=lowbit(t);
        }
        return tot;
    }
    inline int read(){
        int x=0,f=1;
        char ch=getchar();
        while(ch>'9'||ch<'0'){
            if(ch=='-')f=-1;
            ch=getchar();
        }
        while(ch>='0'&&ch<='9'){
            x=x*10+(ch^48);
            ch=getchar();
        }
        return x*f;
    }
    int main(){
        n=read();
        for(int i=1;i<=n;++i){
            a[i].v=read();
            a[i].pos=i;
        }
        sort(a+1,a+1+n,cmp);
        for(int i=1;i<=n;++i){
            ans+=sum(a[i].pos);
            update(a[i].pos,1);
        }
        printf("%lld",ans);
    }
    ```
