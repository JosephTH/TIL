1. 최소 비용 트리

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define MAX ((int)(1e5+10))
int N;
int v[110][110];
int cost[110];
int conn[110];
int Min_cost;
 
void InputData(void)
{
    int i, j;
    scanf("%d", &N);
    for (i = 0; i < N; i++) {
        for (j = 0; j <= i; j++) {
            scanf("%d", &v[i][j]);
            v[j][i] = v[i][j];
        }
    }
    for (i = 0; i < 110; i++) {
        cost[i] = MAX;
    }
}
 
void calc_cost(int k)
{
    int conn_node, conn_cost;
    for (conn_node = 0; conn_node < N; conn_node++) {
        conn_cost = v[k][conn_node];
        if (conn_cost == 0) continue;
        if (conn_cost < cost[conn_node]) cost[conn_node] = conn_cost;
    }
}
 
int connect()
{
    int i, connect_i = -1, min = MAX;
    for (i = 0; i < N; i++) {
        if (conn[i]) continue;
        if (cost[i] < min) { //연결이 안된 노드 중, 비용이 제일 작은 노드를 찾는다.
            min = cost[i];
            connect_i = i;
        }
    }
    Min_cost += min;
    return connect_i;
}
 
void solve(void)
{
    int i, k = 0;
    cost[0] = 0; conn[0] = 1;
    for (i = 0; i < N - 1; i++) {
        calc_cost(k);
        k = connect();
        if (k == -1) return 0;
        conn[k] = 1;
    }
    printf("%d", Min_cost);
}
 
int main(void)
{
    InputData();
    solve();
    return 0;
}
```



2. 최소비용신장트리(JUNG 1060)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define MAX ((int)(1e5+10))
int N;
int v[110][110];
int cost[110];
int conn[110];
int Min_cost;
 
void InputData(void)
{
    int i, j;
    scanf("%d", &N);
    for (i = 0; i < N; i++) {
        for (j = 0; j <N; j++) {
            scanf("%d", &v[i][j]);
        }
    }
    for (i = 0; i < 110; i++) {
        cost[i] = MAX;
    }
}
 
void calc_cost(int k)
{
    int conn_node, conn_cost;
    for (conn_node = 0; conn_node < N; conn_node++) {
        conn_cost = v[k][conn_node];
        if ((conn_cost == 0) || conn[conn_node]) continue;
        if (conn_cost < cost[conn_node]) cost[conn_node] = conn_cost;
    }
}
 
int connect()
{
    int i, connect_i = -1, min = MAX;
    for (i = 0; i < N; i++) {
        if (conn[i]) continue;
        if (cost[i] < min) { //연결이 안된 노드 중, 비용이 제일 작은 노드를 찾는다.
            min = cost[i];
            connect_i = i;
        }
    }
    Min_cost += min;
    return connect_i;
}
 
void solve(void)
{
    int i, k = 0;
    cost[0] = 0; conn[0] = 1;
    for (i = 0; i < N - 1; i++) {
        calc_cost(k);
        k = connect();
        if (k == -1) return 0;
        conn[k] = 1;
    }
    printf("%d", Min_cost);
}
 
int main(void)
{
    InputData();
    solve();
    return 0;
}
```



3. 최대 신장 트리(JUNG 1350)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#define MAX ((int)(1e5+10))
#define N_conn 20010
#define N_node 1010
typedef struct _span {
    int from;
    int to;
    int weight;
}SP;
int N, M;
SP span[N_conn*2];
int weight[N_node];
int conn[N_node];
int Max_weight;
int index[N_node];
 
int cmp(const void *a, const void *b)
{
    return ((SP *)a)->from - ((SP *)b)->from;
}
 
void get_index(void)
{
    int i, from = span[0].from;
    index[from] = 0;
    for (i = 1; i < M*2; i++) {
        if (from == span[i].from) continue;
        from = span[i].from;
        index[from] = i;
    }
}
 
void InputData(void)
{
    int i, j;
    scanf("%d %d", &N, &M);
    for (i = 0; i < M; i++) {
            scanf("%d %d %d", &span[i].from, &span[i].to, &span[i].weight);
            span[M + i].from = span[i].to;
            span[M + i].to = span[i].from;
            span[M + i].weight = span[i].weight;
    }
    qsort(span, M*2, sizeof(span[0]), cmp);
    get_index();
}
 
void calc_weight(int from_n)
{
    int i, to_n, w;
    for (i = index[from_n]; from_n == span[i].from; i++) {
        to_n = span[i].to; w = span[i].weight;
        if (conn[to_n]) continue;
        if (weight[to_n] < w) weight[to_n] = w;
    }
}
 
