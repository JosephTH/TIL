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

