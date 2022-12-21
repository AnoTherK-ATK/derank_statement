## Sắp tát 1

Ở subtask này chỉ cần vét $O(n^3)$ là được.

## Sắp tát 2

Ở subtask này duyệt qua từng cặp $(x,y)$ sao cho $2x \le y$, đếm số lượng $z \in [2y,n]$ còn lại bằng **prefix sum**.

=> Độ phức tạp: $O(n^2)$

## Sắp tát 3

Cũng ý tưởng trên nhưng cải tiến một chút.

Đó là duyệt từng vị trí $y$ đếm số cặp $(x,z)$ thoả điều kiện bằng **prefix sum**. Xem code để hiểu rõ hơn nha.

=> Độ phức tạp: $O(n)$


```c++
#include <bits/stdc++.h>
using namespace std;

const int N=2e5+7;
const int LIM=10;
const int MOD=1e9+7;

int n, k;
int a[N], cnt[LIM][N];

int calc(int x, int l, int r){
    if(l>r) return 0;
    return cnt[x][r]-cnt[x][l-1];
}

void read(){
    cin>>n>>k;
    for(int i=1; i<=n; i++) cin>>a[i];
}

void solve(){
    for(int i=1; i<=n; i++){
        a[i]%=10;
        cnt[a[i]][i]++;
        for(int x=0; x<LIM; x++) 
            cnt[x][i]+=cnt[x][i-1];
    }

    int ans=0;
    for(int i=2; i<n; i++) 
        for(int x=0; x<LIM; x++)
            for(int y=x; y<LIM; y++)
                if((x+a[i]+y)%10==k){
                    ans=(ans+1ll*calc(x,1,i/2)*calc(y,2*i,n)%MOD)%MOD;
                    if(y>x)
                        ans=(ans+1ll*calc(y,1,i/2)*calc(x,2*i,n)%MOD)%MOD;
                }
    cout<<ans;
}

int main(){
    read();
    solve();
    return 0;
}
```
