1. 단지 번호 붙이기(JUNG 1695)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
 
int N;
char map[30][30];
int cnt;
int index;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int ans[30];
 
int cmp(const void *a, const void *b)
{
    return *(int *)a - *(int *)b;
}
 
void InputData(void)
{
    register int r, c;
    scanf("%d", &N);
    for (r = 1; r <= N; r++) {
        scanf("%s", &map[r][1]);
    }
}
 
void DFS(int r, int c)
{
    register int i;
    if (map[r][c] != '1') return;
    map[r][c] = 0;
    cnt++;
    for (i = 0; i < 4; i++) DFS(r + dr[i], c + dc[i]);
}
 
void solve(void)
{
    register int r, c;
    for (r = 1; r <= N; r++) {
        for (c = 1; c <= N; c++) {
            if (map[r][c] == '1') {
                cnt = 0;
                DFS(r, c);
                ans[index++] = cnt;
            }
        }
    }
    qsort(ans, index, sizeof(ans[0]), cmp);
}
 
void prt(void)
{
    register int i;
    printf("%d\n", index);
    for (i = 0; i < index; i++) printf("%d\n", ans[i]);
}
 
int main(void)
{
    InputData();
    solve();
    prt();
    return 0;
}
```



2. 영역 구하기(JUNG 1457)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int M, N, K;
int map[110][110];
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int ans[110*110];
int index, cnt;
 
void Initial(void)
{
    memset(map, 0, sizeof(map));
    memset(ans, 0, sizeof(ans));
    index = cnt = 0;
}
 
void InputData(void)
{
    register int r, c, k;
    int sc, sr, ec, er;
    scanf("%d %d %d", &M, &N, &K);
    for (k = 0; k < K; k++) {
        scanf("%d %d %d %d", &sc, &sr, &ec, &er);
        for (r = sr; r < er; r++) {
            for (c = sc; c < ec; c++) {
                map[r][c] = 1;
            }
        }
    }
}
 
void DFS(int r, int c)
{
    register int i;
    if ((r < 0) || (r >= M) || (c < 0) || (c >= N)) return;
    if (map[r][c]) return;
    map[r][c] = 1;
    cnt++;
    for (i = 0; i < 4; i++) DFS(r + dr[i], c + dc[i]);
}
 
int cmp(const void *a, const void *b)
{
    return *(int *)a - *(int *)b;
}
 
void solve(void)
{
    register int r, c;
    for (r = 0; r < M; r++) {
        for (c = 0; c < N; c++) {
            if (map[r][c] == 0) {
                cnt = 0;
                DFS(r, c);
                ans[index++] = cnt;
            }
        }
    }
    qsort(ans, index, sizeof(ans[0]), cmp);
}
 
void prt(void)
{
    register int i;
    printf("%d\n", index);
    for (i = 0; i < index; i++) printf("%d ", ans[i]);
    printf("\n");
}
 
int main(void)
{
    int TC, i;
    scanf("%d", &TC);
    for (i = 0; i < TC; i++) {
        Initial();
        InputData();
        solve();
        prt();
    }
    return 0;
}
```



3. 암소 미인 대회 (Bronze) - USACO 2011 Novermber Contest Bronze

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
typedef struct _Q{
    int r, c, d;
}Queue;
 
typedef struct _candi{
    int r, c;
}Candidat;
 
Queue Q[60 * 60];
Candidat cand[50 * 50];
char map[60][60];
int R, C;
int ci;
int rp, wp;
int visited[60][60];
int index = 1;
int cnt[3];
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int counter[] = { 0, 2, 1 };
int empty(void) { return rp == wp; }
Queue pop(void) { return Q[rp++]; }
int push(int r, int c, int d)
{
    if (visited[r][c]) return -1;
    if (d == 0) {
        Q[wp].r = r; Q[wp].c = c; Q[wp].d = d; wp++; return -1;
    }
    if (map[r][c] == counter[index]) return d;
    if (map[r][c] != '.') return -1;
    visited[r][c] = 1;
    Q[wp].r = r; Q[wp].c = c; Q[wp].d = d; wp++;
    return -1;
}
 
