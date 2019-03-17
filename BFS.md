1. 너비 우선 탐색(KOISTUDY Prob No. 00AB)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
#define MAXN (210)
 
typedef struct _span{
    int s, e;
}Sp;
 
Sp span[MAXN];
int Q[MAXN];
int visit[110];
int rp, wp;
int empty() { return rp == wp; }
int N, S;
void push(int e) 
{
    if (visit[e]) return;
    Q[wp++] = e;
    visit[e] = 1;
    printf("%d ", e);
}
int pop(void) 
{
    return Q[rp++];
}
void Initial(void) 
{
    memset(Q, 0, MAXN*sizeof(Q[0]));
    memset(visit, 0, 110);
    rp = wp = 0;
}
void InputData(void)
{
    register int i;
    scanf("%d %d", &N, &S);
    for (i = 0; i < S; i++) {
        scanf("%d %d", &span[i].s, &span[i].e);
    }
}
void solve(void)
{
    int i, v;
    push(1);
    while (!empty()) {
        int p = pop();
        for (i = 0; i < S; i++) {
            if (span[i].s == p) push(span[i].e); 
            else if (span[i].e == p) push(span[i].s);
            else continue;
        }
    }
}
int main(void)
{
    Initial();
    InputData();
    solve();
    return 0;
}
```



2. 미로탈출로봇(JUNG 1661)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
typedef struct _Q{
    int r, c;
}Q;
Q Queue[110 * 110];
int rp, wp;
int R, C;
int sr, sc, er, ec;
char map[110][110];
int time[110][110];
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int empty(void) { return wp == rp; }
Q pop(void){ return Queue[rp++]; }
int push(int r, int c, int t) 
{
    if (map[r][c] != '0') return -1;
    if (time[r][c]) return -1;
    if ((r == er) && (c == ec)) return t;
    Queue[wp].r = r; Queue[wp].c = c; wp++;
    time[r][c] = t + 1;
    return -1;
}
void Initial(void)
{
    memset(map, 0, 110 * 110);
    memset(time, 0, 110 * 110);
    rp = wp = 0;
}
void InputData(void)
{
    int i;
    scanf("%d %d", &C, &R);
    scanf("%d %d %d %d", &sc, &sr, &ec, &er);
    for (i = 1; i <= R; i++) {
        scanf("%s", &map[i][1]); //map[r][c]
    }
}
 
int solve(void)
{
    int i, ans = -1;
    push(sr, sc, 0);
    while (!empty()) {
        Q p = pop();
        for (i = 0; i < 4; i++) {
            ans = push(p.r + dr[i], p.c + dc[i], time[p.r][p.c]);
            if (ans >= 0) return ans;
        }
    }
    return ans;
}
 
int main(void)
{
    Initial();
    InputData();
    printf("%d", solve());
    return 0;
}
```



3. 저글링 방사능 오염(JUNG 1078)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
 
typedef struct _Q{
    int r, c, t;
}Q;
 
Q Queue[110 * 110];
int C, R;
int sc, sr;
char map[110][110];
int rp, wp;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int last_contam;
int contaminated[110][110];
 
int empty(void) { return rp == wp; }
 
Q pop() { return Queue[rp++]; }
 
void contam(int r, int c, int t)
{
    if (map[r][c] != '1') return;
    if (contaminated[r][c]) return;
    contaminated[r][c] = 1;
    Queue[wp].r = r; Queue[wp].c = c; Queue[wp].t = t; wp++;
    map[r][c] = 0;
    if (last_contam < t) last_contam = t;
}
 
void Initial(void)
{
    memset(map, 0, 110 * 110);
    memset(contaminated, 0, 110 * 110);
    rp = wp = 0;
    last_contam = 0;
}
 
void InputData(void)
{
    register int i;
    scanf("%d %d", &C, &R);
    for (i = 1; i <= R; i++) {
        scanf("%s", &map[i][1]);
    }
    scanf("%d %d", &sc, &sr);
}
 
int cnt_not_contam(void)
{
    int i, j, cnt = 0;
    for (i = 1; i <= C; i++) {
        for (j = 1; j <= R; j++) {
            if (map[j][i] == '1') cnt++;
        }
    }
    return cnt;
}
 
