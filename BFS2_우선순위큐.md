1. 최소 비용으로 포장 다시 하기

```C
#include <stdio.h>
#define MAXN ((int)(5e3+100))
#define SWAP(a,b) {int tmp; tmp = Heap[(a)]; Heap[(a)] = Heap[(b)]; Heap[(b)] = tmp;}
int N;
int Heap[MAXN];
int last;
 
void push(int num)
{
    register int P, C;
    Heap[++last] = num;
    C = last; P = C / 2;
    while (P && (Heap[P] > Heap[C])) {
        SWAP(P, C);
        C = P; P = C / 2;
    }
}
 
int pop()
{
    register int P = 1, L = 2, R = 3, C;
    int ret = Heap[1]; Heap[P] = Heap[last--];
    while (L <= last) {
        if (R > last) C = L;
        else C = (Heap[L] < Heap[R]) ? L : R;
        if (Heap[P] < Heap[C]) break;
        SWAP(P, C);
        P = C; L = P * 2; R = L + 1;
    }
    return ret;
}
 
void InputData(void)
{
    register int i; int ni;
    scanf("%d", &N);
    for (i = 0; i < N; i++) {
        scanf("%d", &ni);
        push(ni);
    }
}
 
int solve(void)
{
    register int i, tmp = 0, ans = 0;
    for (i = 1; i < N; i++) {
        tmp = pop() + pop();
        ans += tmp;
        push(tmp);
    }
    return ans;
}
 
 
int main(void)
{
    InputData();
    printf("%d", solve());
    return 0;
}
```



2. 못생긴 수2 

```C
#include <stdio.h>
#include <string.h>
#define SWAP(a, b) {long long tmp = heap[(a)]; heap[(a)] = heap[(b)]; heap[(b)] = tmp;}
long long heap[1501*3] = { 0, 1 ,2 ,3 ,4 };
long long out[1501];
int last = 4;
int SP;
 
void Initial(void)
{
    memset(out, 0, sizeof(out));
    heap[0] = 0; heap[1] = 1; heap[2] = 2; heap[3] = 3; heap[4] = 4;
    SP = 0;
    last = 4;
}
 
int BFS(long long d)
{
    int m, s = 0, e = SP - 1;
    while (s <= e) {
        m = (s + e) / 2;
        if (out[m] == d) return 1;
        if (out[m] < d) s = m + 1;
        else e = m - 1;
    }
    return 0;
}
 
int already(long long d)
{
    if (BFS(d)) return 1;
    out[SP++] = d;
    return 0;
}
 
void push(long long d)
{
    register int P, C;
    heap[++last] = d;
    for (C = last, P = C / 2; P && (heap[P] > heap[C]); C = P, P = C / 2) {
        SWAP(P, C);
    }
}
 
long long pop(void)
{
    register int C, P = 1, L = 2, R = 3;
    long long ret = heap[1];
    heap[1] = heap[last--];
    while (L <= last) {
        if (last < R) C = L;
        else C = (heap[L] < heap[R]) ? L : R;
        if (heap[P] <= heap[C]) break;
        SWAP(P, C);
        P = C; L = P * 2; R = L + 1;
    }
    return ret;
}
 
long long Find(int n)
{
    long long i, ret = pop();
    for (i = 1; i < n; i++) {
        push(ret * 2); push(ret * 3); push(ret * 5);
        ret = pop();
        while (already(ret)) {
            ret = pop();
        }
    }
    return ret;
}
 
int main(void)
{
    int N;
    for (;;) {
        Initial();
        scanf("%d", &N);
        if (N == 0) break;
        printf("%lld\n", Find(N));
    }
     
    return 0;
}
```



3. 자외선을 피해 가기

