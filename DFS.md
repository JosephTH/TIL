1. 주사위 던지기1(JUNG 1169)

``` C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
 
int N, M;
int num[5];
int sel[10];
 
void DFS1(int n)
{  //주사위를 N번 던져서 나올 수 있는 모든 경우, 0부터 시작
    register int i;
    if (n >= N) {
        for (i = 0; i < N; i++) {
            printf("%d ", num[i]);
        }
        printf("\n");
        return;
    }
    for (i = 1; i <= 6; i++) {
        num[n] = i;
        DFS1(n + 1);
    }
}
 
void DFS2(int n, int s)
{  //주사위를 N번 던져서, 중복이 되는 경우를 제외하고 나올 수 있는 모든 경우!
   //112가 나오면, 121, 211은 안됨. 즉 앞에 나온 수 이상으로만 나와야함.
    register int i;
    if (n >= N) {
        for (i = 0; i < N; i++) {
            printf("%d ", num[i]);
        }
        printf("\n");
        return;
    }
    for (i = s; i <= 6; i++) {
        num[n] = i;
        DFS2(n + 1, i);
    }
}
 
void DFS3(int n)
{  //주사위를 N번 던져서 모두 다른 수가 나와야함!
   //앞에 나온 수는 '제외' => 앞에 나온 수를 체크한다.
    register int i;
    if (n >= N) {
        for (i = 0; i < N; i++) {
            printf("%d ", num[i]);
        }
        printf("\n");
        return;
    }
    for (i = 1; i <= 6; i++) {
        if (sel[i]) continue;
        sel[i] = 1;
        num[n] = i;
        DFS3(n + 1);
        sel[i] = 0;
    }
}
 
int main(void)
{
    scanf("%d %d", &N, &M);
    switch (M) {
    case 1: DFS1(0); break;
    case 2: DFS2(0, 1); break;
    case 3: DFS3(0); break;
    }
    return 0;
}
```



2. N QUEEN(JUNG 1889)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
 
int N;
int sol;
int map[14][14];
int sel[14];
int xy1[14 * 2 - 1];
int xy2[14 * 2 - 1];
 
void InputData(void)
{
    scanf("%d", &N);
}
 
int check(int r, int c)
{
    int tmp_r = r - 1, tmp_cl = c - 1, tmp_cr = c + 1;
    //대각선 좌/우 체크
    while (tmp_r >= 0) {
        if (tmp_cl >= 0 && map[tmp_r][tmp_cl]) return 0;
        if (tmp_cr < N && map[tmp_r][tmp_cr]) return 0;
        tmp_r--;
        tmp_cl--;
        tmp_cr++;
    }
    return 1;
}
 
void DFS(int n)
{
    register int i;
    if (n >= N) {
        sol++; return;
    }
    for (i = 0; i < N; i++) {
        if (sel[i]) continue; //세로로 없음
        if (xy1[n+i]) continue; //오른쪽 대각선으로 없음 0,0 / 1,0 , 0,1 / 2,0 1,1 0,2/
        if (xy2[N-1-(n-i)]) continue;    //왼쪽 대각선으로 없음 3,0 / 2,0  3,1 / 1,0 2,1 3,2 ... /0,3
        sel[i] = 1;
        xy1[n + i] = 1;
        xy2[N - 1 - (n - i)] = 1;
        DFS(n + 1);
        sel[i] = 0;
        xy1[n + i] = 0;
        xy2[N - 1 - (n - i)] = 0;
    }
}
 
