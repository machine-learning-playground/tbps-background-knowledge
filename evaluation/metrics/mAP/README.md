# Mean Average Precision(mAP)

## 🧾 Tổng quan
Với mỗi truy vấn **i**, ta định nghĩa precision **P\_i(j)** tại vị trí **j** là tỉ lệ số kết quả đúng trong top-j kết quả trả về. 
Sau đó, với mỗi mẫu đúng (positive instance), ta tính trung bình các giá trị precision tại những vị trí mà mẫu đúng này xuất hiện. Kết quả cuối cùng chính là Average Precision (AP\_i) cho truy vấn đó.

![Công thức AP](/evaluation/metrics/mAP/img/APi.png)

Ở đây:
- **M\_i** là số lượng kết quả đúng (positive instances) cho truy vấn **i**.
- **∑** là phép cộng (tổng) thực hiện trên tất cả các kết quả đúng.

Ký hiệu **I(rank\_i = j)** là hàm chỉ thị (indicator function). Cụ thể, nó có giá trị bằng 1 nếu trong kết quả xếp hạng của truy vấn **i**, phần tử ở vị trí **j** là một kết quả đúng. Ngược lại, nếu không phải kết quả đúng thì nó bằng 0.

Nói cách khác, hàm này dùng để xác định xem ở vị trí **j** trong danh sách xếp hạng có phải là một kết quả đúng hay không.

Sau đó, **mAP (mean Average Precision)** được tính bằng cách lấy trung bình các giá trị Average Precision (AP) của tất cả các truy vấn.

![Công thức mAP](/evaluation/metrics/mAP/img/mAP.png)

Ở đây:
- **N** là tổng số lượng truy vấn.
- **∑** biểu thị phép cộng (tổng) được thực hiện trên tất cả các truy vấn.

## 📊 Code
Đây chính là phần RaSa dùng để tính mAP, đã rút gọn ra thành function compute_mAP() để có thể được gọi độc lập.

```python
import torch

def compute_mAP(matches: torch.Tensor) -> float:
    """
    Compute mean Average Precision (mAP) given a matches matrix.

    Args:
        matches (Tensor): ma trận nhị phân (num_queries x num_gallery),
                          matches[i, j] = 1 nếu gallery[j] là đúng cho query[i].

    Returns:
        mAP (float): mean Average Precision (%).
    """
    # Số lượng ground truth đúng cho từng query
    real_num = matches.sum(dim=-1)   # (num_queries,)

    # Cumulative Match Curve (CMC): tính số lượng match tích lũy theo rank
    tmp_cmc = matches.cumsum(dim=-1).float()  # (num_queries x num_gallery)

    # Thứ hạng 1, 2, 3, ..., n (dùng để tính precision tại rank j)
    order = torch.arange(1, matches.size(1) + 1, dtype=torch.float)

    # Precision tại mỗi vị trí rank j = số match tích lũy / rank index
    tmp_cmc /= order

    # Giữ lại precision tại các vị trí có match thật sự (lọc bằng matches)
    tmp_cmc *= matches

    # Average Precision (AP) cho từng query
    AP = tmp_cmc.sum(dim=-1) / real_num

    # mean AP (mAP) trên toàn bộ query
    mAP = AP.mean() * 100.0
    return mAP.item()
```