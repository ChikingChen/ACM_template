```c++
//区间数字最大值
void st(){//区间数字最大值

    for(int j=1;j<=log(n)/log(2);j++){
        for(int i=1;i<=n-bit[j]+1;i++){
            f[i][j]=max(f[i][j-1],f[i+bit[j-1]][j-1]);
        }
    }

    for(int i=1;i<=m;i++){
        int l,r;
        scanf("%d%d",&l,&r);
        int k=log(r-l+1)/log(2);
        printf("%d\n",max(f[l][k],f[r-bit[k]+1][k]));
    }

}
```

