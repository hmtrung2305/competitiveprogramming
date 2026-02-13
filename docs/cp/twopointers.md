# 2 con trỏ

## Cách nhận diện bài toán:
1. Đếm số đoạn liên tục $[L, R]$ sao cho thỏa mãn điều kiện bài toán.
2. Tìm số phần tử  trên các đoạn $[L, R]$ sao cho thỏa điều kiện bài toán.

## Cách tiếp cận chung:
Ý tưởng là cố định các điểm cực trái để tìm được các điểm cực phải thỏa mãn điều kiện bài toán, hoặc ngược lại.
Khi chúng ta cố định một cực, ví dụ là cực trái $L$ thì khi chúng ta sẽ cố gắng chọn cực phải thì chúng ta nên chọn cực phải xa nhất có thể. Nghĩa là khi cố định $L$ thì ta tìm $R$ sao cho $(R - L)_max$

Lưu ý:
2 con trỏ chỉ áp dụng khi mà với mọi điểm cực trái $L$ và mọi điểm $R (R > L)$ sao cho $[L, R]$ đó thỏa mãn bài toán thì $[L, R - 1]$ cũng phải thỏa mãn điều kiện bài toán.

## Các bài toán:
### Bài toán 1:
Cho mảng $A_i (0 \leq A_i \leq 10^9)$ có độ dài $N (1 \leq N \leq 10^5)$ với $i=0,1,2,..,N-1$. Đếm số cặp $(l,r) (l \leq r)$ sao cho tổng từ $A[l..r] \leq k (k <= 10^9)$.

### Bài toán 2:

