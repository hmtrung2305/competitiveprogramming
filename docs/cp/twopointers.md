# **2 con trỏ**

Đây là một phương pháp dùng để xử lý các bài trên mặt phẳng tuyến tính ví dụ như mảng hoặc mặt phẳng tọa độ. Cách nhận diện các bài toán để áp dụng phương pháp *2 con trỏ* cũng khá đơn giản nhưng có một điều kiện quan trọng để áp dụng.

Các phương pháp như *cửa sổ trượt (Sliding windows)* cũng là một trong những biến thể của *2 con trỏ*.

## Cách nhận diện bài toán
1. Đếm số đoạn liên tục $[L, R]$ sao cho thỏa mãn điều kiện bài toán.
2. Tìm số phần tử  trên các đoạn $[L, R]$ sao cho thỏa điều kiện bài toán.

## Cách tiếp cận chung
Ý tưởng là cố định các điểm cực trái để tìm được các điểm cực phải thỏa mãn điều kiện bài toán, hoặc ngược lại.

Khi cố định được một cực, ví dụ là cực trái $L$ thì sẽ chọn cực phải $R$ sao cho $R$ xa nhất có thể. Nghĩa là khi cố định $L$ thì sẽ tìm $R$ sao cho $(R - L)$ lớn nhất có thể.

**Lưu ý quan trọng**:
*2 con trỏ* chỉ áp dụng khi mà với mọi điểm cực trái $L$, cực phải $R$ và $L<R$ sao cho đoạn $A[L..R]$ thỏa mãn bài toán thì đoạn $A[L..R - 1]$ cũng phải thỏa mãn điều kiện bài toán.

## Các bài toán
### Bài toán 1
Cho mảng $A_i (0 \leq A_i \leq 10^9)$ có độ dài $N (1 \leq N \leq 10^5)$ với $i=0,1,2,..,N-1$.

Đếm số cặp $(L, R)$ sao cho
$$
\sum^{R}_{i=L}{A[i]} \leq k
$$

#### **Lời giải**
Để tiếp cận bài toán nhanh nhất thì chúng ta sẽ xét tất cả các cặp điểm $(L, R)$ sao cho thỏa mãn điều kiện bài toán.

```cpp
long long ans = 0;
for (int L = 0; L < N; L++) {
    long long sum = 0;
    for (int R = L; R < N; R++) {
        sum += A[R];
        if (sum <= k) {
            ans++;
        }
    }
}
```

Độ phức tạp thuật toán ở đây là $O(N^2)$ không phù hợp với giới hạn $N \leq 10^5$.

Để tiếp cận với phương pháp *2 con trỏ*, đầu tiên chúng ta hãy xét điều kiện để áp dụng *2 con trỏ*.

Nhận xét với $A[i] \geq 0$ và $L<R$ thì
$$
\sum_{i=L}^{R}{A[i]} \geq \sum_{i=L}^{R-1}{A[i]}
$$
nên
$$
\sum_{i=L}^{R}{A[i]} \leq k
\Rightarrow \sum_{i=L}^{R-1}{A[i]} \leq k
$$
Đã đủ điều kiện áp dụng *2 con trỏ*.

Ở đây chúng ta chỉ cần tối ưu cách tìm $R$. Ta tiếp tục nhận xét với $A[i] \geq 0$:

$$
\sum_{i=L'}^{R'}{A[i]}>k
\Rightarrow \sum_{i=L'}^{R'+1}{A[i]}>k
$$

Ta thấy với các đoạn $[L'..R']$ không thỏa mãn điều kiện đề bài thì các đoạn $[L'..R'], [L'..R'+1], ..., [L'..N-1]$ là không cần thiết. Vậy giờ với mỗi $L$ cần tìm $R$ xa nhất sao cho đoạn $[L..R]$ thỏa mãn điều kiện bài toán, thì các đoạn thỏa mãn bắt đầu từ $L$ sẽ là $[L..L], [L..L+1], ..., [L..R]$ và đóng góp $R-L+1$ đoạn. Với mỗi $L$ thì vẫn dùng vòng lặp `for` để xét từng điểm như cách tiếp cận đơn giản ở trên (*ở đây chúng ta có thể nhận xét rằng có thể dùng phương pháp tìm kiếm nhị phân để tìm $R$ với tính chất $R$ tăng thì tổng tăng*), chúng ta cần tiếp tục tối ưu cách tìm $R$. Ta nhận xét với $A[i] \geq 0$ với
$$
\sum_{i=L}^{R}{A[i]} \leq k
\Rightarrow \sum_{i=L+1}^{R}{A[i]} \leq k
$$
Dễ dàng thấy rằng khi tịnh tiến $L$ và $R$ giữ nguyên thì vẫn thỏa mãn điều kiện bài toán nên chỉ cần tịnh tiến $R$ khi tịnh tiến $L$. Đến đây ý tưởng code đã hình thành. Đây là bản chất của *2 con trỏ*.

#### **Code**
```cpp
long long ans = 0;
long long sum = A[0];
for (int L = 0, R = 0; L < N; L++) {
    while (R + 1 < N && sum + A[R + 1] <= k) {
        sum += A[R + 1];
        R++;
    }
    sum -= A[L];
    ans += R - L + 1;
}
```
Độ phức tạp: $O(N)$

### Bài toán 2
[Codeforces Round 171 (Div. 2) - B. Books](https://codeforces.com/contest/279/problem/B)

#### **Tóm tắt bài toán**
Cho mảng $a_i (0 \leq a_i \leq 10^9)$ có độ dài $n (1 \leq n \leq 10^5)$ với $i=0,1,2,..,n-1$.

Tìm dãy con liên tục dài nhất sao cho tổng các số trong đoạn đó bé hơn $t (1 \leq t \leq 10^9)$. Nói cách khác tìm giá trị lớn nhất $(R - L)$ sao cho
$$
\sum_{i=L}^{R}{a[i]} \leq t
$$

#### **Lời giải**
Xét điều kiện để áp dụng phương pháp *2 con trỏ*.

Với $A[i] \geq 0$ và $L<R$
$$
\sum_{i=L}^{R}{a[i]} \leq t
\Rightarrow \sum_{i=L}^{R - 1}{a[i]} \leq t
$$
Đã thỏa mãn điều kiện áp dụng *2 con trỏ*.

Tương tự như cách giải đếm số đoạn $[L..R]$ thì bài này đối với mỗi $L$ ta chỉ cần tìm $R$ xa nhất và cập nhật đáp án.

#### **Code**
```cpp
long long ans = 0;
long long sum = 0;
for (int l = 0, r = 0; l < n; l++) {
    while (r < n && sum + a[r] <= t) {
        sum += a[r];
        r++;
    }
    sum -= a[l];
    ans = std::max(ans, r - l);
}
```