Đây là một bài tập đơn giản giúp bạn có thể luyện tập việc duyệt theo hàng, cột và các đường chéo trong một ma trận.

Đầu tiên bạn duyệt qua ma trận để biết được nước đi tiếp theo là `X` hay `O`. Sau đó thì bạn có thể duyệt qua các hàng, cột và các đường chéo để tìm ở đâu đang có 2 nước giống nhau và giống với nước tiếp theo thì bạn đã có kết quả.

## Code:

### C++:
<blockquote class="spoiler">
```c++
#include <bits/stdc++.h>
using namespace std;

void print(int x){
    if(x == 0) cout << "X\n";
    if(x == 1) cout << "A\n";
    if(x == 2) cout << "B\n";
    if(x == 3) cout << "D\n";
}

//kiểm tra hàng/cột
int brute_force_straight(vector< vector<int> > a, int next){
    for(int i = 1; i <= 3 ; ++i){
        int cnt[3] = {};
        for(int j = 1; j <= 3 ; ++j) ++cnt[a[i][j]]; //đếm số lượng các nước đã đánh/chưa đánh trong hàng/cột
        if(cnt[0] == 1 && cnt[next] == 2) return next; //khi đã có kết quả, trả về nước đi tiếp theo
    }
    return 0; //chưa thể dự đoán
}

//kiểm tra đường chéo
int brute_force_cross(vector< vector<int> > a, int next, int mode){
    int cnt[3] = {};
    for(int i = 1; i <= 3; ++i){
        int j = i;
        if(mode == 1) j = 3 - i + 1;
        ++cnt[a[i][j]]; //đếm số lượng các nước đã đánh/chưa đánh trong đường chéo
        if(cnt[0] == 1 && cnt[next] == 2) return next; //khi đã có kết quả, trả về nước đi tiếp theo
    }
    return 0; //chưa thể dự đoán
}

signed main(){
    int t;
    cin >> t;
    while(t--){
        int x = 0, o = 0, next = 0;
        vector< vector<int> > a(4, vector<int> (4, 0));
        vector< vector<int> > b(4, vector<int> (4, 0)); //ma trận chuyển vị của a
        for(int i = 1; i <= 3; ++i){
            for(int j = 1; j <= 3; ++j){
                char c; cin >> c;
                if(c == 'X') a[i][j] = 1, b[j][i] = 1, ++x;
                else if(c == 'O') a[i][j] = 2, b[j][i] = 2, ++o;
            }
        }
        if(x > o) next = 2; else next = 1; //nước đi tiếp theo là của X hay O

        int predict = brute_force_straight(a, next);
        if(predict != 0) {print(next); continue;}

        predict = brute_force_straight(b, next);
        if(predict != 0) {print(next); continue;}

        predict = brute_force_cross(a, next, 0);
        if(predict != 0) {print(next); continue;}

        predict = brute_force_cross(a, next, 1);
        if(predict != 0) {print(next); continue;}

        if(x + o == 8) {print(3); continue;} //hoà
        print(0); //chưa thể dự đoán
    }
}

```
</blockquote>
