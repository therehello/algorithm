# 悬线法

???+ note "参考代码"

    ```cpp
    #include<bits/stdc++.h>
    using namespace std;
    int n,m,ans;
    int a[1005][1005];
    int l[1005][1005],r[1005][1005],h[1005][1005];
    inline char read(){
        char ch=getchar();
        while(ch!='R'&&ch!='F')ch=getchar();
        return ch;
    }
    int main(){
        cin>>n>>m;
        char ch;
        for(int i=1;i<=n;++i){
            for(int j=1;j<=m;++j){
                ch=read();
                if(ch=='F')a[i][j]=1;
            }
        }
        for(int i=1;i<=n;++i)
            for(int j=2;j<=m;++j)
                if(a[i][j]==a[i][j-1]&&a[i][j])
                    l[i][j]=l[i][j-1]+1;
        for(int i=1;i<=n;++i)
            for(int j=m-1;j;--j)
                if(a[i][j]==a[i][j+1]&&a[i][j])
                    r[i][j]=r[i][j+1]+1;
        for(int i=1;i<=n;++i){
            for(int j=1;j<=m;++j){
                if(a[i][j]&&a[i][j]==a[i-1][j]){
                    l[i][j]=min(l[i][j],l[i-1][j]);
                    r[i][j]=min(r[i][j],r[i-1][j]);
                    h[i][j]=h[i-1][j]+1;
                }
                ans=max(ans,(l[i][j]+r[i][j]+1)*(h[i][j]+1));
            }
        }
        cout<<ans*3;
    }
    ```
