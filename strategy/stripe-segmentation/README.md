# Stripe Segmentation 

## 🧾 Tổng quan
**Stripe Segmentation** là một kỹ thuật được sử dụng rộng rãi trong bài toán person Re-ID để giảm thiểu ảnh hưởng của tư thế khác nhau, vật thể che khuất và điều kiện ánh sáng đến hiệu năng nhận dạng [25, 70].

Ý tưởng cốt lõi là chia ảnh người đi bộ thành nhiều dải dọc, mỗi dải đại diện cho một vùng con của ảnh, chẳng hạn như đầu, thân trên, thân dưới hoặc chân.

Cách làm này giúp trích xuất đặc trưng cục bộ tốt hơn vì ảnh được phân nhỏ theo vùng, từ đó tăng độ chính xác khi nhận dạng. Các đặc trưng như **texture (kết cấu), color (màu sắc) và shape (hình dạng)** có thể được rút ra từ từng dải, giúp phân biệt rõ hơn các phần khác nhau của một người. Ngoài ra, các dải có chiều cao khác nhau cũng cung cấp thông tin đa tỉ lệ (multi-scale), nhờ đó giải quyết được vấn đề biến đổi kích thước trong ảnh.

Nhiều nghiên cứu đã áp dụng Stripe Segmentation để hỗ trợ việc căn chỉnh (alignment) giữa các modal ở mức cục bộ. Ví dụ:

- Một số nghiên cứu [25, 78] giả định rằng các bộ phận cơ thể được phân bố đều trong ảnh và dùng Stripe Segmentation như một mô hình tham chiếu.
- Ding et al. [70] thiết kế một mạng multi-view non-local để học quan hệ giữa các bộ phận cơ thể, nhằm liên kết tốt hơn giữa bộ phận cơ thể và cụm từ mô tả.
- Li et al. [77] chia dọc ảnh thành nhiều vùng bằng cách kết hợp overlapping slices (cắt chồng lấn) và key-point-based slices (cắt dựa trên điểm đặc trưng).
- Ref. [25] còn nhấn mạnh việc local-local alignment, sử dụng Stripe Segmentation trong mô-đun Bidirectional Fine-Grained Matching (BFM) để ghép nối bộ phận cơ thể với các cụm từ danh từ tương ứng.

Tuy nhiên, nhược điểm của Stripe Segmentation là dễ bị ảnh hưởng bởi sự thay đổi điều kiện. Ví dụ, không phải ảnh nào cũng có đầy đủ cơ thể người; đôi khi phần đầu không nằm ở dải đầu tiên, dẫn đến giảm tính ổn định của phương pháp này.

[25] K. Niu, Y. Huang, W. Ouyang, L. Wang, Improving descriptionbased person re-identification by multi-granularity image-text alignments, IEEE Transactions on Image Processing 29 (2020) 5542–5556.

[70] Z. Ding, C. Ding, Z. Shao, D. Tao, Semantically self-aligned network for text-to-image part-aware person re-identification, arXiv preprint arXiv:2107.12666 (2021).

[77] H. Li, J. Xiao, M. Sun, E. G. Lim, Y. Zhao, Transformer-based language-person search with multiple region slicing, IEEE Transactions on Circuits and Systems for Video Technology 32 (2021) 1624–1633.

[78] Z. Ji, J. Hu, D. Liu, L. Y. Wu, Y. Zhao, Asymmetric cross-scale alignment for text-based person search, IEEE Transactions on Multimedia 25 (2022) 7699–7709.