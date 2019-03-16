### 아이디어성&최적화

1. 배열 구간의 합 구하기

> **N****개의 1차원 배열에 수들이 입력되어 있다.**
>
> **이 배열의 임의의 구간에 있는 수들의 합을 물어보는 Q개의 쿼리에 답하는 프로그램을 작성하시오**
>
> **쿼리에 답하는 중간에 배열의 어떤 원소도 값이 변경되지 않는다**

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

> **KOI** **부설 과학연구소에서는 많은 종류의 산성 용액과 알칼리성 용액을 보유하고 있다. 각 용액에는 그 용액의 특성을 나타내는 하나의 정수가 주어져있다. 산성 용액의 특성 값은 1부터 1,000,000,000까지의 양의 정수로 나타내고, 알칼리성 용액의 특성 값은 -1부터 -1,000,000,000까지의 음의 정수로 나타낸다.**
>
> **같은 양의 두 용액을 혼합한 용액의 특성 값은 혼합에 사용된 각 용액의 특성 값의 합으로 정의한다. 이 연구소에서는 같은 양의 두 용액을 혼합하여 특성 값이 0에 가장 가까운 용액을 만들려고 한다.**
>
> **예를 들어, 주어진 용액들의 특성 값이 [-99, -2, -1, 4, 98]인 경우에는 특성 값이 -99인 용액과 특성 값이 98인 용액을 혼합하면 특성 값이 -1인 용액을 만들 수 있고, 이 용액의 특성 값이 0에 가장 가까운 용액이다. 참고로, 두 종류의 알칼리성 용액 만으로나 혹은 두 종류의 산성 용액만으로 특성 값이 0에 가장 가까운 혼합 용액을 만드는 경우도 존재할 수 있다.**
>
> **산성 용액과 알칼리성 용액의 특성 값이 정렬된 순서로 주어졌을 때, 이 중 두 개의 서로 다른 용액을 혼합하여 특성 값이 0에 가장 가까운 용액을 만들어내는 두 용액을 찾는 프로그램을 작성하시오**
>
>  
>
> **KOI** **부설 과학연구소에서는 많은 종류의 산성 용액과 알칼리성 용액을 보유하고 있다. 각 용액에는 그 용액의 특성을 나타내는 하나의 정수가 주어져있다. 산성 용액의 특성 값은 1부터 1,000,000,000까지의 양의 정수로 나타내고, 알칼리성 용액의 특성 값은 -1부터 -1,000,000,000까지의 음의 정수로 나타낸다.**
>
>  
>
> **같은 양의 두 용액을 혼합한 용액의 특성 값은 혼합에 사용된 각 용액의 특성 값의 합으로 정의한다. 이 연구소에서는 같은 양의 두 용액을 혼합하여 특성 값이 0에 가장 가까운 용액을 만들려고 한다.**
>
>  
>
> **예를 들어, 주어진 용액들의 특성 값이 [-99, -2, -1, 4, 98]인 경우에는 특성 값이 -99인 용액과 특성 값이 98인 용액을 혼합하면 특성 값이 -1인 용액을 만들 수 있고, 이 용액의 특성 값이 0에 가장 가까운 용액이다. 참고로, 두 종류의 알칼리성 용액 만으로나 혹은 두 종류의 산성 용액만으로 특성 값이 0에 가장 가까운 혼합 용액을 만드는 경우도 존재할 수 있다.**
>
>  
>
> **산성 용액과 알칼리성 용액의 특성 값이 정렬된 순서로 주어졌을 때, 이 중 두 개의 서로 다른 용액을 혼합하여 특성 값이 0에 가장 가까운 용액을 만들어내는 두 용액을 찾는 프로그램을 작성하시오.**

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

4. 회전 초밥(고) (KOI 본선 2012 고2)

> **회전 초밥 음식점에는 회전하는 벨트 위에 여러 가지 종류의 초밥이 접시에 담겨 놓여 있고, 손님은 이 중에서 자기가 좋아하는 초밥을 골라서 먹는다. 초밥의 종류를 번호로 표현할 때, 다음 그림은 회전 초밥 음식점의 벨트 상태의 예를 보여주고 있다. 벨트 위에는 같은 종류의 초밥이 둘 이상 있을 수 있다.**
>
> **새로 문을 연 회전 초밥 음식점이 불경기로 영업이 어려워서, 다음과 같이 두 가지 행사를 통해서 매상을 올리고자 한다.**
>
>  
>
> **1.** **원래 회전 초밥은 손님이 마음대로 초밥을 고르고, 먹은 초밥만큼 식대를 계산하지만, 벨트의 임의의 한 위치부터 k개의 접시를 연속해서 먹을 경우 할인된 정액 가격으로 제공한다.**
>
> **2.** **각 고객에게 초밥의 종류 하나가 쓰인 쿠폰을 발행하고, 1번 행사에 참가할 경우 이 쿠폰에 적혀진 종류의 초밥 하나를 추가로 무료로 제공한다. 만약 이 번호에 적혀진 초밥이 현재 벨트 위에 없을 경우, 요리사가 새로 만들어 손님에게 제공한다.**
>
>  
>
> **위 할인 행사에 참여하여 가능한 한 다양한 종류의 초밥을 먹으려고 한다. 위 그림의 예를 가지고 생각해보자. k=4이고, 30번 초밥을 쿠폰으로 받았다고 가정하자. 쿠폰을 고려하지 않으면 4가지 다른 초밥을 먹을 수 있는 경우는 (9, 7, 30, 2), (30, 2, 7, 9), (2, 7, 9, 25) 세 가지 경우가 있는데, 30번 초밥을 추가로 쿠폰으로 먹을 수 있으므로 (2, 7, 9, 25)를 고르면 5가지 종류의 초밥을 먹을 수 있다.**
>
>  
>
> **회전 초밥 음식점의 벨트 상태, 메뉴에 있는 초밥의 가짓수, 연속해서 먹는 접시의 개수, 쿠폰 번호가 주어졌을 때, 손님이 먹을 수 있는 초밥 가짓수의 최대값을 구하는 프로그램을 작성하시오.**

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