```C
#include <stdio.h>
#define MAX (1000)
 
typedef struct _Q{
    int r, c, ray;
}Queue;
Queue Q[101*101*101*4];
int rp, wp;
int N;
int ray[101][101];
int visit[101][101];
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int min = MAX;
 
int empty(void) { return rp == wp; }
Queue pop(void) { return Q[rp++]; }
void Initial(void)
{
    register int r, c;
    for (r = 1; r <= N; r++) {
        for (c = 1; c <= N; c++) {
            visit[r][c] = MAX;
        }
    }
}
 
int push(int r, int c, int cumul_ray)
{
    if ((r < 1) || (N < r) || (c < 1) || (N < c)) return -1;
    cumul_ray += ray[r][c];
    if (visit[r][c] <= cumul_ray) return -1;
    if (r == N && c == N) return cumul_ray;
    visit[r][c] = cumul_ray;
    Q[wp].r = r; Q[wp].c = c; Q[wp].ray = cumul_ray; wp++;
    return -1;
}
 
void Input(void)
{
    register int r, c;
    scanf("%d", &N);
    for (r = 1; r <= N; r++) {
        for (c = 1; c <= N; c++) {
            scanf("%1d", &ray[r][c]);
        }
    }
}
 
int BFS(void)
{
    register int i, ans = -2;
    push(1, 1, 0);
    while (!empty()) {
        Queue p = pop();
        for (i = 0; i < 4; i++) {
            ans = push(p.r + dr[i], p.c + dc[i], p.ray);
            if (ans >= 0) {
                if (ans < min) min = ans;
            }
        }
    }
    return min;
}
 
int main(void)
{
    Input();
    Initial();
    printf("%d", BFS());
    return 0;
}


#include <stdio.h>
#define INF (2000)
#define SWAP(a, b) {Qu tmp = Q[(a)]; Q[(a)] = Q[(b)]; Q[(b)] = tmp;}
typedef struct _Q{
    int r, c, t;
}Qu;
 
Qu Q[101 * 101];
int map[101][101];
int visit[101][101];
int N;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int lastn;
 
int empty(void) { return lastn == 0; }
Qu pop(void) 
{   //minimum heap!
    register int C, P = 1, L = 2, R = 3;
    Qu ret = Q[1];
    Q[1] = Q[lastn--];
    while (L <= lastn) {
        if (R > lastn) C = L;
        else C = (Q[L].t < Q[R].t) ? L : R;
        if (Q[P].t < Q[C].t) break;
        SWAP(P, C);
        P = C; L = P * 2; R = L + 1;
    }
    return ret;
}
 
void push(int r, int c, int t)
{
    int P, C;
    if ((r < 1) || (N < r) || (c < 1) || (N < c)) return;
    if (visit[r][c] <= t) return;
    visit[r][c] = t;
    Q[++lastn].r = r; Q[lastn].c = c; Q[lastn].t = t;
    for (C = lastn, P = C / 2; P && (Q[P].t > Q[C].t); C = P, P = C / 2) {
        SWAP(P, C);
    }
}
 
void Input(void)
{
    register int r, c;
    scanf("%d", &N);
    for (r = 1; r <= N; r++) {
        for (c = 1; c <= N; c++) {
            scanf("%1d", &map[r][c]);
        }
    }
}
 
int BFS(void)
{
    register int i;
    push(1, 1, 0);
    while (!empty()) {
        Qu p = pop();
        if (p.r == N && p.c == N) return p.t;
        for (i = 0; i < 4; i++) {
            int nr = p.r + dr[i], nc = p.c + dc[i];
            push(nr, nc, p.t + map[nr][nc]);
        }
    }
    return -2;
}
 
void Initial(void)
{
    register r, c;
    for (r = 1; r <= N; r++) {
        for (c = 1; c <= N; c++) {
            visit[r][c] = INF;
        }
    }
}
 
int main(void)
{
    Input();
    Initial();
    printf("%d", BFS());
    return 0;
}
```



4. 귀가( JUNG 1208)

```C
#include <stdio.h>
#define MAX (50000)
int map[58][58];
int N;
int cap[58];
int ci;
int cost[58];
int sel[58];
 
 
void set(void)
{
    register int i; char c;
    for (c = 'A'; c <= 'Y'; c++) cap[c-'A'] = 1;
    for (i = 0; i < 58; i++) cost[i] = MAX;
}
 
void InputData(void)
{
    register int i;
    char s, e; int d;
    scanf("%d", &N);
    for (i = 0; i < N; i++) {
        scanf(" %c %c %d", &s, &e, &d); //Z를 제외한 대문자는 소가 있음. 
        s = s - 'A'; //A:0 ~ Y:24 , a: 32 ~ y: 57, Z: 25;
        e = e - 'A';
        if (!map[s][e]) {
            map[s][e] = d; map[e][s] = d;
        }
        else if (d < map[s][e]) {
            map[s][e] = d; map[e][s] = d;
        }
    }
}
 
void calc_cost(char c)
{
    register int i, cumul_c;
    for (i = 0; i < 58; i++) {
        if (!map[c][i]) continue; //연결되지 않았거나
        if (sel[i]) continue;     //이미 연결된 곳이면 패스
        cumul_c = cost[c] + map[c][i]; //누적 비용 + 새로 가는 비용
        if (cumul_c < cost[i]) { //새로 계산한 비용이 기존 비용보다 싸다면
            cost[i] = cumul_c;
        }
    }
}
 
char conn(void)
{
    register int i, min = MAX, min_i = -1;
    for (i = 0; i < 58; i++) {
        if (sel[i]) continue;
        if (min > cost[i]) {
            min = cost[i];
            min_i = i;
        }
    }
    return min_i;
}
 
void Dijkstra(void)
{
    //Z에서 출발해서 제일 먼저 도착하는 대문자!
    char s = 'Z' - 'A';
    sel[s] = 1, cost[s] = 0;
    while (cap[s] != 1) { //s 가 대문자이면 취소!
        calc_cost(s);
        s = conn();
        sel[s] = 1;
    }
    printf("%c %d", s+'A', cost[s]);
}
 
int main(void)
{
    set();
    InputData();
    Dijkstra();
    return 0;
}
```