int connect(void)
{
    int i, max=0, max_i=-1;
    for (i = 1; i <= N; i++) {
        if (conn[i]) continue;
        if (max < weight[i]) {
            max = weight[i];
            max_i = i;
        }
    }
    Max_weight += max;
    return max_i;
}
 
void solve(void)
{
    int i, k = 1;
    weight[1] = 0; conn[1] = 1;
    for (i = 0; i < N - 1; i++) {
        calc_weight(k);
        k = connect();
        conn[k] = 1;
    }
    printf("%d", Max_weight);
}
 
int main(void)
{
    InputData();
    solve();
    return 0;
}
```



4. 최단경로찾기

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAXN 15
#define MAX 1100
int N;
int city[MAXN][MAXN];
int cost[MAXN];
int conn[MAXN];
int path[MAXN];
 
void InputData(void)
{
    int i, j;
    scanf("%d", &N);
    for (i = 1; i <= N; i++) {
        for (j = 1; j <= N; j++) {
            scanf("%d", &city[i][j]);
            if (city[i][j] == 0) city[i][j] = MAX;
        }
    }
    for (i = 1; i <= N; i++) {
        cost[i] = MAX;
    }
}
 
void calc_cost(int k)
{
    int i, cumul_cost;
    for (i = 1; i <= N; i++) {
        if (conn[i]) continue;
        cumul_cost = cost[k] + city[k][i];
        if (cost[i] > cumul_cost) {
            cost[i] = cumul_cost;
            path[i] = k;
        }
    }
}
 
int connect(void)
{
    int i, min_i=-1, min=MAX;
    for (i = 1; i <= N; i++) {
        if (conn[i]) continue;
        if (cost[i] < min) {
            min = cost[i];
            min_i = i;
        }
    }
    return min_i;
}
 
int solve(void)
{
    int i, k = 1;
    cost[1] = 0, conn[1] = 1;
    for (i = 0; i < N - 1; i++) {
        calc_cost(k);
        k = connect();
        if (k == -1) break;
        conn[k] = 1;
        if (k == N) return cost[k];
    }
    return 0;
}
 
 
 
int main(void)
{
    InputData();
    printf("%d", solve());
    return 0;
}
```



5. 최소 비용 구하기

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAXB ((int)(1e5+10)*2)
#define MAXC (1100)
#define MAX (110*1000)
typedef struct _city{
    int e, c;
    struct _city *next;
}C;
 
int N, M;
C c_node[MAXB];
C c_table[MAXC];
int cost[MAXC];
int conn[MAXC];
int path[MAXC];
int nptr;
int S, E;
 
C * myalloc(int e, int c, C *next)
{
    C *p = &c_node[nptr++];
    p->next = next;
    p->e = e;
    p->c = c;
    return p;
}
 
void InputData(void)
{
    register int i, j;
    int s, e, c;
    scanf("%d", &N);
    scanf("%d", &M);
    for (i = 1; i <= M; i++) {
        scanf("%d %d %d", &s, &e, &c);
        c_table[s].next = myalloc(e, c, c_table[s].next);
    }
    scanf("%d %d", &S, &E);
    for (i = 1; i <= N; i++) {
        cost[i] = MAX;
    }
    //for (i = 1; i <= N; i++) {
    //  if (c_table[i].next) {
    //      C *p = c_table[i].next;
    //      while (p) {
    //          printf("%d -> %d\n", i, p->e);
    //          p = p->next;
    //      }
    //  }
    //}
}
 
void calc_cost(int k)
{
    int cumul_cost;
    C *dest = c_table[k].next;
    while (dest) {
        if (conn[dest->e]) {
            dest = dest->next;
            continue;
        }
        cumul_cost = cost[k] + dest->c;
        if (cost[dest->e] > cumul_cost) {
            cost[dest->e] = cumul_cost;
            path[dest->e] = k;
        }
        dest = dest->next;
    }
}
 
int connect(void)
{
    int i, min_i=-1, min=MAX;
    for (i = 1; i <= N; i++) {
        if (conn[i]) continue;
        if (cost[i] < min) {
            min = cost[i];
            min_i = i;
        }
    }
    return min_i;
}
 
