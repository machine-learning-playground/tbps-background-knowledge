# Transformer

## 🧾 Tổng quan
**Transformer** không chỉ có khả năng mô hình hóa sâu (deep modeling) và khả năng mở rộng (scalability), mà còn thể hiện **khả năng thích ứng mạnh mẽ với nhiều modality khác nhau**.

Trong các nhiệm vụ đa phương thức, chẳng hạn như **Text-based Person Re-ID**, Transformer đã trở thành **nền tảng công nghệ quan trọng**, nhờ vào khả năng **trích xuất và căn chỉnh hiệu quả các đặc trưng văn bản và hình ảnh**. [77, 82, 119, 120, 78, 8, 99, 84, 118, 98, 90, 79, 2, 85, 34, 114, 121, 37, 122, 35, 38, 39, 40, 41, 42, 43, 44, 123]

Theo **cách mô hình xử lý văn bản và hình ảnh**, các framework công nghệ hiện nay có thể chia thành **hai loại chính**:
- Dual-stream Encoding
- Unified Large Models Era Encoding

## Dual-stream Encoding
Dual-stream Encoding sử dụng các **bộ mã hóa độc lập cho văn bản và hình ảnh** để trích xuất đặc trưng từ cả hai modality.
- Mỗi bộ mã hóa (ví dụ: BERT, Swin Transformer) tập trung vào kiểu dữ liệu riêng của nó:
    - Văn bản: sử dụng Transformer [94] và BERT [8, 99, 84, 118, 98, 90, 79, 2, 85, 34, 114, 121, 122, 45] để trích xuất đặc trưng ngữ cảnh.
    - Hình ảnh: sử dụng Vision Transformer [124, 121], Swin Transformer [78, 37, 45], Pyramid ViT [85]... để trích xuất đặc trưng đa tỉ lệ từ ảnh người.

Sau đó, đặc trưng của cả hai modality được **chiếu vào cùng một không gian** để so khớp thông qua một cơ chế căn chỉnh đặc thù (ví dụ: Contrastive Learning).

### Một số công trình tiêu biểu
- **Li et al.** [119]: đề xuất mạng **SAF (Semantically-Aligned Feature aggregation)** dựa trên Vision Transformer, có khả năng gom các đặc trưng có cùng ngữ nghĩa thành những đặc trưng cục bộ riêng biệt.
- **Ji et al.** [78]: đưa ra phương pháp **Asymmetric Cross-Scale Alignment (ACSA)** dùng Swin Transformer để căn chỉnh toàn cục giữa hình ảnh và văn bản, sau đó căn chỉnh động các thực thể cross-modal, giải quyết vấn đề lệch tỉ lệ (scale alignment).
- **Shao et al.** [118]: đề xuất **LGUR (Learning Granularity-Unified Representations)** dựa trên DeiT-Transformer, ánh xạ đặc trưng hình ảnh và văn bản vào một không gian thống nhất về mức độ chi tiết (granularity).
- **Ref.** [85]: xây dựng khung tokenization và căn chỉnh đặc trưng dựa trên Pyramid ViT, giúp dần thu hẹp khoảng cách giữa các modality và giữa các lớp (inter/intra-class).
- **Li et al.** [121]: phát triển mô hình **MSN-BRR (Multi-Granularity Separation Network with Bidirectional Refinement Regularization)**, chỉ dùng BERT để mã hóa văn bản, tập trung xử lý nhiễu liên-modal.
- **Yang et al.** [37]: đề xuất **APTM (Attribute Prompt Learning and Text Matching Learning)** dựa trên Swin Transformer, bổ sung explicit attribute recognition để cải thiện representation learning.
- **Ref.** [122]: khai thác thông tin bị bỏ sót bằng phương pháp dựa trên Vision Transformer, kết hợp khai thác đặc trưng tiến trình và tri thức ngoài để tinh lọc đặc trưng.
- **Li et al.** [45]: giải quyết vấn đề khoảng cách liên-modal và biến thiên nội/ngoại lớp bằng khung **AUL (Adaptive Uncertainty Based Learning)** dựa trên Swin Transformer.
- **Shu et al.** [94]: đề xuất khung **IVT (Implicit Visual Text)**, trong đó trích xuất đặc trưng văn bản và hình ảnh dùng chung một backbone Transformer (chia sẻ tham số). Cách này, tối ưu bằng dữ liệu cross-modal, cho phép mô hình học được một ánh xạ không gian chung.