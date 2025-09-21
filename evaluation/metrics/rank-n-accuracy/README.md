# Rank-N Accuracy

## 🧾 Tổng quan
Với mỗi truy vấn **i**, ta định nghĩa một hàm chỉ thị **I(rank_i ≤ n)**, có giá trị bằng 1 nếu ảnh đúng nằm trong top-n kết quả, và bằng 0 nếu ngược lại.

Sau đó, Rank-n accuracy được tính bằng trung bình của hàm này trên tất cả các truy vấn:

![Công thức Rank-n](/evaluation/metrics/rank-n-accuracy/img/rank-n-accuracy.png)

Trong đó:
- **N** là tổng số truy vấn.
- **∑** là phép cộng trên toàn bộ các truy vấn.

## 📊 Code
Đây chính là phần RaSa dùng để tính Rank-n, đã rút gọn ra thành function compute_rank_n() để có thể được gọi độc lập.

```python
import torch

def compute_rank_n(matches: torch.Tensor, k: int = 1) -> float:
    """
    Tính Rank-n Accuracy cho bài toán text-to-image retrieval.

    Args:
        matches (Tensor): ma trận nhị phân (num_queries x num_gallery),
                          matches[i, j] = 1 nếu gallery[j] là đúng cho query[i].
        k (int): giá trị n trong Rank-n (ví dụ: 1, 5, 10).

    Returns:
        rank_n (float): Rank-n accuracy (%).
    """
    # Lấy top-k kết quả đầu tiên cho mỗi query
    matches_k = matches[:, :k].sum(dim=-1)  

    # Với mỗi query, kiểm tra xem trong top-k có ít nhất 1 kết quả đúng hay không
    hits = (matches_k > 0).sum()

    # Tính trung bình trên tất cả query
    rank_n = 100.0 * hits / matches.size(0)
    return rank_n.item()
```