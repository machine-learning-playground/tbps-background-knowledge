# Định hướng đề tài luận văn

**"Empirical Zero/Few-Shot Text-based Person Search with Prompt-Guided Visual Enhancement"**

## 1. Lý do chọn đề tài & điểm cần cải thiện
- **(Gap 1) Hạn chế trong khả năng tổng quát hóa (open-set / zero-shot):**
    - Các mô hình Text-based Person Search (TBPS) hiện nay như VFE-TPS, RaSa, hay TBPS-CLIP chủ yếu hoạt động trong closed-set setting, tức là chỉ có thể nhận dạng những ID đã xuất hiện trong quá trình huấn luyện.
    - Trong thực tế, người dùng thường mô tả những người chưa từng thấy trước đó. Do đó, cần hướng nghiên cứu zero-shot và few-shot adaptation để cải thiện tính ứng dụng.
- **(Gap 2) Chưa tận dụng tốt ngữ nghĩa sâu từ ngôn ngữ tự nhiên:**
    - Các hệ thống hiện tại chủ yếu sử dụng text encoder cố định (như BERT hoặc CLIP encoder) mà chưa khai thác sức mạnh của LLM-based prompt expansion, semantic paraphrasing hay context enrichment, vốn giúp mô hình hiểu sâu hơn mô tả của người dùng.
- **(Gap 3) Thiếu đa dạng dữ liệu mô tả và hình ảnh:**
    - Các tập dữ liệu như CUHK-PEDES và RSTPReid có số lượng mô tả ngắn, ít biến thể; hình ảnh hạn chế, không đủ đại diện cho các ID chưa thấy.
    - Do đó, cần bổ sung text & image augmentation — bao gồm paraphrase, synonym substitution, LLM-driven captioning, hoặc diffusion-based image synthesis.
- **(Gap 4) Chi phí huấn luyện và suy luận cao:**
    - Các mô hình như RaSa sử dụng pipeline phức tạp gồm segmentation / region alignment hoặc multi-stage training, gây tốn thời gian và tài nguyên.
    - Cần một thiết kế nhẹ hơn, inference nhanh, ưu tiên global feature alignment như trong VFE-TPS nhưng vẫn giữ độ chính xác cao.
- **(Gap 5) Thiếu module alignment linh hoạt & hiệu quả:**
    - Nhiều nghiên cứu vẫn freeze backbone CLIP hoặc fine-tune toàn bộ model, chưa khai thác các cơ chế prompt tuning, cross-modal adapter, giúp cân bằng giữa chi phí tính toán và khả năng thích ứng ngữ nghĩa.

## 2. Mục tiêu luận văn
- Phát triển mô hình **zero-shot / few-shot text-based person search** — có thể truy vấn những ID chưa từng gặp trong training.
- Tăng cường dữ liệu mô tả / đặc trưng bằng cách **augmentation mô tả văn bản** (paraphrase, synonym substitution, LLM prompt expansion) và/hoặc **sinh ảnh giả từ mô tả** (diffusion model) để hỗ trợ alignment.
- Thiết kế module alignment hiệu quả (cross-modal attention, contrastive alignment, prompt tuning) để cải thiện khả năng tổng quát hóa.
- Tối ưu chi phí inference và training so với baseline như RaSa, thông qua thiết kế kiến trúc gọn nhẹ dựa trên global feature alignment và empirical efficiency analysis.
- Đánh giá trên các dataset chuẩn và thiết lập thử nghiệm open-set (loại bỏ một số ID trong training, dùng cho test).

## 3. Phương pháp đề xuất (kế hoạch / pipeline thực nghiệm)
- **(Step 1) Backbone kết hợp:**
    - Dùng CLIP-ViT-B/16 làm nền tảng chính.
    - Kết hợp cơ chế alignment của TBPS-CLIP (R-ITC / Cyclic) [arXiv](https://arxiv.org/html/2308.10045v2) với visual enhancement từ VFE-TPS (TG-MIM + IS-GVFC).
- **(Step 2) Zero-shot / few-shot training setup:**
    - Chia ID thành seen/unseen splits.
    - Áp dụng prompt tuning và meta-learning episodic training để mô hình thích nghi nhanh với ID mới.
- **(Step 3) Text augmentation:**
    - Sử dụng LLM (ví dụ GPT-4/5) để sinh paraphrase, synonym substitution, attribute expansion.
    - Triển khai prompt-based augmentation pipeline tự động hóa sinh mô tả mới cho mỗi ảnh.
- **(Step 4) Image synthesis augmentation:**
    - Dùng text-to-image diffusion model (Stable Diffusion, SDXL) để sinh thêm ảnh pedestrian theo mô tả text (style-constrained generation).
    - Dùng ảnh tổng hợp này trong pre-training để cải thiện alignment.
- **(Step 5) Alignment module hiệu quả:**
    - Thêm Cross-Modal Attention (CMA) + Prompt Tuning Layer giữa encoder text và image.
    - Huấn luyện với loss kết hợp: 𝐿 = 𝐿_𝑅−𝐼𝑇𝐶 + 𝐿_𝑇𝐺−𝑀𝐼𝑀 + 𝐿_𝐼𝑆−𝐺𝑉𝐹𝐶 + 𝜆_1 * 𝐿_𝑃𝑟𝑜𝑚𝑝𝑡𝑅𝑒𝑔
    - Chỉ fine-tune prompt và adapter → giảm chi phí training.
- **(Step 6) Evaluation:**
    - Benchmark trên CUHK-PEDES, ICFG-PEDES với zero-shot split.
    - So sánh với RaSa, TBPS-CLIP, VFE-TPS về: Rank@1, mAP, inference time.

## 4. Đóng góp kỳ vọng
- Giải quyết hạn chế closed-set assumption trong text-based person search.
- Đưa ra phương pháp augmentation mô tả + attention alignment để cải thiện khả năng generalization.
- Có thể có ứng dụng trong thực tế: truy vấn người mới chưa trong dữ liệu huấn luyện.

## 5. Tài liệu khởi điểm / đọc thêm (nên đọc)
- Dong et al., Person Search by Text Attribute Query as Zero-Shot Learning (ICCV 2019). [Xiatian Zhu](https://xiatian-zhu.github.io/papers/DongEtAl_ICCV2019.pdf)
- An Empirical Study of CLIP for Text-based Person Search (2023). [arXiv](https://arxiv.org/html/2308.10045v2)
- Papers on diffusion/data augmentation (DA-Fusion, Effective Data Augmentation With Diffusion Models). [arXiv](https://arxiv.org/html/2302.07944v3)
- Recent empirical studies on synthetic data for TBPR (2025). [arXiv](https://arxiv.org/html/2503.22171v1)