5. 등산로 찾기( JUNG 1111)

```C
#include <stdio.h>
#define SWAP(a, b) { Qu tmp = heap[(a)]; heap[(a)] = heap[(b)]; heap[(b)] = tmp; }
#define MAX (500000)
typedef struct _Q{
    int r, c, t;
}Qu;
 
Qu heap[105 * 105 * 10];
int lastn;
int map[105][105];
int visit[105][105];
int N, er, ec;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
 
int empty(void) { return lastn == 0; }
Qu pop(void)  //위에서부터!
{
    register int C, P = 1, L = 2, R = 3;
    Qu ret = heap[1];
    heap[1] = heap[lastn--];
    while (L <= lastn) { //부모랑 비교 가능함
        if (lastn < R) C = L; //R 존재 x
        else C = (heap[L].t < heap[R].t) ? L : R; //작은 애 주세요!
        if (heap[P].t < heap[C].t) break; //내가 내 아들보다 작나요? 그럼 괜춘하죠
        SWAP(P, C); //그렇지 않으면 내가 아들 해야죠
        P = C; L = P * 2; R = L + 1; 
    }
    return ret;
}
 
void push(int r, int c, int diff, int cost)
{
    register int P, C;
    if (map[r][c] == 0) return;
    if (diff > 0) diff *= diff;
    else if (diff < 0) diff = -diff;
    cost += diff;
    if (visit[r][c] <= cost) return;
    visit[r][c] = cost;
    heap[++lastn].r = r; heap[lastn].c = c; heap[lastn].t = cost;
    for (C = lastn, P = C / 2; P && (heap[P].t > heap[C].t); C = P, P = C / 2) {
        SWAP(P, C);
    }
}
 
void InputData(void)
{
    register int r, c;
    scanf("%d", &N);
    scanf("%d %d", &er, &ec);
    for (r = 1; r <= N; r++) {
        for (c = 1; c <= N; c++) {
            scanf("%d", &map[r][c]);
            if ((r == 1) || (r==N)) push(r, c, map[r][c], 0);  //둘레에 있는 애들을 다 힙에 넣는다.
            else if ((c == 1) || (c == N)) push(r, c, map[r][c], 0);
        }
    }
}
 
void Set(void)
{
    register int r, c;
    for (r = 0; r < 105; r++) {
        for (c = 0; c < 105; c++) {
            visit[r][c] = MAX;
        }
    }
}
 
int BFS(void)
{
    register int i;
    while (!empty()) {
        Qu p = pop();
        if (p.r == er && p.c == ec) return p.t;
        for (i = 0; i < 4; i++) {
            int nr = p.r + dr[i], nc = p.c + dc[i];
            push(nr, nc, map[nr][nc] - map[p.r][p.c], p.t); //you - me;
        }
    }
    return -1;
}
 
int main(void)
{
    Set();
    InputData();
    printf("%d", BFS());
    return 0;
}
```



6. 먼 목초지 -  USACO 2012 November Contest Silver

