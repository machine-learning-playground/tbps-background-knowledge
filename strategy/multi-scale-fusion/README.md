# Multi-scale Fusion 

## 🧾 Tổng quan

Trong các bài toán truy xuất ảnh người đi bộ (pedestrian retrieval), do sự khác biệt giữa các lớp (inter-class variance) trong ảnh và mô tả văn bản thường rất nhỏ, nên cần khai thác thông tin toàn diện để phối hợp manh mối hình ảnh và ngôn ngữ ở nhiều cấp độ.

Kỹ thuật **Multi-scale feature fusion** (kết hợp đặc trưng đa tỉ lệ) được sử dụng để giải quyết vấn đề này bằng cách gộp các đặc trưng ở nhiều mức khác nhau [79], như minh họa trong hình sau.

![Multi-scale fusion](/strategy/multi-scale-fusion/img/multi-scale-fusion.png)

[79] Y. Wang, D. Qi, C. Zhao, Part-based multi-scale attention network for text-based person search, in: PRCV 2022, Springer, 2022, pp. 462–474.

## Attribute Segmentation

Một số phương pháp tập trung vào việc **phân rã đặc trưng hình ảnh** thành các không gian con gắn với từng thuộc tính (attribute-specific subspaces), nhằm tăng cường khả năng liên kết (alignment) với mô tả văn bản.
- **ViTAA (Visual-Text Attribute Alignment)** do Wang et al. [24] đề xuất sử dụng một lớp attribute segmentation layer để chia ảnh người đi bộ thành nhiều phần như: toàn thân, đầu, áo trên, quần, giày và túi. Sau đó, các cụm từ văn bản tương ứng được trích xuất cho từng phần đã phân đoạn, giúp thực hiện liên kết chi tiết hơn giữa ảnh và văn bản.
- **A-GANet (Image Structure Graph Network)** do Liu et al. [30] giới thiệu thì sử dụng residual modules để trích xuất đặc trưng hình ảnh, kết hợp với graph attention convolutional layers để xây dựng biểu diễn ngữ nghĩa ở mức cao. Mạng này khai thác mối quan hệ giữa các bộ phận cơ thể người thông qua **graph convolutional networks**, từ đó nâng cao tính có cấu trúc trong biểu diễn đặc trưng hình ảnh.

[24] Z. Wang, Z. Fang, J. Wang, Y. Yang, Vitaa: Visual-textual attributes alignment in person search by natural language, in: Computer Vision–ECCV 2020: 16th European Conference, Glasgow, UK, August 23–28, 2020, Proceedings, Part XII 16, Springer, 2020, pp. 402–420.

[30] J. Liu, Z.-J. Zha, R. Hong, M. Wang, Y. Zhang, Deep adversarial graph attention convolution network for text-based person search, in: Proceedings of the 27th ACM International Conference on Multimedia, Association for Computing Machinery, New York, NY, USA, 2019, pp. 665–673.

## Multi-Granularity Alignment

Một số hướng nghiên cứu tập trung vào việc **căn chỉnh (alignment) đặc trưng hình ảnh và văn bản ở nhiều mức độ chi tiết khác nhau (multi-granularity):**
- **Niu et al. [25]** đề xuất framework **MIA (Multi-Granularity Image-Text Alignment)**, sử dụng ba module để giải quyết bài toán căn chỉnh chi tiết giữa ảnh và văn bản ở mức độ fine-grained.
- **Zheng et al. [21]** phát triển mô hình **Hierarchical Adaptive Matching**, cho phép mô hình nắm bắt sự tương ứng chi tiết giữa ảnh và văn bản ở nhiều cấp: **từ, cụm từ, và câu** → giúp căn chỉnh đặc trưng chính xác hơn.
- **Wang et al. [80]** đưa ra **MGEL (Multi-Granularity Embedding Learning)**, mô hình này tăng hiệu quả truy xuất bằng cách trích xuất embedding từ ảnh người ở **nhiều tỉ lệ không gian khác nhau** (multi-scale).
- **Ji et al. [78]** giới thiệu **ACSA (Asymmetric Cross-Scale Alignment)**, kết hợp biểu diễn văn bản toàn cục (global text representations) với biểu diễn cụm từ cục bộ (local phrase representations), đồng thời chia đặc trưng hình ảnh thành các vùng quan trọng (ví dụ: đầu, thân, tay chân). Cách chia này giúp giữ lại các chi tiết quan trọng để căn chỉnh fine-grained, mà không làm tăng nhiều chi phí tính toán.

[21] K. Zheng, W. Liu, J. Liu, Z.-J. Zha, T. Mei, Hierarchical gumbel attention network for text-based person search, in: Proceedings of the 28th ACM international conference on multimedia, 2020, pp. 3441–3449.

[25] K. Niu, Y. Huang, W. Ouyang, L. Wang, Improving descriptionbased person re-identification by multi-granularity image-text alignments, IEEE Transactions on Image Processing 29 (2020) 5542–5556.

[78] Z. Ji, J. Hu, D. Liu, L. Y. Wu, Y. Zhao, Asymmetric cross-scale alignment for text-based person search, IEEE Transactions on Multimedia 25 (2022) 7699–7709.

[80] C. Wang, Z. Luo, Y. Lin, S. Li, Text-based person search via multigranularity embedding learning, in: Proceedings of the Thirtieth International Joint Conference on Artificial Intelligence, International Joint Conferences on Artificial Intelligence Organization, 2021, pp. 1068–1074.