int solve(void)
{
    int i, k = S;
    cost[S] = 0, conn[S] = 1;
    for (i = 0; i < N - 1; i++) {
        calc_cost(k);
        k = connect();
        if (k == -1) return 0;
        if (k == E) return cost[E];
        conn[k] = 1;
    }
}
 
 
int main(void)
{
    InputData();
    printf("%d", solve());
    return 0;
}
```



6. 지하철(JUNG 2097)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAXN (110)
#define MAX 1100
int N, E;
int city[MAXN][MAXN];
int cost[MAXN];
int conn[MAXN];
int path[MAXN];
 
void InputData(void)
{
    int i, j;
    scanf("%d", &N);
    scanf("%d", &E);
    for (i = 1; i <= N; i++) {
        for (j = 1; j <= N; j++) {
            scanf("%d", &city[i][j]);
            if (city[i][j] == 0) city[i][j] = MAX;
        }
    }
    for (i = 1; i <= N; i++) {
        cost[i] = MAX;
    }
}
 
void calc_cost(int k)
{
    int i, cumul_cost;
    for (i = 1; i <= N; i++) {
        if (conn[i]) continue;
        cumul_cost = cost[k] + city[k][i];
        if (cost[i] > cumul_cost) {
            cost[i] = cumul_cost;
            path[i] = k;
        }
    }
}
 
int connect(void)
{
    int i, min_i = -1, min = MAX;
    for (i = 1; i <= N; i++) {
        if (conn[i]) continue;
        if (cost[i] < min) {
            min = cost[i];
            min_i = i;
        }
    }
    return min_i;
}
 
int solve(void)
{
    int i, k = 1;
    cost[1] = 0, conn[1] = 1;
    for (i = 0; i < N - 1; i++) {
        calc_cost(k);
        k = connect();
        if (k == -1) break;
        conn[k] = 1;
        if (k == E) return cost[E];
    }
    return 0;
}
 
void prt(int e)
{
    if (e == 0) return;
    prt(path[e]);
    printf("%d ", e);
}
 
int main(void)
{
    InputData();
    printf("%d\n", solve());
    prt(E);
    return 0;
}
```



7. 암소 여행 경로(Silver) - USACO 2015 January Contest Silver

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define MAXC (1010)
#define MAXR (1010*1010)
#define MAX ((long long)(1e9+10)*(long long)(1e9+10))
 
int N, A, B;
int city[MAXC][MAXC][2];
long long cost[MAXC];
int path[MAXC];
int conn[MAXC];
int route[110];
 
void InputData(void)
{
    register int i, j, k;
    int C, n_city;
    scanf("%d %d %d", &A, &B, &N);
    for (i = 0; i < N; i++) {
        scanf("%d %d", &C, &n_city);
        for (j = 0; j < n_city; j++) {
            scanf("%d", &route[j]);
        }
        for (j = 0; j < n_city-1; j++) {
            for (k = j + 1; k < n_city; k++) {
                if (!city[route[j]][route[k]][0]) {
                    city[route[j]][route[k]][0] = C;
                    city[route[j]][route[k]][1] = k - j;
                }
                else if (C < city[route[j]][route[k]][0]) {
                    city[route[j]][route[k]][0] = C;
                    city[route[j]][route[k]][1] = k - j;
                }
                else if ((C == city[route[j]][route[k]][0]) && (city[route[j]][route[k]][1] == k - j)) {
                    city[route[j]][route[k]][1] = k-j;
                }
            }
        }
    }
    for (i = 0; i < MAXC; i++) {
        cost[i] = MAX;
        path[i] = 10000;
    }
}
 
void calc_cost(int k)
{
    register int i, cumul_path;
    long long cumul_cost;
    for (i = 0; i < MAXC; i++) {
        if (!city[k][i][0]) continue;
        if (conn[i]) continue;
        cumul_cost = cost[k] + city[k][i][0]; //i까지 드는 누적 비용
        cumul_path = path[k] + city[k][i][1]; //i까지 필요한 경로 수
        if ((cost[i] == cumul_cost) && (path[i] > cumul_path)) { //비용이 동일하고 경로가 적으면
            path[i] = cumul_path;
        }
        else if (cost[i] > cumul_cost) { // 그냥 비용이 적으면
            cost[i] = cumul_cost;
            path[i] = cumul_path;
        }
    }
}
 
int connect(void)
{
    register int i, min_i = -1;
    long long min = MAX;
    for (i = 0; i < MAXC; i++) {
        if (conn[i]) continue;
        if (cost[i] < min) {
            min = cost[i];
            min_i = i;
        }
    }
    return min_i;
}
 
void Dijkstra(void)
{
    register int i, k = A;
    conn[k] = 1, cost[k] = 0, path[k] = 0;
    for (i = 0; i < MAXC; i++) {
        calc_cost(k);
        k = connect();
        if (k == B) {
            printf("%lld %d", cost[B], path[B]);
            return;
        }
        if (k == -1) break;
        conn[k] = 1;
    }
    printf("%d %d", -1, -1);
}
 
 
int main(void)
{
    InputData();
    Dijkstra();
    return 0;
}
```