```C
#include <stdio.h>
#include <string.h>
#define SWAP(A, B) {Qu tmp = heap[(A)]; heap[(A)] = heap[(B)]; heap[(B)] = tmp;}
#define INF ((int)(900e6))
typedef struct _Q{
    int r, c, t;
}Qu;
 
Qu heap[40 * 40 * 100];
int lastn;
char map[40][40];
int visit[40][40];
int N, A, B;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int cnt;
 
int empty(void) { return lastn == 0; }
Qu pop(void) 
{   //위에서부터!!
    register int P = 1, L = 2, R = 3, C; 
    Qu ret = heap[1];
    heap[1] = heap[lastn--];
    while (L <= lastn) {
        if (lastn < R) C = L;
        else C = (heap[L].t < heap[R].t) ? L : R;
        if (heap[P].t < heap[C].t) break; //아빠가 나보다 작아 괜춘
        SWAP(P, C);
        P = C; L = P * 2; R = L + 1;
    }
    return ret;
}
 
int push(int br, int bc, int nr, int nc, int cost)
{   //아래서부터!
    int P, C;
    if (map[nr][nc] == 0) return -1; //바깥이면 ㅌㅌ
    if (map[nr][nc] == map[br][bc]) cost += A; //같으면 A더하고 다르면 B더함
    else cost += B;
    if (visit[nr][nc] <= cost) return -1; //더 큰 비용이 들면 안 가죠~
    visit[nr][nc] = cost;
    heap[++lastn].r = nr; heap[lastn].c = nc; heap[lastn].t = cost;
    for (C = lastn, P = C / 2; P && (heap[P].t > heap[C].t); C = P, P = C / 2) {
        SWAP(P, C);
    }
    return cost;
}
 
void Set(void)
{
    register int r, c;
    memset(heap, 0, sizeof(heap));
    for (r = 0; r <= N; r++) {
        for (c = 0; c <= N; c++) {
            visit[r][c] = INF;
        }
    }
    lastn = 0; cnt = N*N;
}
 
void InputData(void)
{
    register int r;
    scanf("%d %d %d", &N, &A, &B); //같으면 비용 A, 다르면 비용 B
    for (r = 1; r <= N; r++) {
        scanf("%s", &map[r][1]);
    }
}
 
int BFS(int r, int c)
{
    register int i, j, max=0;
    int ans = -2;
    push(r, c, r, c, -A);
    while (!empty()) {
        Qu p = pop();
        for (i = 0; i < 4; i++) {
            int nr = p.r + dr[i], nc = p.c + dc[i];
            push(p.r, p.c, nr, nc, p.t);
        }
    }
    for (i = 1; i <= N; i++) {
        for (j = 1; j <= N; j++) {
            if (max < visit[i][j]) max = visit[i][j];
        }
    }
    return max;
}
 
int main(void)
{
    register int r, c;
    int ans, max=0;
    InputData();
    for (r = 1; r <= N; r++) {
        for (c = 1; c <= N; c++) {
            Set();
            ans = BFS(r, c);
            if (max < ans) max = ans;
        }
    }
    printf("%lld", max);
    return 0;
}
```



7. 자동차 경주대회( JUNG 1491)

```C
#include <stdio.h>
#define INF ((int)(1e8))
#define SWAP(A, B) {Qu tmp = heap[(A)]; heap[(A)] = heap[(B)]; heap[(B)] = tmp;}
typedef struct _Q{
    int pos, t, cnt, path[110];
}Qu;
 
 
Qu heap[110 * 10];
int lastn;
int N, T;
int shop[110];
int wait[110];
int visit[110];
 
int empty(void) { return lastn == 0; }
Qu pop(void)
{   //위에서부터
    int C, P = 1, L = 2, R = 3;
    Qu ret = heap[1];
    heap[1] = heap[lastn--];
    while (L <= lastn) {
        if (lastn < R) C = L;
        else C = (heap[L].t < heap[R].t) ? L : R;
        if (heap[P].t < heap[C].t) break;
        SWAP(P, C);
        P = C; L = 2 * P; R = L + 1;
    }
    return ret;
}
 
void push(int pos, int time, int cnt, int path[110])
{
    register int P, C, i;
    if (visit[pos] <= time) return;
    visit[pos] = time;
    heap[++lastn].pos = pos; heap[lastn].t = time; heap[lastn].cnt = cnt;
    for (i = 0; i < cnt; i++) heap[lastn].path[i] = path[i];
    heap[lastn].path[cnt] = pos;
    for (C = lastn, P = C / 2; P && (heap[P].t > heap[C].t); C = P, P = C / 2) {
        SWAP(P, C);
    }
}
 
void InputData(void)
{
    register int i;
    scanf("%d", &T);
    scanf("%d", &N);
    for (i = 1; i <= N+1; i++) {
        scanf("%d", &shop[i]);
    }
    for (i = 1; i <= N; i++) {
        scanf("%d", &wait[i]);
    }
    wait[N + 1] = 0;
}
 
Qu BFS(void)
{
    register int i, len;
    Qu tmp = { 0, 0, 0 };
    push(0, 0, 0, 0);
    while (!empty()) {
        Qu p = pop();
        if (p.pos == N + 1) return p;
        len = shop[p.pos + 1];
        for (i = 1; (len <= T) && (p.pos+i <= N+1); i++) {
            push(p.pos + i, p.t + wait[p.pos + i], p.cnt + 1, p.path);
            len += shop[p.pos + i + 1];
        }
    }
    return tmp;
}
void Reset(void)
{
    register int i;
    for (i = 0; i < 110; i++) visit[i] = INF;
}
 
int main(void)
{
    register int i;
    Qu ans;
    InputData();
    Reset();
    ans = BFS(); //우선순위 큐로!
    printf("%d\n", ans.t);
    if (ans.cnt - 1 != 0) {
        printf("%d\n", ans.cnt-1);
        for (i = 1; i <= ans.cnt-1; i++) {
            printf("%d ", ans.path[i]);
        }
    }
    return 0;
}
```

