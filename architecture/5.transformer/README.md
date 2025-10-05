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

## Unified Large Models Era Encoding
Với sự phát triển mạnh mẽ của các **mô hình đa phương thức quy mô lớn được tiền huấn luyện (pre-trained multimodal models)**, các Unified Large Models (mô hình thống nhất) mang đến một **khung Transformer chung** giúp **kết hợp sâu giữa các đặc trưng đa phương thức** (ảnh và văn bản) thông qua **chia sẻ tham số hoặc tiền huấn luyện chung**.
Những mô hình này thể hiện khả năng tổng quát hóa (generalization) và hiệu suất vượt trội trong nhiều tác vụ thị giác-ngôn ngữ [35, 38].

Cụ thể, **đại diện chung giữa các mô thức (cross-modal representation)** được học thông qua **tiền huấn luyện trên bộ dữ liệu ảnh-văn bản cực lớn** (ví dụ như **400 triệu cặp ảnh–chú thích trong CLIP [126]**).
Nhờ quá trình huấn luyện này, **các đặc trưng chi tiết (fine-grained features)** mà Text-based Person Re-ID cần đã được học sẵn, nên các mô hình Unified Large Models chỉ cần **fine-tune nhẹ** để thích ứng với các tác vụ downstream như tìm người qua mô tả [39–44].

### Một số công trình tiêu biểu:
- **Yan et al.** [38] đề xuất **CFine**, khung khai thác thông tin chi tiết (fine-grained) được dẫn dắt bởi bộ mã hóa hình ảnh của CLIP, tận dụng tri thức có sẵn từ CLIP để khai thác thông tin mức chi tiết phục vụ TIReID (Text-Image Re-ID).
- **Ref.** [39] chỉ ra rằng chỉ sử dụng bộ mã hóa hình ảnh (visual encoder) mà bỏ qua bộ mã hóa văn bản (text encoder) sẽ làm mất đi khả năng căn chỉnh giữa các mô thức (modal alignment) mà CLIP đã học được trong quá trình tiền huấn luyện.
Vì vậy, các mô hình TPS (Text-based Person Search) sau đó đã tận dụng cả hai bộ mã hóa của CLIP để khai thác tối đa thông tin song phương.
- **IRRA** [40] giải quyết vấn đề biến dạng thông tin nội mô thức (intra-modal distortion) bằng cách suy luận quan hệ ẩn (implicit relational inference) và căn chỉnh đặc trưng mà không cần học giám sát trước giữa các vùng hình ảnh và từ mô tả.
- **Zuo et al.** [44] – CFAM (CLIP-based Fine-grained Alignment Model)
Đề xuất một kiến trúc CLIP-based cho truy hồi văn bản siêu chi tiết (ultra-fine-grained retrieval).
Mô hình này dùng decoder chia sẻ mức hạt (granularity decoder) và cơ chế hard-negative matching để tinh chỉnh căn chỉnh đa phương thức.

### Hướng khắc phục vấn đề thực tế:
Đối mặt với các vấn đề như **thiếu dữ liệu (incomplete data)**, **nhiễu dữ liệu (data noise)** và **quyền riêng tư (privacy)** trong môi trường thực tế mở, các nghiên cứu sau đã mở rộng khả năng thích ứng của CLIP:
- **Du et al. [41] – iTIReID (Incomplete Textual Image Re-ID)** sử dụng **đặc trưng phân cụm (clustering features)** để **bù đắp các mô thức bị thiếu**, cho phép mô hình học hiệu quả ngay cả với dữ liệu không đầy đủ.
- **Gong et al. [42] – RTIReID (Robust Text-Image Re-ID)** mở rộng khoảng cách giữa các lớp (inter-class distance) để giảm phương sai trong lớp (intra-class variance), đồng thời áp dụng **nearest-neighbor consistent complementation** nhằm khôi phục đặc trưng bổ sung chất lượng cao.
- **Qin et al. [43] – RDE (Robust Double Embedding)** đề xuất phương pháp học **liên kết ngữ nghĩa thị giác mạnh mẽ**, tránh mô hình bị sụp đổ (collapse) khi dữ liệu bị nhiễu, đồng thời tập trung vào **các mẫu hard-negative** để cải thiện hiệu suất.

Cả ba mô hình này đều sử dụng **CLIP-ViT** và **CLIP-Transformer (Xformer)** làm bộ mã hóa ảnh và văn bản.