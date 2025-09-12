# Rank-N Accuracy

## 🧾 Tổng quan
Với mỗi truy vấn **i**, ta định nghĩa một hàm chỉ thị **I(rank_i ≤ n)**, có giá trị bằng 1 nếu ảnh đúng nằm trong top-n kết quả, và bằng 0 nếu ngược lại.

Sau đó, Rank-n accuracy được tính bằng trung bình của hàm này trên tất cả các truy vấn:

![Công thức Rank-n](/evaluation/metrics/rank-n-accuracy/img/rank-n-accuracy.png)

Trong đó:
N là tổng số truy vấn.
∑ là phép cộng trên toàn bộ các truy vấn.