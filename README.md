# Đề tài luận văn gợi ý

**"Zero-shot / Few-shot Text-based Person Search with Data Augmentation & Prompting"**

## 1. Lý do chọn đề tài
- Các challenge như **Text-based Person Anomaly Search** cho thấy có nhu cầu xử lý truy vấn người mới, mô tả mới ngoài tập huấn luyện (open-set).
- Nhiều mô hình hiện tại yêu cầu mỗi ID phải xuất hiện trong tập huấn luyện, hạn chế ứng dụng thực tế.
- Sự kết hợp prompt learning / textual augmentation / synthetic image generation là xu hướng mới, phù hợp cho tin học thị giác + ngôn ngữ.

## 2. Mục tiêu luận văn
- Phát triển mô hình **zero-shot / few-shot text-based person search** — có thể truy vấn những ID chưa từng gặp trong training.
- Tăng cường dữ liệu mô tả / đặc trưng bằng cách **augmentation mô tả văn bản** (paraphrase, synonym substitution, LLM prompt expansion) và/hoặc **sinh ảnh giả từ mô tả** (diffusion model) để hỗ trợ alignment.
- Thiết kế module alignment hiệu quả (cross-modal attention, contrastive alignment, prompt tuning) để cải thiện khả năng tổng quát hóa.
- Đánh giá trên các dataset chuẩn và thiết lập thử nghiệm open-set (loại bỏ một số ID trong training, dùng cho test).

## 3. Phương pháp đề xuất
- **Backbone:** dùng mô hình vision-language pretrained (CLIP, BLIP, Florence...) làm nền tảng.
- **Mô-đun prompt / augmentation:**
    - Với mô tả: paraphrase, synonym, thêm thông tin ngữ nghĩa mở rộng.
    - Với ảnh: sinh ảnh giả từ mô tả (tùy khả năng) hoặc augmentation hình ảnh thông thường.
- **Alignment:**
    - Contrastive loss + cross-modal attention.
    - Có thể thêm soft / hard attention hỗn hợp hoặc gating dynamic giữa các loại attention.
- **Zero-shot / few-shot:**
    - Sử dụng meta-learning (MAML, ProtoNet) để mô hình học cách tìm ID mới nhanh.
    - Hoặc sử dụng prompt tuning / adapter tuning để mở rộng ID mới.

## 4. Thử nghiệm & đánh giá
- Dataset chuẩn: CUHK-PEDES, ICFG-PEDES, RSTPReid.
- Chia lại splits open-set: loại bỏ (ví dụ) 20 % ID ra khỏi training, dùng chúng làm unseen.
- Metrics: Rank@K, mAP, đặc biệt quan tâm performance trên unseen ID (zero-shot).
- So sánh:
    - Mô hình baseline (không augmentation, no prompt).
    - Mô hình với augmentation mô tả.
    - Mô hình với sinh ảnh giả hỗ trợ.
    - Mô hình kết hợp cả hai.

## 5. Đóng góp kỳ vọng
- Giải quyết hạn chế closed-set assumption trong text-based person search.
- Đưa ra phương pháp augmentation mô tả + attention alignment để cải thiện khả năng generalization.
- Có thể có ứng dụng trong thực tế: truy vấn người mới chưa trong dữ liệu huấn luyện.