void DFS(int r, int c)
{
    register int i;
    if (map[r][c] != 'X') return;
    map[r][c] = index;
    cnt[index]++;
    for (i = 0; i < 4; i++) DFS(r + dr[i], c + dc[i]);
}
 
void Initial(void)
{
    memset(visited, 0, sizeof(visited));
    rp = wp = 0;
}
 
void InputData(void)
{
    register int r, c;
    int min = 60 * 60;
    scanf("%d %d", &R, &C);
    for (r = 1; r <= R; r++) {
        scanf("%s", &map[r][1]);
    }
 
    for (r = 1; r <= R; r++) {
        for (c = 1; c <= C; c++) {
            if (map[r][c] == 'X') {
                DFS(r, c);
                index++;
            }
        }
    }
 
    if (cnt[1] < cnt[2]) index = 1;
    else index = 2;
 
    for (r = 1; r <= R; r++) {
        for (c = 1; c <= C; c++) {
            if (map[r][c] == index) {
                cand[ci++] = (Candidat) { r, c };
            }
        }
    }
}
 
int BFS(int sr, int sc)
{
    register int i;
    int ans = 50*50;
    push(sr, sc, 0);
    while (!empty()) {
        Queue p = pop();
        for (i = 0; i < 4; i++) {
            ans = push(p.r + dr[i], p.c + dc[i], p.d + 1);
            if (ans >= 0) return ans;
        }
    }
    return 50*50;
}
 
int main(void)
{
    int i, ans, min = 50*50;
    InputData();
    for (i = 0; i < ci; i++) {
        Initial();
        ans = BFS(cand[i].r, cand[i].c);
        if (ans < min) min = ans;
    }
    printf("%d", min-1);
    return 0;
}
```



4. 치즈(JUNG 1840)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
typedef struct _rc{
    int r, c, t;
}RC;
RC Q[110 * 110];
int rp, wp;
int map[110][110];
int used[110][110];
int checked[110][110];
int time[110];
int R, C;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
 
int empty(void) { return rp == wp; }
int push(int r, int c, int t)
{
    if ((r<1) || (r>R) || (c<1) || (c>C)) return 0;
    if (used[r][c]) return 0;
    if (map[r][c] != 1) return 0;
    map[r][c] = 0;
    Q[wp].r = r; Q[wp].c = c; Q[wp].t = t; wp++;
    time[t]++;
    return 1;
}
RC pop(void) { return Q[rp++]; }
 
void Initial(void)
{
    memset(Q, 0, sizeof(Q));
    memset(map, 0, sizeof(map));
    memset(checked, 0, sizeof(checked));
    memset(time, 0, sizeof(time));
    memset(used, 0, sizeof(used));
    rp = wp = 0;
}
 
void DFS(int r, int c, int t)
{
    register int i;
    if ((r<1) || (r>R) || (c<1) || (c>C)) return 0;
    if (map[r][c] == 1) return;
    if (checked[r][c]) return;
    checked[r][c] = 1;
    Q[wp].r = r; Q[wp].c = c; Q[wp].t = t; wp++;
    for (i = 0; i < 4; i++) DFS(r + dr[i], c + dc[i], t);
}
 
RC getRC(int tmp_R, int tmp_C)
{
    register int r, c, i;
    int oper = 1;
    RC rc;
    tmp_R = R;
    tmp_C = C;
    r = 1; c = 0;
    for (; tmp_C >= 0;) {
        for (i = 0; i < tmp_C; i++) {
            c += oper;
            if (map[r][c] == 0) {
                rc.r = r;
                rc.c = c;
                return rc;
            }
        }
        tmp_R--;
        for (i = 0; i < tmp_R; i++) {
            r += oper;
            if (map[r][c] == 0) {
                rc.r = r;
                rc.c = c;
                return rc;
            }
        }
        oper *= -1;
        tmp_C--;
    }
}
 
void InputData(void)
{
    register int r, c;
    scanf("%d %d", &R, &C);
    for (r = 1; r <= R; r++) {
        for (c = 1; c <= C; c++) {
            scanf("%d", &map[r][c]);
        }
    }
}
 
void solve(void)
{
    RC rc;
    register int i, j, t = 0;
    int nr, nc, ok, tmp_w;
    rc = getRC(R, C);
    nr = rc.r; nc = rc.c;
    DFS(nr, nc, t);
    for (;;) {
        tmp_w = wp;
        for (i = rp; i < tmp_w; i++) {
            RC p = pop();
            used[p.r][p.c] = 1;
            for (j = 0; j < 4; j++) {
                ok = push(p.r + dr[j], p.c + dc[j], p.t+1);
                if (ok == 1) {
                    nr = p.r + dr[j];
                    nc = p.c + dc[j];
                    DFS(nr, nc, p.t+1);
                }
            }
        }
        t++;
        if (empty()) {
            printf("%d\n", t-1);
            printf("%d\n", time[t - 1]);
            break;
        }
    }
}
 
int main(void)
{
    int TC, i;
    scanf("%d", &TC);
    for (i = 0; i < TC; i++) {
        Initial();
        InputData();
        solve();
    }
    return 0;
}
```



