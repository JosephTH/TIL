### 아이디어성&최적화

1. 배열 구간의 합 구하기

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
int N, Q;
int data[50010];
void InputData(void)
{
    int i, num;
    scanf("%d %d", &N, &Q);
    for (i = 1; i <= N; i++) {
        scanf("%d", &num);
        data[i] = num + data[i - 1];
    }
}
void solve(void)
{
    int i, s, e;
    for (i = 1; i <= Q; i++) {
        scanf("%d %d", &s, &e);
        printf("%d\n", data[e] - data[s-1]);
    }
}
int main(void)
{
    InputData();
    solve();
    return 0;
}
```

2. 냉장고(JUNG 1828)

> **N**개의 화학 물질 C1, C2, …, Cn이 있다. 이들 각각은 보관되어야 할 온도가 각기 다른데, 각 Ci마다 최저보관온도 xi와 최고보관온도 yi가 정해져 있다. 즉 Ci는 온도 xi이상, yi이하의 온도에서 보관되어야만 안전하다.
>
> **이 화학 물질들을 모두 보관하기 위해서는 여러 대의 냉장고가 필요한데 가능하면 적은 수의 냉장고를 사용하고 싶다. 이를 해결하는 프로그램을 작성하시오.**

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
typedef struct fridge{
    int s;
    int e;
}F;
F *fridges;
int N;
int cmp(const void *a, const void *b)
{
    return ((F *)a)->s - ((F *)b)->s;
}
void InputData(void)
{
    register int i;
    scanf("%d", &N);
    fridges = calloc(N, sizeof(F));
    for (i = 0; i < N; i++) {
        scanf("%d %d", &fridges[i].s, &fridges[i].e);
    }
    qsort(fridges, N, sizeof(F), cmp);
}
void solve(void)
{
    int i, cnt=0, maxmin = fridges[0].e;
    for (i = 1; i < N; i++) {
        if (maxmin < fridges[i].s) {
            cnt++;
            maxmin = fridges[i].e;
        }
        if (maxmin > fridges[i].e) maxmin = fridges[i].e;
    }
    printf("%d", ++cnt);
}
int main(void)
{
    InputData();
    solve();
    return 0;
}
```

3. 용액(KOI 본선 2010 초1)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#define MAXN (int)(1e5+10)
int N;
int arr[MAXN];
 
void InputData(void)
{
    int i;
    scanf("%d", &N);
    for (i = 0; i < N; i++) {
        scanf("%d", &arr[i]);
    }
}
 
void solve(void)
{
    int s, e, min, end = N-1, res=200000000, minS=0, minE=end;
    for (s = 0; s < end; s++) {
        min = abs(arr[s] + arr[end]);
        for (e = end-1; e > s; e--) {
            if (min < abs(arr[s] + arr[e]))  break;
            min = abs(arr[s] + arr[e]);
            end--;
        }
        if (min < res) {
            res = min;
            minS = s;
            minE = e + 1;
        };
    }
    printf("%d %d", arr[minS], arr[minE]);
}
 
int main(void)
{
    InputData();
    solve();
    return 0;
}
```

4. 회전 초밥(고) 

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#define MAXN (int)(3e6+10)
 
int N, d, k, c;
int types[3010];
int sushi[MAXN];
 
void solve(void)
{
    register int i, cnt = 1, max;
    scanf("%d %d %d %d", &N, &d, &k, &c);
    types[c] = 1;
    for (i = 0; i < k; i++) {
        scanf("%d", &sushi[i]);
        if (++types[sushi[i]] == 1) cnt++;
    }
    max = cnt;
    for (i = k; i < N; i++) {
        scanf("%d", &sushi[i]);
        if (--types[sushi[i - k]] == 0) cnt--;
        if (++types[sushi[i]] == 1) cnt++;
        if (max < cnt) max = cnt;
    }
    for (i = 0; i < k; i++) {
        if (--types[sushi[N-k+i]] == 0) cnt--;
        if (++types[sushi[i]] == 1) cnt++;
        if (max < cnt) max = cnt;
    }
    printf("%d", max);
}
 
int main(void)
{
    solve();
    return 0;
}
```



5. 택배(JUNG 2641)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#define MIN(a, b) ((a)>(b)) ? (b): (a)
int N, C, pc;
int parc[2010][2010];
int town[2010]; //0. 남아있는 것  1. 보내는 것
 
void InputData(void)
{
    int parcel, i, src, dest, s, e, amount, max = 0, min;
    scanf("%d %d", &N, &C);
    scanf("%d", &pc);
    for (i = 1; i <= pc; i++) {
        scanf("%d %d %d", &src, &dest, &amount);
        parc[src][dest] += amount;
    }
     
    for (dest = 2; dest <= N; dest++) {
        for (src = dest-1; src >= 1; src--) {
            if (!parc[src][dest]) continue;
            min = parc[src][dest];
            for (i = src; i < dest; i++) {
                if (C - town[i] < min) min = C - town[i];
            }
            for (i = src; i < dest; i++) {
                town[i] += min;
            }
            max += min;
        }
    }
    printf("%d", max);
}
 
int main(void)
{
    InputData();
    return 0;
}
```



6. 공주님의 정원(JUNG 2461)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#define MAXN ((int)1e5+10)
#define Endday (12*100+1)
typedef struct flower{
    int st;
    int et;
}F;
F arr[MAXN];
int N;
int index;
 
void InputData(void)
{
    int i, sm, sd, em, ed;
    scanf("%d", &N);
    for (i = 0; i < N; i++) {
        scanf("%d %d %d %d", &sm, &sd, &em, &ed);
        arr[i].st = sm * 100 + sd;
        arr[i].et = em * 100 + ed;
    }
}
 
int cmp(const void *a, const void *b)
{
    return ((F*)a)->st - ((F*)b)->st;
}
 
int latestF(int date)
{
    int i, max = arr[index].et;
    for (i = index+1; i<N; i++) {
        if (arr[i].st > date) break;
        if (max < arr[i].et) max = arr[i].et;
        index = i;
    }
    return max;
}
 
int solve(void)
{
    int date, cnt = 1;
    qsort(arr, N, sizeof(F), cmp);
    date = latestF(3 * 100 + 1);
    while (date) {
        if (date >= Endday) break;
        if (index >= N - 1) return 0;
        date = latestF(date);
        cnt++;
    }
    return cnt;
}
 
int main(void)
{
    InputData();
    printf("%d", solve());
    return 0;
}
```

