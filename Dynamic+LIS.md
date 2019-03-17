1. 피보나치수열( JUNG 1047)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
 
int N;
int fibo[45] = { 0, 1, 1 };
 
int memo(int n)
{
    if (fibo[n] == 0) {
        fibo[n] = memo(n-1) + memo(n-2);
    }
    return fibo[n];
}
 
int Dynamic(int n)
{
    register int i;
    for (i = 3; i <= n; i++) {
        fibo[i] = fibo[i - 1] + fibo[i - 2];
    }
    return fibo[n];
}
 
int main(void)
{
    scanf("%d", &N);
    printf("%d\n", memo(N));
    //printf("%d\n", Dynamic(N));
    return 0;
}
```



2. 동전교환( JUNG 2000)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define INF ((int)(1e5))
#define MINV(a, b) (((a) < (b)) ? (a) : (b))
int N, W;
int coin[11];
int dp[64001];
 
void Initial(void)
{
    register int i;
    dp[0] = 0;
    for (i = 1; i <= W; i++) dp[i] = INF;
}
 
void InputData(void)
{
    register int i;
    scanf("%d", &N);
    for (i = 1; i <= N; i++) {
        scanf("%d", &coin[i]);
    }
    scanf("%d", &W);
}
 
int DD(int N, int W)
{
    register int i, w;
    for (i = 1; i <= N; i++) {
        for (w = 1; w <= W; w++) {
            int ans1 = dp[w];
            int ans2 = INF;
            if (w >= coin[i]) ans2 = dp[w - coin[i]] + 1;
            dp[w] = MINV(ans1, ans2);
        }
    }
    if (dp[W] == INF) printf("impossible\n");
    else printf("%d\n", dp[W]);
}
 
 
int main(void)
{
    InputData();
    Initial();
    DD(N, W);
    return 0;
}
```



3. 배낭채우기( JUNG 1077)

```C
#include <stdio.h>
#define MAX(a, b) (((a) < (b)) ? (b) : (a))
typedef struct _gem{
    int w, v;
}G;
G gem[1001];
int dp[10001];
 
int N, W;
 
void InputData(void)
{
    register int i;
    scanf("%d %d", &N, &W);
    for (i = 1; i <= N; i++) {
        scanf("%d %d", &gem[i].w, &gem[i].v);
    }
}
 
void Initial(void)
{
    register int i;
    for (i = 0; i <= W; i++) dp[i] = 0;
}
 
int Dynamic(int N, int W)
{
    register int i, w;
    for (i = 1; i <= N; i++) {
        for (w = 1; w <= W; w++) {
            int ans1 = dp[w];
            int max_n = w / gem[i].w;
            int ans2 = max_n * gem[i].v + dp[w - max_n*gem[i].w];
            dp[w] = MAX(ans1, ans2);
        }
    }
    return dp[W];
}
 
int main(void)
{
    InputData();
    Initial();
    printf("%d", Dynamic(N, W));
    return 0;
}
```



4. 줄 세우기( JUNG 1871)

```C
#include <stdio.h>
 
int N;
int arr[201];
int index_arr[201];
 
void InputData(void)
{
    register int i;
    scanf("%d", &N);
    for (i = 1; i <= N; i++) {
        scanf("%d", &arr[i]);
    }
}
 
 
int solve(void)
{
    register int i, j, max = 0;
    for (i = 1; i <= N; i++) {
        for (j = 0; j < i; j++) {
            if (arr[j] < arr[i]) {
                if(index_arr[i] < index_arr[j] + 1) index_arr[i] = index_arr[j]+1;
            }
        }
        if (max < index_arr[i]) max = index_arr[i];
    }
    return N - max;
}
 
int main(void)
{
    InputData();
    printf("%d", solve());
    return 0;
}
```



5. 전깃줄(초) - KOI 본선 2007 초4

```C
#include <stdio.h>
#include <stdlib.h>
int N;
int arr[501][2];
int seq[501];
 
int cmp(const void *a, const void *b)
{
    return ((int *)a)[0] - ((int *)b)[0];
}
 
void InputData(void)
{
    register int i;
    int s, e;
    scanf("%d", &N);
    for (i = 1; i <= N; i++) {
        scanf("%d %d", &arr[i][0], &arr[i][1]);
    }
    qsort(arr[1], N, sizeof(arr[0]), cmp);
}
 
 
int solve(void)
{
    register int i, j, max = 0;
    for (i = 1; i <= N; i++) {
        for (j = 0; j < i; j++) {
            if ((arr[j][0] < arr[i][0]) && (arr[j][1] < arr[i][1])) {
                if (seq[i] < seq[j] + 1) seq[i] = seq[j] + 1;
            }
        }
        if (max < seq[i]) max = seq[i];
    }
    return N - max;
}
 
int main(void)
{
    InputData();
    printf("%d", solve());
    return 0;
}
```



6. 색종이 올려놓기( JUNG 1510

```C
#include <stdio.h>
#include <stdlib.h>
int N;
int arr[101][2];
int n_arr[101];
 
int cmp(const void *a, const void *b)
{
    if (((int *)a)[0] == ((int *)b)[0]) return ((int *)a)[1] - ((int *)b)[1];
    return ((int *)a)[0] - ((int *)b)[0];
}
 
void InputData(void)
{
    register int i, tmp;
    scanf("%d", &N);
    for (i = 1; i <= N; i++) {
        scanf("%d %d", &arr[i][0], &arr[i][1]);
        if (arr[i][0] < arr[i][1]) {
            tmp = arr[i][0]; arr[i][0] = arr[i][1]; arr[i][1] = tmp;
        }
    }
    qsort(arr[1], N, sizeof(arr[0]), cmp);
}
 
int Dynamic(void)
{
    register int i, j, max=0;
    for (i = 1; i <= N; i++) {
        for (j = 0; j < i; j++) {
            if ((arr[j][0] <= arr[i][0]) && (arr[j][1] <= arr[i][1])) {
                if (n_arr[i] < n_arr[j]+1) n_arr[i] = n_arr[j]+1;
            }
        }
        if (max < n_arr[i]) max = n_arr[i];
    }
    return max;
}
 
int main(void)
{
    InputData();
    printf("%d", Dynamic());
    return 0;
}
```



7. 조깅(Gold) - USACO 2014 December Gold 3

```C
#include <stdio.h>
#define MAXN ((int)(1e5+10))
long long N, T;
long long pos[MAXN];
long long stack[MAXN];
int SP;
 
void InputData(void)
{
    register int i;
    long long start, speed;
    scanf("%lld %lld", &N, &T);
    for (i = 1; i <= N; i++) {
        scanf("%lld %lld", &start, &speed);
        pos[i] = start + speed * T;
    }
}
 
int find(long long num)
{
    register int m, s = 0, e = SP-1, sol = -1;
    if (SP == 0) return sol;
    while (s <= e) { //나보다 작은 애 중 제일 큰 애 찾는다. 못 찾으면? 스택에 넣는다!
        m = (s + e) / 2;
        if (stack[m] < num) {
            sol = m;
            e = m - 1;
        }
        else s = m + 1;
    }
    return sol;
}
 
int solve(void)
{
    register int i, j;
    for (i = 1; i <= N; i++) {
        j = find(pos[i]); //들어가야 할 자리 찾음
        if (j == -1) stack[SP++] = pos[i];
        else stack[j] = pos[i];
    }
    return SP;
}
 
int main(void)
{
    InputData();
    printf("%d", solve());
    return 0;
}
```

