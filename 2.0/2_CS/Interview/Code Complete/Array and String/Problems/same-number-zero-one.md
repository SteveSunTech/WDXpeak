# 01 相等的串

> 给定一个 01 串，求它一个最长的子串满足 0 和 1 的个数相等

分析

+ 把 0 看成 -1，1 当做 +1，使用前缀和
+ 需要两个前缀和相等，则这两个前缀和之间的子串满足 0 的个数和 1 的个数相等。
+ 对前缀和排序？O(nlogn)
+ 优化——不需要排序
    + 前缀和范围是[-n..n]，我们加上 n 之后就是[0..2n]，只要记录每个和第一次出现的位置
    + 本质是利用 hash 代替排序
    + 当 hash 值是比较小的非负整数时，可以用作数组下标

```
int run(char *s){
    int n = strlen(s);
    // 开一个长度为 2n + 1 的数组 (n << 1) | 1 ，并设为 -1
    vector<int> have((n << 1) | 1, -1);
    // 因为是从 n 开始，
    have[n] = 0;
    int sum = n;
    int best = 0;
    for (int i = 0; i < n; i++){
        sum += (s[i] == '0') ? (-1):(+1);
        if (have[sum] >= 0){
            best = max(best, i - have[sum] + 1);
        } else {
            have[sum] = i + 1;
        }
    }
}
```

方法比较 tricky，需要注意细节。

