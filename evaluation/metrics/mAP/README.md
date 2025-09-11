# Mean Average Precision(mAP)

Với mỗi truy vấn **i**, ta định nghĩa precision **P\_i(j)** tại vị trí **j** là tỉ lệ số kết quả đúng trong top-j kết quả trả về. 
Sau đó, với mỗi mẫu đúng (positive instance), ta tính trung bình các giá trị precision tại những vị trí mà mẫu đúng này xuất hiện. Kết quả cuối cùng chính là Average Precision (AP\_i) cho truy vấn đó.

![Công thức AP](/evaluation/metrics/mAP/img/APi.png)

Ở đây:
- **M\_i** là số lượng kết quả đúng (positive instances) cho truy vấn **i**.
- **∑** là phép cộng (tổng) thực hiện trên tất cả các kết quả đúng.

Ký hiệu **I(rank\_i = j)** là hàm chỉ thị (indicator function). Cụ thể, nó có giá trị bằng 1 nếu trong kết quả xếp hạng của truy vấn **i**, phần tử ở vị trí **j** là một kết quả đúng. Ngược lại, nếu không phải kết quả đúng thì nó bằng 0.

Nói cách khác, hàm này dùng để xác định xem ở vị trí **j** trong danh sách xếp hạng có phải là một kết quả đúng hay không.

Sau đó, **mAP (mean Average Precision)** được tính bằng cách lấy trung bình các giá trị Average Precision (AP) của tất cả các truy vấn.

![Công thức AP](/evaluation/metrics/mAP/img/mAP.png)

Ở đây:
- **N** là tổng số lượng truy vấn.
- **∑** biểu thị phép cộng (tổng) được thực hiện trên tất cả các truy vấn.