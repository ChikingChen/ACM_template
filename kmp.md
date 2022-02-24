```c++
void pre(){
    int j=0,k=-1;
    int len2=strlen(s2);
    while(j<len2) {  //预处理
        if (k == -1 || s2[j] == s2[k]) {//k==-1已经跳到头//j和k位置上的字符相等，两者匹配
            next[++j] = ++k;
        }else {  //j和k位置上的字符不等失配，向前跳重新匹配
            k = next[k];
        }
    }
}

void kmp(){
    int k=0,j=0;
    while(j<len1){   //模式串和文本串的匹配
        if(k==-1||s1[j]==s2[k]) {//匹配失败跳过当前字符//模式串和文本串对应位置匹配成功
            j++; k++;
        } else {    //失配后向前跳
            k = next[k];
        }
        if(k==len2){   //匹配成功
            k=next[k];
        }
    }
}
```

