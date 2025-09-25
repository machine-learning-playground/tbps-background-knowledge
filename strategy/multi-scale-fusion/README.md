# Multi-scale Fusion 

## 🧾 Tổng quan

Trong các bài toán truy xuất ảnh người đi bộ (pedestrian retrieval), do sự khác biệt giữa các lớp (inter-class variance) trong ảnh và mô tả văn bản thường rất nhỏ, nên cần khai thác thông tin toàn diện để phối hợp manh mối hình ảnh và ngôn ngữ ở nhiều cấp độ.

Kỹ thuật **Multi-scale feature fusion** (kết hợp đặc trưng đa tỉ lệ) được sử dụng để giải quyết vấn đề này bằng cách gộp các đặc trưng ở nhiều mức khác nhau [79], như minh họa trong hình sau.

![Multi-scale fusion](/strategy/multi-scale-fusion/img/multi-scale-fusion.png)

## Attribute Segmentation

Một số phương pháp tập trung vào việc **phân rã đặc trưng hình ảnh** thành các không gian con gắn với từng thuộc tính (attribute-specific subspaces), nhằm tăng cường khả năng liên kết (alignment) với mô tả văn bản.
- ViTAA (Visual-Text Attribute Alignment) do Wang et al. [24] đề xuất sử dụng một lớp attribute segmentation layer để chia ảnh người đi bộ thành nhiều phần như: toàn thân, đầu, áo trên, quần, giày và túi. Sau đó, các cụm từ văn bản tương ứng được trích xuất cho từng phần đã phân đoạn, giúp thực hiện liên kết chi tiết hơn giữa ảnh và văn bản.
- A-GANet (Image Structure Graph Network) do Liu et al. [30] giới thiệu thì sử dụng residual modules để trích xuất đặc trưng hình ảnh, kết hợp với graph attention convolutional layers để xây dựng biểu diễn ngữ nghĩa ở mức cao. Mạng này khai thác mối quan hệ giữa các bộ phận cơ thể người thông qua graph convolutional networks, từ đó nâng cao tính có cấu trúc trong biểu diễn đặc trưng hình ảnh.