int main(void)
{
    InputData();
    DFS(0);
    printf("%d", sol);
    return 0;
}
```



3. 스도쿠(JUNG 1824)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
 
//R행과 C열에 숫자 N이 있는가? 
//exist_r[r][N] exist_c[c][N]
//내가 속한 정사각형에 숫자 N이 있는가?
//exist_sq[(r/3) * 3 + c/3][N]
 
int map[10][10];
int Q_zero[10 * 10][2];
int Qi;
int exist_r[10][10];
int exist_c[10][10];
int exist_sq[10][10];
int R = 9, C = 9;
 
void InputData(void)
{
    register int r, c;
    int num;
    for (r = 0; r < R; r++) {
        for (c = 0; c < C; c++) {
            scanf("%d", &num);
            map[r][c] = num;
            if (num != 0) {
                exist_r[r][num] = 1;
                exist_c[c][num] = 1;
                exist_sq[(r / 3) * 3 + c / 3][num] = 1;
            }
            else {
                Q_zero[Qi][0] = r;
                Q_zero[Qi][1] = c;
                Qi++;
            }
        }
    }
}
 
void prt(void)
{
    register int r, c;
    for (r = 0; r < R; r++) {
        for (c = 0; c < C; c++) {
            printf("%d ", map[r][c]);
        }
        printf("\n");
    }
}
 
int fill(int n)
{
    register int i, r = Q_zero[n][0], c = Q_zero[n][1];
    if (n >= Qi) {
        prt();
        return 1;
    }
    for (i = 1; i <= 9; i++) {
        if (exist_r[r][i]) continue;
        if (exist_c[c][i]) continue;
        if (exist_sq[(r / 3) * 3 + c / 3][i]) continue;
        exist_r[r][i] = 1;
        exist_c[c][i] = 1;
        exist_sq[(r / 3) * 3 + c / 3][i] = 1;
        map[r][c] = i;
        if (fill(n + 1)) return 1;
        exist_r[r][i] = 0;
        exist_c[c][i] = 0;
        exist_sq[(r / 3) * 3 + c / 3][i] = 0;
        map[r][c] = 0;
    }
    return 0;
}
 
int main(void)
{
    InputData();
    fill(0);
    return 0;
}
 
```



4. 농장탈출 - USACO 2011 December Contest Bronze

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
//경우의 수를 계산해본다.
//1. 1~N번째 소 중에 하나를 정한다.
//2. 해당 소를 제외한 소 중에 하나를 정한다.
//3. 1과 2를 제외한 소 중에 하나를 정한다
//자리 올림이 발생하지 않는 한 위를 계속 한다.
//그와 동시에 몇마리까지 태웠는지, 최대보다 많이 태웠는지 확인하고 업데이트 한다.
 
int N;
int cow[21];
int sel[21];
int max;
 
 
long long add(long long a, long long b)
{
    register int i, sum = 0, digit_sum = 0;
    int digit[30];
    for(i=0; a || b; i++) {
        digit_sum = a % 10 + b % 10;
        if (digit_sum > 9) return 0;
        digit[i] = digit_sum;
        a /= 10;
        b /= 10;
    }
    while (i--) {
        sum = sum * 10 + digit[i];
    }
    return sum;
}
 
 
void Cowxodus(int n, int s, long long t_weight)
{
    register int i;
    long long notfat;
    if (n > max) max = n;
    if (n >= N) return;
    for (i = s; i < N; i++) {
        if (sel[i]) continue;
        notfat = add(t_weight, cow[i]);
        if (notfat == 0) continue;
        sel[i] = 1;
        Cowxodus(n + 1, i, notfat);
        sel[i] = 0;
    }
}
 
void InputData(void)
{
    register int i;
    scanf("%d", &N);
    for (i = 0; i < N; i++) {
        scanf("%d", &cow[i]);
    }
}
 
