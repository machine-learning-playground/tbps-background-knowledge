# Architecture
Khi ta ánh xạ **đặc trưng từ một modality (ví dụ: ảnh hoặc văn bản)** sang **không gian chung (common manifold)**, thì **phân bố đặc trưng của modality khác lại không hiển thị rõ ràng** trong không gian đó.
Điều này có nghĩa là việc **nhúng và căn chỉnh đặc trưng đa phương thức** trong cùng một không gian **phụ thuộc hoàn toàn vào khả năng học của mô hình**, thay vì phản ánh đúng phân bố dữ liệu gốc.

Nói cách khác, **thách thức lớn trong Cross-modal Person Re-ID** là:
- Làm sao để **phân bố đặc trưng trong không gian chung** phản ánh trung thực **phân bố trong không gian gốc của từng modality** (ảnh, văn bản).
- Điều này đòi hỏi mô hình phải có **năng lực đủ mạnh** để nắm bắt và hiểu được mối quan hệ giữa các modality.
- Và để đạt được điều đó, thường cần:
    - **Lượng dữ liệu đủ lớn**,
    - **Cấu trúc mô hình được thiết kế cẩn thận**.

📌 Hình sau minh họa các nhánh (branches) khác nhau của kiến trúc mô hình được sử dụng để giải quyết vấn đề này.

![Kiến trức](/architecture/img/architecture.jpg)