void BFS(void)
{
    register int i;
    contam(sr, sc, 0);
    while (!empty()) {
        Q p = pop();
        for (i = 0; i < 4; i++) {
            contam(p.r + dr[i], p.c + dc[i], p.t+1);
        }
    }
    printf("%d\n", last_contam+3);
    printf("%d\n", cnt_not_contam());
}
int main(void)
{
    int i, TC;
    scanf("%d", &TC);
    for (i = 0; i < TC; i++) {
        Initial();
        InputData();
        BFS();
    }
}
```



4. 토마토(초) - KOI 본선 2013 초3

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
typedef struct _Q{
    int r, c, h, day;
}Queue;
 
Queue Q[110 * 110 * 110];
int H, R, C;
int box[110][110][110];
int dr[] = {0, 0, -1, 1, 0, 0};
int dc[] = {0, 0, 0, 0, -1, 1};
int dh[] = {-1, 1, 0, 0, 0, 0};
int wp, rp;
int not_ripen;
int lastday;
 
int empty(void) { return wp == rp; }
Queue pop(void) { return Q[rp++]; }
void push(int h, int r, int c, int d)
{
    if ((r < 1) || (R < r) || (c < 1) || (C < c) || (h < 1) || (H < h)) return;
    if ((box[h][r][c] == 2) || (box[h][r][c] == -1))return;
    if (box[h][r][c] == 0) not_ripen--;
    box[h][r][c] = 2;
    Q[wp].r = r; Q[wp].c = c; Q[wp].h = h; Q[wp].day = d; wp++;
    if (lastday < d) lastday = d;
}
void Initial(void)
{
    memset(box, 0, 110 * 110 * 110);
    memset(Q, 0, 110 * 110 * 110 * sizeof(Q[0]));
    not_ripen = 0;
    wp = rp = 0;
    lastday = 0;
}
 
void InputData(void)
{
    register int h, r, c;
    scanf("%d %d %d", &C, &R, &H);
    for (h = 1; h <= H; h++) {
        for (r = 1; r <= R; r++) {
            for (c = 1; c <= C; c++) {
                scanf("%d", &box[h][r][c]);
                if (box[h][r][c] == 1) push(h, r, c, 0);
                if (box[h][r][c] == 0) not_ripen++;
            }
        }
    }
}
 
int BFS(void)
{
    register int i;
    if (not_ripen == 0) return 0;
    while (!empty()) {
        Queue p = pop();
        for (i = 0; i < 6; i++) {
            push(p.h + dh[i], p.r + dr[i], p.c + dc[i], p.day + 1);
        }
    }
    if (not_ripen) return -1;
    return lastday;
}
 
int main(void)
{
    Initial();
    InputData();
    printf("%d", BFS());
    return 0;
}
```



5. 보물섬(JUNG 1462)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
 
typedef struct _Q{
    int r, c, h;
}Queue;
 
Queue Q[55 * 55];
Queue cand[55 * 55];
char map[55][55];
int visit[55][55];
int R, C;
int rp, wp, cp;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
int empty(void) { return wp == rp; }
Queue pop(void) { return Q[rp++]; }
int push(int r, int c, int h)
{
    if (visit[r][c]) return 0;
    if (map[r][c] != 'L') return 0;
    visit[r][c] = 1;
    Q[wp].r = r; Q[wp].c = c; Q[wp].h = h; wp++;
    return h;
}
 
void cand_check(int r, int c)
{
    register int i, L_cnt = 0;
    for (i = 0; i < 4; i++) {
        if (map[r + dr[i]][c + dc[i]] == 'L') L_cnt++;
    }
    if (L_cnt == 1) {
        cand[cp].r = r;
        cand[cp].c = c;
        cp++;
    }
}
 
void Initial(void)
{
    //memset(map, 0, sizeof(map));
    memset(Q, 0, sizeof(Q));
    memset(visit, 0, sizeof(visit));
    rp = wp = 0;
}
 
void InputData(void)
{
    register int r, c;
    scanf("%d %d", &R, &C);
    for (r = 1; r <= R; r++) {
            scanf("%s", &map[r][1]);
    }
 
    for (r = 1; r <= R; r++) {
        for (c = 1; c <= C; c++) {
            if (map[r][c] == 'L') {
                cand[cp].r = r;
                cand[cp].c = c;
                cp++;
            }
        }
    }
 
    //for (r = 1; r <= R; r++) {
    //  for (c = 1; c <= C; c++) {
    //      if (map[r][c] == 'L') {
    //          cand_check(r, c);
    //      }
    //  }
    //}
}
 
int BFS(Queue start)
{
    register int i, max = -1, tmp;
    push(start.r, start.c, 0);
    while (!empty()) {
        Queue p = pop();
        for (i = 0; i < 4; i++) {
            tmp = push(p.r+dr[i], p.c+dc[i], p.h + 1);
            if (max < tmp) max = tmp;
        }
    }
    return max;
}
 
