# 最近公共祖先

## 平常在信息学竞赛中求LCA一般有四种办法

- 倍增法求解，预处理复杂度是 $O(nlog⁡n)$ ,每次询问的复杂度是 $O(log⁡n)$。

- 利用欧拉序转化为RMQ问题，用ST表求解RMQ问题，预处理复杂度$O(n+nlogn)$，每次询问的复杂度为 $O(1)$。

- 采用Tarjan算法求解，复杂度 $O(α(n)+Q)$。

- 利用树链剖分求解，复杂度预处理$O(n)$，单次查询 $O(log⁡n)$。

下面将详细地介绍“用欧拉序转换为RMQ问题”。

## 用欧拉序转换为RMQ问题

温馨提示，若想更好地阅读本文章，你需要掌握一下知识：

- 可以dfs一棵树
- 熟知st表

![树](https://s2.ax1x.com/2019/09/28/u123j0.png)

| 数组下标 |1  |  2|3  |  4|5  |6  | 7 | 8 |9  |10  | 11 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|**欧拉序列**| 1 | 2 |  4|  2|5  | 2| 1 | 3 | 6 |  3|1|
|**深度**| 1 | 2 | 3 | 2 | 3 | 2 |  1|  2| 3 | 2 |  1|

对有根树进行dfs，无论是递归还是回溯，每次到达一个结点时都将深度记录下来，这个东西叫做**欧拉序列**，其长度为$2n-1$，不难证明。

把节点$k$在欧拉序列中第一次出现的序号记为$fir(k)$。
有了欧拉序列，**$lca$问题可以在线性时间内化为RMQ问题** ，即
$lca(u,v)=RMQ(fir(u),fir(v))$，这里RMQ返回的是深度最小的值所对应的节点。

若需求两个点的距离，利用 $dep[u]+dep[v]-(2*dep[lca(u,v)]$ 可以求得。

仔细想一下，从$u$走到$v$的过程中一定会经过$lca(u,v)$，但不会经过$lca(u,v)$的祖先。因此，从$u$走到$v$的过程中经过的深度最小的结点就是$lca(u,v)$。

用$dfs$计算欧拉序列的时间复杂度$O(n)$，空间复杂度$O(n)$，所以$lca$问题可以在$O(n)$的时间内用$O(n)$的空间转化成等规模的$RMQ$问题。

>此处引用刘汝佳《算法竞赛入门经典》并做适当修改。

$dfs$计算欧拉序列：

!!! note ""

    ``` cpp
    void dfs(int x,int fa,int depth){
        st[++dfn_tail][0]=x;//直接用st[i][0]储存欧拉序列了
        dep[x]=depth;
        fir[x]=dfn_tail;
        for(int i=head[x];i;i=edge[i].ne){
            if(edge[i].to!=fa){
                dfs(edge[i].to,x,depth+1);
                st[++dfn_tail][0]=x;
            }
        }
    }
    ```

现在已经知道深度了，先预处理一下st：

!!! note ""

    ```cpp
    void ready(){
        lg[1]=0;//由于cmath里log函数处理比较慢，这里进行预处理一下
        for(int i=2;i<=dfn_tail;i++)lg[i]=lg[i>>1]+1;
        for(int j=1;j<=lg[dfn_tail];j++)
            for(int i=1;i<=dfn_tail-(1<<j)+1;i++)
                st[i][j]=dep[st[i][j-1]]<dep[st[i+(1<<(j-1))][j-1]]
                ?st[i][j-1]
                :st[i+(1<<(j-1))][j-1];
        //比较深度返回下标
    }
    ```

整体思路就是这样

???+ note "参考代码"

    ``` cpp
    #include<bits/stdc++.h>
    using namespace std;
    #define add(x,y) edge[++tail]=(dd){head[x],y},head[x]=tail;
    #define N 505050
    #define N2 1111111
    struct dd{
        int ne,to;
    }edge[N2];
    int head[N],tail,dfn_tail,fir[N],dep[N];
    int n,m,s;
    int st[N2][21],lg[N2];
    inline int read(){
        char c=getchar();int res=0;
        while(c>'9'||c<'0'){c=getchar();}
        while (c<='9'&&c>='0'){res*=10;res+=c-48;c=getchar();}
        return res;
    }
    void dfs(int x,int fa,int depth){
        st[++dfn_tail][0]=x;
        dep[x]=depth;
        fir[x]=dfn_tail;
        for(int i=head[x];i;i=edge[i].ne){
            if(edge[i].to!=fa){
                dfs(edge[i].to,x,depth+1);
                st[++dfn_tail][0]=x;
            }
        }
    }
    void ready(){
        lg[1]=0;
        for(int i=2;i<=dfn_tail;i++)lg[i]=lg[i>>1]+1;
        for(int j=1;j<=lg[dfn_tail];j++)
            for(int i=1;i<=dfn_tail-(1<<j)+1;i++)
                st[i][j]=dep[st[i][j-1]]<dep[st[i+(1<<(j-1))][j-1]]
                ?st[i][j-1]
                :st[i+(1<<(j-1))][j-1];
    }
    inline int query_lca(int x,int y){
        int l=fir[x],r=fir[y];
        if(l>r)swap(l,r);
        int k=lg[r-l+1];
        return dep[st[l][k]]<dep[st[r-(1<<k)+1][k]]
               ?st[l][k]:st[r-(1<<k)+1][k];
    }
    int main(){
        n=read();m=read();s=read();
        for(int i=1,x,y;i<n;i++){
            x=read();y=read();
            add(x,y);
            add(y,x);
        }
        
        dfs(s,s,1);
        ready();

        for(int i=1,x,y;i<=m;i++){
            x=read();y=read();
            printf("%d\n",query_lca(x,y));
        }
        return 0;
    }
    ```
