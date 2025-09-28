# Attention Mechanism

## 🧾 Tổng quan
**Căn chỉnh đa phương thức (cross-modal alignment)** rất khó khi phải ghép cặp chi tiết (fine-grained) giữa văn bản và hình ảnh.

Để giải quyết, **cơ chế Attention** được dùng vì nó giúp mô hình tập trung vào những đặc trưng quan trọng liên quan đến mô tả ngôn ngữ và đặc điểm trực quan của hình ảnh. Điều này làm cho việc hiểu và kiểm soát quá trình căn chỉnh trở nên trực quan hơn, mà không cần thêm thông tin phụ trợ.

- **Attention** có thể chia thành hai loại chính:
    - **Hard attention** [86], [21] → chọn một vùng hoặc đặc trưng cụ thể.
    - **Soft attention** → phân bổ trọng số mềm lên nhiều vùng/đặc trưng cùng lúc.
- Trong các nghiên cứu, **soft attention** lại được chia chi tiết hơn:
    - **Spatial attention** [80], [87]: tập trung vào các vùng không gian (ví dụ: đầu, thân, chân).
    - **Channel attention** [81], [88]: tập trung vào các kênh đặc trưng (ví dụ: màu sắc, texture).
    - **Mixed attention** [89], [83]: kết hợp cả spatial và channel.
    - **Non-local & contextual attention** [90], [82]: tìm mối liên hệ xa hoặc ngữ cảnh rộng giữa các vùng.
    - **Cross-modal attention** [91], [40], [92]: học sự liên kết trực tiếp giữa đặc trưng của ảnh và văn bản.

[21] K. Zheng, W. Liu, J. Liu, Z.-J. Zha, T. Mei, Hierarchical gumbel attention network for text-based person search, in: Proceedings of the 28th ACM international conference on multimedia, 2020, pp. 3441–3449.

[40] D. Jiang, M. Ye, Cross-modal implicit relation reasoning and aligning for text-to-image person retrieval, in: Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2023, pp. 2787–2797.

[80] C. Wang, Z. Luo, Y. Lin, S. Li, Text-based person search via multigranularity embedding learning, in: Proceedings of the Thirtieth International Joint Conference on Artificial Intelligence, International Joint Conferences on Artificial Intelligence Organization, 2021, pp. 1068–1074.

[81] S. Yan, H. Tang, L. Zhang, J. Tang, Image-specific information suppression and implicit local alignment for text-based person search, IEEE transactions on neural networks and learning systems (2023).

[82] C. Gao, G. Cai, X. Jiang, F. Zheng, J. Zhang, Y. Gong, P. Peng, X. Guo, X. Sun, Contextual non-local alignment over fullscale representation for text-based person search, arXiv preprint arXiv:2101.03036 (2021).

[83] C. Wang, Z. Luo, Z. Zhong, S. Li, Divide-and-merge the embedding space for cross-modality person search, Neurocomputing 463 (2021) 388–399.

[86] Z. Wang, A. Zhu, Z. Zheng, J. Jin, Z. Xue, G. Hua, Img-net: innercross-modal attentional multigranular network for description-based person re-identification, Journal of Electronic Imaging 29 (2020) 043028–043028.

[87] T.-Y. Liu, C. Zhu, L. Yang, Efficient text-based person search via single-stage identity-guided attribute parsing and alignment, in: 2022 26th International Conference on Pattern Recognition (ICPR), 2022, pp. 4111–4117.

[88] Y. Jing, C. Si, J. Wang, W. Wang, L. Wang, T. Tan, Cascade attention network for person search: Both image and text-image similarity selection, arXiv preprint arXiv:1809.08440 2 (2018) 5.

[89] Y. Li, H. Xu, J. Xiao, Hybrid attention network for language-based person search, Sensors 20 (2020) 5279.

[90] A. Farooq, M. Awais, J. Kittler, S. S. Khalid, Axm-net: Implicit cross-modal feature alignment for person re-identification, in: Proceedings of the AAAI conference on artificial intelligence, volume 36, 2022, pp. 4477–4485.