int main(void)
{
    InputData();
    Cowxodus(0, 0, 0);
    printf("%d", max);
    return 0;
}
```



5. 월드컵 - KOI 전국 2008 초2

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
#define A (0)
#define F (5)
 
typedef struct _Team{
    int w, d, l;
}T;
T player[6];
int sel[6][6];
int W, D, L;
 
void Initial(void)
{
    memset(player, 0, sizeof(player));
    memset(sel, 0, sizeof(sel));
}
 
int Play(int team, int w, int d, int l)
{
    register int i;
    int flag = 0;
    if ((w < 0) || (d < 0) || (l < 0)) return 0;
    if (w + d + l == 0) {
        if (team == F) return 1;
        int next_team = team + 1;
        flag = Play(next_team, player[next_team].w, player[next_team].d, player[next_team].l);
    }
    if (flag) return 1;
    for (i = team+1; i <= F; i++) {
        if (sel[team][i]) continue;
        sel[team][i] = 1;
        if (w) {
            player[i].l--;
            if (player[i].l < 0) {
                player[i].l++; sel[team][i] = 0; continue;
            }
            player[team].w--;
            flag = Play(team, w - 1, d, l);
            player[team].w++;
            player[i].l++;
        }
        else if (d) {
            player[i].d--;
            if (player[i].d < 0) {
                player[i].d++; sel[team][i] = 0; continue;
            }
            player[team].d--;
            flag = Play(team, w, d - 1, l);
            player[team].d++;
            player[i].d++;
        }
        else {
            player[i].w--;
            if (player[i].w < 0) {
                player[i].w++; sel[team][i] = 0; continue;
            }
            player[team].l--;
            flag = Play(team, w, d, l - 1);
            player[team].l++;
            player[i].w++;
        }
        sel[team][i] = 0;
        if (flag) return 1;
    }
    return 0;
}
 
int main(void)
{
    register int i,t;
    for (i = 0; i < 4; i++) {
        Initial();
        for (t = A; t <= F; t++) {
            scanf("%d %d %d", &player[t].w, &player[t].d, &player[t].l);
        }   
        printf("%d ", Play(A, player[0].w, player[0].d, player[0].l));
    }
    return 0;
}


#include <stdio.h>
typedef struct _wdl{
    int w, d, l;
}wdl;
int play[6 * 5][2];
wdl match[7];
int cnt;
 
void Set(void)
{
    register int i, j;
    for (i = 1; i <= 6; i++) {
        for (j = i + 1; j <= 6; j++) {
            play[cnt][0] = i; play[cnt][1] = j; cnt++;
        }
    }
}
 
int InputData(void)
{
    register int i, wrong = 0;
    for (i = 1; i <= 6; i++) {
        scanf("%d %d %d", &match[i].w, &match[i].d, &match[i].l);
        if (match[i].w + match[i].d + match[i].l != 5) wrong = 1;
    }
    if (wrong) return 1;
    return 0;
}
 
int DFS(int n)
{
    int me = play[n][0], you = play[n][1];
    if (n >= cnt) return 1;
    if (match[me].w && match[you].l) {
        match[me].w--; match[you].l--;
        if(DFS(n + 1)) return 1;
        match[me].w++; match[you].l++;
    }
    if (match[me].d && match[you].d) {
        match[me].d--; match[you].d--;
        if (DFS(n + 1)) return 1;
        match[me].d++; match[you].d++;
    }
    if (match[me].l && match[you].w) {
        match[me].l--; match[you].w--;
        if (DFS(n + 1)) return 1;
        match[me].l++; match[you].w++;
    }
    return 0;
}
 
int main(void)
{
    register int i, wrong;
    Set();
    for (i = 0; i < 4; i++) {
        wrong = InputData();
        if (wrong) printf("%d ", 0);
        else printf("%d ", DFS(0));
    }
    return 0;
}
```



6. 해밀턴 순환회로(JUNG 1681)

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define MAX (1000)
 
int N;
int min = MAX;
int f_map[14][14];
int visited[14];
 
void InputData(void)
{
    register int r, c;
    scanf("%d", &N);
    for (r = 1; r <= N; r++) {
        for (c = 1; c <= N; c++) {
            scanf("%d", &f_map[r][c]);
        }
    }
}
 
void DFS(int s, int cnt, int cost)
{
    register int i;
    if (cost >= min) return;
    if (cnt >= N) {
        if (!f_map[s][1]) return;
        cost += f_map[s][1];
        if (cost < min) min = cost;
        return;
    }
    for (i = 2; i <= N; i++) {
        if (visited[i]) continue;
        if (!f_map[s][i]) continue;
        visited[i] = 1;
        DFS(i, cnt + 1, cost + f_map[s][i]);
        visited[i] = 0;
    }
}
 
int main(void)
{
    InputData();
    DFS(1, 1, 0);
    printf("%d", min);
    return 0;
}
```