int main(void)
{
    int tc, s_max, L_max = 0;
    InputData();
    for (tc = 0; tc < cp; tc++) {
        s_max = BFS(cand[tc]);
        if (L_max < s_max) L_max = s_max;
        Initial();
    }
    printf("%d", L_max);
    return 0;
}
```



6. 로봇(JUNG 1006)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
#define change_d(x, y) (dir[index_d[(x)]+(y)])
 
typedef struct _Q{
    int r, c, d, o;
}Queue;
 
Queue Q[110 * 110 * 15];
int dir[6] = { 4, 1, 3, 2, 4, 1 };
int index_d[] = { 0, 1, 3, 2, 4 };  //1->1, 3->2, 2->3, 4->4
int R, C;
int rp, wp;
int map[110][110];
int visit[110][110][5];
int sr, sc, sd;
int er, ec, ed;
int dr[5][3] = { { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 }, { 1, 2, 3 }, { -1, -2, -3 } }; //1동 2서 3남 4북
int dc[5][3] = { { 0, 0, 0 }, { 1, 2, 3 }, { -1, -2, -3 }, { 0, 0, 0 }, { 0, 0, 0 } };
 
int empty(void) { return rp == wp; }
Queue pop(void) { return Q[rp++]; }
int push(int r, int c, int d, int ord)
{
    if ((r < 1) || (R < r) || (c < 1) || (C < c)) return -2;
    if (visit[r][c][d]) return -2;
    if (map[r][c] == 1) return -1;
    visit[r][c][d] = 1;
    Q[wp].r = r; Q[wp].c = c; Q[wp].d = d; Q[wp].o = ord; wp++;
    if ((r == er) && (c == ec) && (d == ed)) return ord;
    return -2;
}
void Initial(void) 
{
    memset(map, 2, sizeof(map));
    memset(visit, 0, sizeof(visit));
    memset(Q, 0, sizeof(Q));
    rp = wp = 0;
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
    scanf("%d %d %d", &sr, &sc, &sd);
    scanf("%d %d %d", &er, &ec, &ed);
}
int solve(void) 
{
    register int i, ans = -1;
    push(sr, sc, sd, 0);
    while (!empty()) {
        Queue p = pop();
        for (i = 0; i < 2; i++) {
            ans = push(p.r, p.c, change_d(p.d, 1-i*2), p.o+1);
            if (ans >= 0) return ans;
        }
        for (i = 0; i < 3; i++) {
            ans = push(p.r + dr[p.d][i], p.c + dc[p.d][i], p.d, p.o+1);
            if (ans == -1) break;
            if (ans >= 0) return ans;
        }
    }
    return ans;
}
 
 
int main(void)
{
    Initial();
    InputData();
    printf("%d", solve());
    return 0;
}
```



7. 구슬 집어넣기 게임

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
typedef struct _Q{
    int red_r, red_c;
    int blue_r, blue_c;
    int trial;
}Queue;
 
 
char map[20][20];
int visit[20][20];
Queue Q[20 * 20 * 2];
int R, C;
int rp, wp;
int red_sr, red_sc;
int blue_sr, blue_sc;
int er, ec;
int dr[] = { -1, 1, 0, 0 };
int dc[] = { 0, 0, -1, 1 };
 
int empty(void) { return rp == wp; }
Queue pop(void) { return Q[rp++]; }
int push(int rr, int rc, int br, int bc, int dr, int dc, int t)
{
    if (visit[rr][rc]) return -1; //빨강이 방문한 적 있는 곳이면 취소
    if ((map[rr][rc] == '#') && (map[br][bc] == '#')) return -1; //이동했을 때, 둘 다 장애물이 있으면 취소
    if (t > 10) return -1;
    if (map[rr][rc] == '#') { //빨간구슬: 장애물이면 제자리로 돌아감, 장애물이 아니면 그대로 이동
        rr -= dr;
        rc -= dc;
    }
    if (map[br][bc] == '#') { //파란구슬: 장애물이면 제자리로 돌아감, 장애물이 아니면 그대로 이동
        br -= dr;
        bc -= dc;
    }
    if ((rr == br) && (rc == bc)) return -1; //둘이 만나면 실패
    if ((br == er) && (bc == ec)) return -1; //파랑이 들어가면 실패!
    if ((rr == er) && (rc == ec)) return t; //골인!
    visit[rr][rc] = 1; 
    Q[wp].red_r = rr; Q[wp].red_c = rc; 
    Q[wp].blue_r = br; Q[wp].blue_c = bc; 
    Q[wp].trial = t; wp++;
    return -1;
}
 
void Initial(void)
{
    memset(map, 0, sizeof(map));
    memset(Q, 0, sizeof(Q));
    memset(visit, 0, sizeof(visit));
    rp = wp = 0;
}
 
void InputData(void)
{
    register int r, c;
    scanf("%d %d", &R, &C);
    for (r = 1; r <= R; r++) {
        scanf("%s", &map[r][1]);
    }
    for (r = 1; r <= R; r++) {
        for (c = 1; c <= C; c++) {
            if (map[r][c] == 'R') {
                red_sr = r; red_sc = c;
            }
            else if (map[r][c] == 'B') {
                blue_sr = r; blue_sc = c;
            }
            else if (map[r][c] == 'H') {
                er = r; ec = c;
            }
        }
    }
}
 
int BFS(void)
{
    register int i, ans = -2;
    push(red_sr, red_sc, blue_sr, blue_sc, 0, 0, 0);
    while (!empty()) {
        Queue p = pop();
        for (i = 0; i < 4; i++) {
            ans = push(p.red_r + dr[i], p.red_c + dc[i], p.blue_r + dr[i], p.blue_c + dc[i], dr[i], dc[i], p.trial + 1);
            if (ans >= 0) return ans;
        }
    }
    return ans;
}
 
int main(void)
{
    int i, TC;
    scanf("%d", &TC);
    for (i = 0; i < TC; i++) {
        Initial();
        InputData();
        printf("%d\n", BFS());
    }
    return 0;
}
```