5. 암소 미인 대회(Silver) - USACO 2011 November Contest Silver

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
#define MINV(a, b) ((a) < (b) ? (a) : (b))
#define ABS(a) (((a) < 0) ? -(a) : (a))
#define DIST(a, b) (ABS((a).r - (b).r) + ABS((a).c - (b).c) - 1)
#define INF (60*60)
 
typedef struct rc{
    int r, c;
}RC;
RC dots[4][50 * 50];
 
int R, C;
char cow[60][60];
int index = 1;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int cnt;
 
void FloodFill(int r, int c, int id)
{
    register int i;
    if (cow[r][c] != 'X') return;
    cow[r][c] = id;
    dots[id][cnt].r = r; dots[id][cnt].c = c; cnt++;
    for (i = 0; i < 4; i++) FloodFill(r + dr[i], c + dc[i], id);
}
 
void InputData(void)
{
    register int r, c;
    scanf("%d %d", &R, &C);
    for (r = 1; r <= R; r++) {
        scanf("%s", &cow[r][1]);
    }
    for (r = 1; r <= R; r++) {
        for (c = 1; c <= C; c++) {
            if (cow[r][c] == 'X') {
                cnt = 0;
                FloodFill(r, c, index++);
            }
        }
    }
}
 
int connect1(void)
{
    register int s, i, j;
    int line, min[4] = { INF, INF, INF, INF }, sum = 0;
    for (s = 1; s <= 3; s++) {
        int e = s % 3 + 1;
        for (i = 0; dots[s][i].r; i++) {
            for (j = 0; dots[e][j].r; j++) {
                line = DIST(dots[s][i], dots[e][j]); 
                if (line < min[s]) min[s] = line;
            }
        }
        sum += min[s];
    }
 
    if ((min[1] > min[2]) && (min[1] > min[3])) sum -= min[1];
    else if (min[2] > min[3]) sum -= min[2];
    else sum -= min[3];
    return sum;
}
 
int calc_dist(int r, int c)
{
    register int i, line, min, sum = 0, s;
    RC rc = { r, c };
    for (s = 1; s <= 3; s++) {
        min = INF;
        for (i = 0; dots[s][i].r; i++) {
            line = DIST(rc, dots[s][i]);
            if (line < min) min = line;
        }
        sum += min;
    }
    return sum + 1;
}
 
int connect2(void)
{
    register int r, c, min = INF, line;
    for (r = 1; r <= R; r++) {
        for (c = 1; c <= C; c++) {
            if (cow[r][c] == '.') {
                line = calc_dist(r, c);
                if (line < min) min = line;
            }
        }
    }
    return min;
}
 
int main(void)
{
    int ans1, ans2;
    InputData();
    ans1 = connect1();
    ans2 = connect2();
    printf("%d", MINV(ans1, ans2));
    return 0;
}
```