[91] K.-H. Lee, X. Chen, G. Hua, H. Hu, X. He, Stacked cross attention for image-text matching, in: Proceedings of the European conference on computer vision (ECCV), 2018, pp. 201–216.

[92] S. Zhang, D. Cheng, W. Luo, Y. Xing, D. Long, H. Li, K. Niu, G. Liang, Y. Zhang, Text-based person search in full images via semantic-driven proposal generation, in: Proceedings of the 4th International Workshop on Human-centric Multimedia Analysis, 2023, pp. 5–14.

## Spatial and Channel Attention
**Attention không gian (spatial) và kênh (channel)** tập trung vào việc khai thác các đặc trưng phân biệt ở mức cục bộ, đồng thời tận dụng thông tin bổ sung từ nhiều thang đo khác nhau.
- Wang et al. [80] đề xuất một mạng nơ-ron sâu dựa trên attention, có khả năng nắm bắt nhiều đặc trưng attention từ tầng đặc trưng thấp cho đến tầng ngữ nghĩa, nhằm biểu diễn người đi bộ một cách chi tiết hơn.
- Liu et al. [87] sử dụng module multi-head attention để trích xuất embedding ở nhiều mức độ chi tiết khác nhau từ dòng văn bản, kèm cơ chế lọc thích ứng để lấy ra các đặc trưng tinh chỉnh.
- Yan et al. [81] thiết kế một module **suppress thông tin đặc trưng theo ảnh**: nó dùng localization được dẫn dắt bởi quan hệ và channel attention filtering để loại bỏ nhiễu từ nền và môi trường, qua đó cải thiện căn chỉnh thông tin giữa ảnh và văn bản.
- Ref. [88] dùng channel attention để tập trung vào người trong ảnh bằng cách mở rộng đầu vào: ghép thêm **14 bản đồ độ tin cậy tư thế (pose confidence maps)** với 3 kênh ảnh gốc, từ đó tăng cường biểu diễn trực quan.

Tuy nhiên, **spatial và channel attention** thường được xây dựng trong một không gian đặc trưng nhất định. Điều này khiến chúng khó xử lý những tình huống phức tạp hơn, ví dụ như khi đầu vào là đa phương thức (multimodal) hoặc liên quan đến các quan hệ ngữ nghĩa phức tạp.

## Mixed Attention
Mixed Attention (chú ý hỗn hợp) là cách kết hợp nhiều loại attention khác nhau để tăng cường khả năng trích xuất đặc trưng.
- **Li et al.** [89] đề xuất một mạng nơ-ron tích chập dạng *cubic attention* kết hợp **spatial attention** (chú ý không gian) và **channel attention** (chú ý theo kênh). Cách này giúp khai thác tối đa thông tin bổ sung từ nhiều thang đo khác nhau, qua đó giải quyết tốt hơn bài toán *cross-modal alignment* (căn chỉnh giữa văn bản và hình ảnh).
- **Wang et al.** [83] thiết kế **Feature Division Network (FDN)**, trong đó input được chia thành **K biểu diễn ngữ nghĩa cục bộ** thông qua self-attention. Mỗi biểu diễn tương ứng với một phần cơ thể khác nhau của người, sau đó các biểu diễn này được gộp lại thành một embedding toàn cục gọn gàng.
- **Yang et al.** [37] sử dụng kiến trúc có cả **nhánh văn bản, hình ảnh và thuộc tính.** Hệ thống này khai thác mixed attention để kết hợp thông tin bổ sung giữa các đặc trưng, từ đó đạt được khả năng trộn (fusion) mạnh mẽ hơn.

[37] S. Yang, Y. Zhou, Z. Zheng, Y. Wang, L. Zhu, Y. Wu, Towards unified text-based person retrieval: A large-scale multi-attribute and language search benchmark, in: Proceedings of the 31st ACM International Conference on Multimedia, 2023, pp. 4492–4501.
