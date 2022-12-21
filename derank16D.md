Nhận xét:

Ta cho mảng $s$ là mảng prefix xor sum, ta có:
$\left\{ \begin{array}{l}
{s_1} = {a_1} &  ,{\rm{if }}\;i = 1\\
{s_i} = {s_{i - 1}} \oplus {a_i} & ,{\rm{ if }}\;i > 1
\end{array} \right.$

Ta có thể kết luận kết quả $a_p \oplus a_{p+1} \oplus \cdots \oplus a_n = s_n \oplus s_{p - 1}$.

## Subtask 1: 
- Với truy vấn REPONSE, ta có thể sử dụng mảng động (i.e vector) hoặc mảng tĩnh có $n + q$ phần tử để lưu mảng prefix $s_i$.

- Với truy vấn REQUEST, ta chạy một vòng lặp $\forall i \in [l; r]$ và tìm kết quả $\max(s_n \oplus s_{i -1} \oplus x)$.

**Độ phức tạp:** $O(n \times q)$.

---

Nhận xét 2: Ta bắt buộc sử dụng 01 trie để giải cho các subtask sau này.

### 1. 01 Trie

Định nghĩa: Là cây trie nhưng chỉ sử dụng hai số 0 và 1 để dựng lên cây, và các truy vấn update cây sẽ sử dụng kiểu pointer để tối ưu bộ nhớ nhất có thể. Ví dụ hình minh hoạ bên dưới:

![Persistence 01 Trie](https://i.postimg.cc/XqgrPCDz/134213-drawio.png)

### 2. XOR trên 01 Trie

Bây giờ, thay vì ta lưu từng phần tử trong mảng vào 01 trie, thì ta lưu prefix (xor sum) của mảng vào cây 01 trie. Ví dụ với mảng $a = [3, 4, 5, 6]$ thì prefix xor sẽ là $s = [3, 7, 2, 4]$.

![Prefix 01 Trie](https://i.postimg.cc/VNQvRgKF/xor-max-ex2-drawio.png)

Bây giờ, với một số $x$ bất kì, để tìm được kết quả $s_i \oplus x$ lớn nhất có thể. Ta sẽ biểu diễn $x$ thành hệ nhị phân, bắt đầu với bit lớn nhất và tham lam bit $\oplus\;1$ trên cây Trie này.

Nên ví dụ với $x = 6_{10} = 110_{2}$, ta sẽ tham lam từ bit cao nhất đến thấp nhất.
- Với bit $2$ là $1$, trie **có** một một đỉnh đến $1 \oplus 1 = 0$, ta sẽ tham theo đường đó.
- Với bit $1$ là $1$, trie **không** có đỉnh đến $1 \oplus 1 = 0$, nên phải đi theo cạnh bit $1$.
- Với bit $0$ là $1$, trie **có** một đỉnh đến $0 \oplus 1 = 1$, ta sẽ tham theo đường đó.

Kết quả là trie sẽ trỏ về $2$ tức kết quả xor maximum là $6 \oplus 3 = 5$ hoặc tính cách khác sẽ là $1 \times 2^2 + 0 \times 2^1 + 1 \times 2^0 = 5$ (có + không + có - qua bốn truy vấn trên)

Giải thích: trong hệ nhị phân, biểu diễn bit thứ $n$ ta thấy: $\displaystyle 2^n > \sum_{i=0}^{n-1} 2^{i}$, bạn sẽ thấy rằng việc chọn bit 1 bên trái nhất có thể là điều cần thiết. Nhưng với phép xor này thì nó sẽ đảo ngược lại thì ta bắt buộc phải chọn bit $\oplus\;1$ đối với biến $x$ đó. Từ đó kết quả của việc tham làm này là $\text{answer} = \displaystyle \sum_{i=0} (\exists\;[\text{node }2^i \oplus 1] \times 2^i)$

Tóm lại từ hai nhận xét trên, thuật toán 01 trie sẽ là:
```python
def insert(x):
	Phân tích bit lớn nhất đến nhỏ nhất của x:
		if (tại định đang xét không có đường đến bit đó):
			Tạo một đỉnh mới
		Đi theo đỉnh (bit)
def query(x):
	answer = 0
	Phân tích bit lớn nhất đến nhỏ nhất của x:
		if (tồn tại một đỉnh kế tiếp là (bit xor 1)):
			answer = answer + (1 << bit)
	        Đi theo đỉnh (bit xor 1) đó
	    else:
	        Đi theo đỉnh (bit) đó
	return answer
```

## Subtask 2

Quay về subtask 2 sau nhận xét to trên, vì truy vấn luôn rằng buộc $l = 1$ và $r = n$, ta có thể dựng cây 01 Trie và tìm query tra kết quả một cách dễ dàng.

- Với truy vấn RESPONSE, ta sẽ thêm $s_n$ mới vào cây 01 Trie.
- Với truy vấn REQUEST, ta sẽ chạy tìm kết quả query($s_n \oplus x$). (Bởi vì ta đang cần tìm $\max(s_n \oplus s_{i -1} \oplus x)$, với biến $s_n$ và $x$ có sẵn thì phải tìm $s_{i - 1}$ thoả)

**Độ phức tạp**: $O((n+q) \times log_{2}(a_i)) = O(24(n+q))$

## Subtask 4

Ở subtask này, ta sẽ không quan tâm mảng được update hay không nữa mà bắt đầu quan tâm truy vấn có $1 \le l \le r \le n$, nên bắt buộc phải dựng **Persistence 01 Trie** tại subtask này.

Tại truy vấn REQUEST, ta cần tìm $p \in [l - 1; r - 1]$ sao cho $s_n \oplus s_p \oplus x$ là lớn nhất có thể. 

- Với điều kiện $p \le r - 1$ thì ta có thể sử dụng Persistence Trie để quay về phiên bản tại $r - 1$. Nhưng để đơn giản hoá nhất có thể, ta có thể tạo một mảng chứa các truy vấn và sắp xếp toàn bộ $r_i$ tăng dần, sau đó tại thời điểm $t > r$ thì ta sẽ insert các phần tử dần dần vào.

- Nhưng còn điều kiện $l - 1 \le p$ thì trong cây Trie, ta có thể lưu thêm một mảng <$version$> để lưu đỉnh hiện tại là phần tử thứ $i$, khi ta thực hiện truy vấn insert thì sau khi đến lá thì sẽ đưa kết quả <$version$> lên cha dần dần. Tóm về khi query thì phải tìm đỉnh có $version_i \ge p$ thì mới đi đến đỉnh đó.

Kết hợp với **subtask 2**, ta chỉ đưa prefix xor-sum (mảng $s$) vào cây Trie và truy vấn từ từ để tìm $s_p$.

Code mẫu cây Trie sẽ là: 
```python
def insert(x):
	Phân tích bit lớn nhất đến nhỏ nhất của x:
		if (tại định đang xét có đường đến bit đó):
			Gán version[to] = max(version[to], now)
		else:
			Tạo một đỉnh mới
			Gán version[to] = now
		Đi theo đỉnh (bit)
def query(x, p):
	answer = 0
	Phân tích bit lớn nhất đến nhỏ nhất của x:
		if (tồn tại một đỉnh kế tiếp là (bit xor 1) và version[to] >= p):
			answer = answer + (1 << bit)
	        Đi theo đỉnh (bit xor 1) đó
	    else:
	        Đi theo đỉnh (bit) đó
	return answer
```

**Độ phức tạp**: $O(24(n+q))$.

## Subtask 5

Tóm lại từ **subtask 4,** ta chỉ còn việc xử lý các phần tử mới thêm vào trong truy vấn RESPONSE. Được kết hợp với **subtask 4** thì độ phức tạp cuối cùng của bài sẽ là $O(24(n + q) + q) \approx O(n+q)$.

## Code (C++):

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 4e5 + 6;
const int maxBIT = 24;

int n, q, cnt;
int a[N], sum[N], ans[N];

struct Query {
    int l, r, x, id;
};

bool cmp(Query a, Query b) { return a.r < b.r; }

struct Trie {
    int rt[N * 10][2], version[N * 10][2];

    void insert(int val, int id) {
        int now = 0;
        for(int i = maxBIT; i >= 0; --i) {
            int bit = ((val >> i) & 1);
            // Next node not exist
            if (!rt[now][bit]) {
                rt[now][bit] = ++cnt;
                version[now][bit] = id;
            } else {
                version[now][bit] = max(version[now][bit], id);
            }
            // Go to next node
            now = rt[now][bit];
        }
    }

    int query(int val, int id) {
        int now = 0, ans = 0;
        for(int i = maxBIT; i >= 0; --i) {
            int bit = ((val >> i) & 1);
            // Priority the opposite bit
            if (rt[now][bit ^ 1] && version[now][bit ^ 1] >= id) {
                now = rt[now][bit ^ 1];
                ans += (1 << i);
            } else {
                now = rt[now][bit];
            }
        }
        return ans;
    }
} T;

int main(int argc, char* argv[]) {
    std::ios_base::sync_with_stdio(false);

    vector<Query> que;

    cin >> n >> q;
    for(int i = 1; i <= n; ++i) cin >> a[i];
    for(int i = 1; i <= q; ++i){
        char tmp; cin >> tmp;
        if (tmp == 'A') {
            int x; cin >> x;
            a[++n] = x;
        } else {
            int l, r, x; cin >> l >> r >> x;
            // [l, r, N, value]: We will set the id later as we need calculate b.ans = sum[n] ^ x
            que.push_back({l, r, n, x});
        }
    }

    // Calculating prefix-xor
    for(int i = n; i >= 1; --i) sum[i] = sum[i + 1] ^ a[i];

    for(int i = 0; i <= que.size(); ++i) {
        que[i].x = sum[que[i].x + 1] ^ que[i].id;
        que[i].id = i;
    }

    sort(que.begin(), que.end(), cmp);
    int head = 1;
    for(int i = 0; i <= que.size(); ++i) {
        while (head <= que[i].r) {
            T.insert(sum[head], head);
            head++;
        }
        ans[que[i].id] = T.query(que[i].x, que[i].l);
    }

    for(int i = 0; i <= que.size(); ++i)
        cout << ans[i] << '\n';

    return 0; }

```
