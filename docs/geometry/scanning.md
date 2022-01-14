# 扫描线

???+ note "参考代码"

    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    int n, tail, tot;
    long long ans;
    int yy[200005], val[200005];
    struct pp
    {
        int len;
        int tag;
    } c[800005];
    struct dd
    {
        int x, y_, y__, k;
        int l, r;
        inline bool operator<(const dd y) const
        {
            return x < y.x;
        }
    } a[200005];
    inline int read()
    {
        register int x = 0, f = 1;
        register char ch = getchar();
        while (ch > '9' || ch < '0')
        {
            if (ch == '-')
                f = -1;
            ch = getchar();
        }
        while (ch >= '0' && ch <= '9')
        {
            x = x * 10 + (ch ^ 48);
            ch = getchar();
        }
        return x * f;
    }
    void modify(int pos, int l, int r, int ll, int rr, int k)
    {
        if (ll <= l && r <= rr)
        {
            c[pos].tag += k;
            c[pos].len = c[pos].tag ? val[r + 1] - val[l] : l == r ? 0 : c[pos << 1].len + c[pos << 1 | 1].len;
            return;
        }
        int mid = l + r >> 1, ls = pos << 1, rs = ls | 1;
        if (ll <= mid)
            modify(ls, l, mid, ll, rr, k);
        if (mid < rr)
            modify(rs, mid + 1, r, ll, rr, k);
        c[pos].len = c[pos].tag ? c[pos].len : c[ls].len + c[rs].len;
    }
    int main()
    {
        scanf("%d", &n);
        for (int i = 1; i <= n; ++i)
        {
            ++tail;
            a[tail].x = read();
            yy[tail] = read();
            ++tail;
            a[tail].x = read();
            yy[tail] = read();
            if (yy[tail - 1] > yy[tail])
                swap(yy[tail - 1], yy[tail]);
            a[tail].y_ = a[tail - 1].y_ = yy[tail - 1];
            a[tail].y__ = a[tail - 1].y__ = yy[tail];
            a[tail].k = -1;
            a[tail - 1].k = 1;
        }
        sort(a + 1, a + 1 + tail);
        sort(yy + 1, yy + 1 + tail);
        tot = unique(yy + 1, yy + 1 + tail) - (yy);
        for (int i = 1; i <= tail; ++i)
        {
            int l = lower_bound(yy + 1, yy + tot, a[i].y_) - (yy),
                r = lower_bound(yy + 1, yy + tot, a[i].y__) - (yy);
            val[l] = a[i].y_;
            val[r] = a[i].y__;
            a[i].l = l;
            a[i].r = r;
        }
        --tail;
        for (int i = 1; i <= tail; ++i)
        {
            modify(1, 1, tail, a[i].l, a[i].r - 1, a[i].k);
            ans += (long long)(a[i + 1].x - a[i].x) * c[1].len;
        }
        printf("%lld", ans);
    }
    ```