> **아래 그림과 같이 직선 도로상에 왼쪽부터 오른쪽으로 1번부터 차례대로 번호가 붙여진 마을들이 있다. 마을에 있는 물건을 배송하기 위한 트럭 한대가 있고, 트럭이 있는 본부는 1번 마을 왼쪽에 있다. 이 트럭은 본부에서 출발하여 1번 마을부터 마지막 마을까지 오른쪽으로 가면서 마을에 있는 물건을 배송한다.**
>
>  
>
> **![img](http://www.codexpert.org:82/JudgeOnline/upload/pimg2033_1.bmp)**
>
>  
>
> **각 마을은 배송할 물건들을 박스에 넣어 보내며, 본부에서는 박스를 보내는 마을번호, 박스를 받는 마을번호와 보낼 박스의 개수를 알고 있다. 박스들은 모두 크기가 같다. 트럭에 최대로 실을 수 있는 박스의 개수, 즉 트럭의 용량이 있다. 이 트럭 한대를 이용하여 다음의 조건을 모두 만족하면서 최대한 많은 박스들을 배송하려고 한다.**
>
> **조건 1: 박스를 트럭에 실으면, 이 박스는 받는 마을에서만 내린다.****조건 2: 트럭은 지나온 마을로 되돌아가지 않는다.****조건 3: 박스들 중 일부만 배송할 수도 있다.**
>
> **마을의 개수, 트럭의 용량, 박스 정보(보내는 마을번호, 받는 마을번호, 박스 개수)가 주어질 때, 트럭 한 대로 배송할 수 있는 최대 박스 수를 구하는 프로그램을 작성하시오. 단, 받는 마을번호는 보내는 마을번호보다 항상 크다.**
>
> **예를 들어, 트럭 용량이 40이고 보내는 박스들이 다음 표와 같다고 하자.**
>
> **![img](http://www.codexpert.org:82/JudgeOnline/upload/pimg2033_2.bmp)**
>
> **이들 박스에 대하여 다음과 같이 배송하는 방법을 고려해 보자.**
>
> **(1) 1**번 마을에 도착하면■ 다음과 같이 박스들을 트럭에 싣는다. (1번 마을에서 4번 마을로 보내는 박스는 30개 중 10개를 싣는다.)**
>
> ![img](http://www.codexpert.org:82/JudgeOnline/upload/pimg2033_3.bmp)
>
> **(2) 2**번 마을에 도착하면■ 트럭에 실려진 박스들 중 받는 마을번호가 2인 박스 10개를 내려 배송한다. (이때 트럭에 남아있는 박스는 30개가 된다.)■ 그리고 다음과 같이 박스들을 싣는다. (이때 트럭에 실려 있는 박스는 40개가 된다.)
>
> ![img](http://www.codexpert.org:82/JudgeOnline/upload/pimg2033_4.bmp)
>
> **(3) 3**번 마을에 도착하면■ 트럭에 실려진 박스들 중 받는 마을번호가 3인 박스 30개를 내려 배송한다. (이때 트럭에 남아있는 박스는 10개가 된다.)■ 그리고 다음과 같이 박스들을 싣는다. (이때 트럭에 실려 있는 박스는 30개가 된다.)
>
> ![img](http://www.codexpert.org:82/JudgeOnline/upload/pimg2033_5.bmp)
>
> **(4) 4**번 마을에 도착하면■ 받는 마을번호가 4인 박스 30개를 내려 배송한다.**
>
> **위와 같이 배송하면 배송한 전체 박스는 70개이다. 이는 배송할 수 있는 최대 박스 개수이다.**

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

> **오늘은 공주님이 태어난 경사스러운 날이다. 왕은 이 날을 기념하기 위해 늘 꽃이 피어있는 작은 정원을 만들기로 결정했다.**
>
> **총 N개의 꽃이 있는데, 꽃은 모두 같은 해에 피어서 같은 해에 진다. 하나의 꽃은 피는 날과 지는 날이 정해져 있다. 예를 들어, 5월 8일 피어서 6월 13일 지는 꽃은 5월 8일부터 6월 12일까지는 꽃이 피어 있고, 6월 13일을 포함하여 이후로는 꽃을 볼 수 없다는 의미이다. (올해는 4, 6, 9, 11월은 30일까지 있고, 1, 3, 5, 7, 8, 10, 12월은 31일까지 있으며, 2월은 28일까지만 있음)**
>
> **이러한 N개의 꽃들 중에서 다음의 두 조건을 만족하는 꽃들을 선택하고 싶다.**
>
> **1.** **공주가 가장 좋아하는 계절인 3월 1일부터 11월 30일까지 매일 꽃이 한 가지 이상 피어 있도록 한다.**
>
> **2.** **정원이 넓지 않으므로 정원에 심는 꽃들의 수를 가능한 적게 한다.**
>
> **N****개의 꽃들 중에서 위의 두 조건을 만족하는, 즉 3월 1일부터 11월 30일까지 매일 꽃이 한 가지 이상 피어 있도록 꽃들을 선택할 때, 선택한 꽃들의 최소 개수를 출력하는 프로그램을 작성하시오.**
>
>  
>
> **<****제약조건>**
>
> **전체 테스트 데이터의 20%는 N≤30**
>
> **전체 테스트 데이터의 50%는 N≤10,000**

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

