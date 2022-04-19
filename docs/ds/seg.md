# 线段树

(通用模板线段树实现)[https://blog.therehello.top/Segment_Tree/]

???+ note "参考代码"

    ```cpp
    struct Num{
        ll add;
        inline static Num zero(){return {0};}
        inline Num operator+(Num b){return {add+b.add};}
    };
    struct Data{
        ll sum,len;
        Num lazytag;
        inline static Data zero(){return {0,0,Num::zero()};}
        inline Data operator+(Num b){return {sum+len*b.add,len,lazytag+b};}
        inline Data operator+(Data b){return {sum+b.sum,len+b.len,Num::zero()};}
    };
    ```

???+ note "参考代码"

    ```cpp
    template <class Data,class Num>
    struct Segment_Tree{
        inline void update(int l,int r,Num x){update(1,l,r,x);}
        inline Data query(int l,int r){return query(1,l,r);}
        Segment_Tree(vector<Data>& a){
            n=a.size();
            tree.assign(n*4+1,{});
            build(a,1,0,n-1);
        }
        int n;
        struct Tree{int l,r;Data data;};
        vector<Tree> tree;
        inline void pushup(int pos){
            tree[pos].data=tree[pos<<1].data+tree[pos<<1|1].data;
        }
        inline void pushdown(int pos){
            tree[pos<<1].data=tree[pos<<1].data+tree[pos].data.lazytag;
            tree[pos<<1|1].data=tree[pos<<1|1].data+tree[pos].data.lazytag;
            tree[pos].data.lazytag=Num::zero();
        }
        void build(vector<Data>& a,int pos,int l,int r){
            tree[pos].l=l;tree[pos].r=r;
            if(l==r){tree[pos].data=a[l];return;}
            int mid=(tree[pos].l+tree[pos].r)>>1;
            build(a,pos<<1,l,mid);
            build(a,pos<<1|1,mid+1,r);
            pushup(pos);
        }
        void update(int pos,int& l,int& r,Num& x){
            if(l>tree[pos].r||r<tree[pos].l)return;
            if(l<=tree[pos].l&&tree[pos].r<=r){tree[pos].data=tree[pos].data+x;return;}
            pushdown(pos);
            update(pos<<1,l,r,x);update(pos<<1|1,l,r,x);
            pushup(pos);
        }
        Data query(int pos,int& l,int& r){
            if(l>tree[pos].r||r<tree[pos].l)return Data::zero();
            if(l<=tree[pos].l&&tree[pos].r<=r)return tree[pos].data;
            pushdown(pos);
            return query(pos<<1,l,r)+query(pos<<1|1,l,r);
        }
    };
    ```
