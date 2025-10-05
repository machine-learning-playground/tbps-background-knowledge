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

# Q/A Tình hình nghiên cứu hiện tại:
Ngắn gọn: **Có — nhưng vẫn còn ít và còn nhiều khoảng trống nghiên cứu.** Dưới đây là tóm tắt với bằng chứng (trích dẫn), điểm mạnh/yếu của những nghiên cứu hiện có, và đề xuất hướng mới cụ thể cho luận văn của bạn (kết hợp prompting + augmentation + zero/few-shot).

## 1. Những công trình đã làm liên quan (ví dụ, trích dẫn)
- **Zero-shot / attribute-based (sớm):** có hướng dùng attribute text queries như một dạng zero-shot person search (ICCV 2019: Person Search by Text Attribute Query as Zero-Shot Learning). 
[Xiatian Zhu](https://xiatian-zhu.github.io/papers/DongEtAl_ICCV2019.pdf)
- **CLIP / V-L backbone áp dụng cho TBPS:** nhiều công trình khảo sát và ứng dụng CLIP/vision-language models cho text-based person search, chỉ ra khả năng zero-shot / transfer tốt nhưng còn hạn chế trên domain pedestrian. (ví dụ: An Empirical Study of CLIP for Text-based Person Search, 2023). [arXiv](https://arxiv.org/html/2308.10045v2)
- **Synthetic data / generation for TBPR:** gần đây có một số nghiên cứu thăm dò dùng ảnh sinh (diffusion/GAN) để bổ sung dữ liệu cho tác vụ liên quan; có cả các bài tổng quan/nhận xét gần đây nghiên cứu tính hữu dụng của dữ liệu tổng hợp cho text-based person retrieval. (ví dụ: An Empirical Study of Validating Synthetic Data for Text-Based Person Retrieval, 2025; DA-Fusion style data augmentation papers). [arXiv](https://arxiv.org/html/2503.22171v1), [arXiv +1](https://arxiv.org/html/2302.07944v3)
- **Few-shot / continual few-shot ReID (ảnh↔ảnh):** lĩnh vực few-shot person Re-ID (image-based) có nhiều công trình; chuyển trực tiếp các kỹ thuật này sang text-to-image vẫn là thách thức và ít được khai thác. (ví dụ: CFReID, Few-shot ReID papers). 
[arXiv](https://arxiv.org/abs/2503.18469), [arXiv +1](https://arxiv.org/abs/1806.09613)

Kết luận tài liệu: có các mảnh rời rạc — attribute zero-shot, CLIP-based transfer, synthetic augmentation, few-shot ReID — nhưng ít bài trực tiếp kết hợp cả ba yếu tố: zero/few-shot text-based person search + text prompting + text→image synthetic augmentation.

## 2. Khoảng trống / cơ hội (điểm để nhắm tới)
- **Open-set / zero-shot TBPS thực tế:** nhiều benchmark giả định closed-set; thiếu work hệ thống trên unseen IDs với mô tả tự do. (gap) [Xiatian Zhu](https://xiatian-zhu.github.io/papers/DongEtAl_ICCV2019.pdf)
- **Sử dụng LLM/Prompting để mở rộng mô tả (text augmentation):** chưa có nhiều công trình dùng prompting/LLM để tạo paraphrase/attribute expansions phục vụ TBPS. (opportunity) [ACL Anthology](https://aclanthology.org/2024.emnlp-main.1186.pdf)
- **Kết hợp ảnh sinh (diffusion) đặc thù pedestrian + text prompts:** có nghiên cứu về dùng diffusion cho augmentation nói chung, nhưng ứng dụng có kiểm chứng trong TBPR còn hạn chế; cần đánh giá chặt chẽ (benefit vs. harm). [arXiv](https://arxiv.org/html/2302.07944v3), [arXiv +1](https://arxiv.org/html/2503.22171v1) 
- **Few-shot adaptation trên TBPS:** kỹ thuật few-shot/ meta-learning đã xuất hiện nhiều cho image-ReID, nhưng chưa đủ cho text→image; idea: meta-learning + prompt tuning + synthetic samples. [arXiv](https://arxiv.org/abs/1806.09613), [arXiv +1](https://arxiv.org/abs/2503.18469) 

## 3. Đề xuất đề tài luận văn (cụ thể, mới so với literature)

**“Zero-shot / Few-shot Text-based Person Search via Prompted Text Augmentation and Diffusion-guided Image Augmentation”**

**Ý chính:**
- Dùng pretrained V-L backbones (CLIP/BLIP/ALBEF) làm nền.
- Text side: dùng LLM prompt engineering để paraphrase, expand attributes, tạo nhiều biến thể mô tả (prompted augmentation).
- Image side: dùng text→image diffusion models (kỹ thuật điều khiển prompt / ControlNet) để từ các mô tả sinh ảnh pedestrian đa dạng, hoặc fine-tune diffusion trên domain pedestrian để sinh variants.
- Training: mix thật + sinh + paraphrase để huấn luyện hoặc prompt-tune model, áp dụng contrastive + cross-modal losses; thiết kế chế độ few-shot adaptation (meta-learning / prompt tuning) để nhanh chóng thích ứng với unseen IDs.
- Evaluation: thiết lập open-set splits (để đánh giá zero/few-shot), báo cáo Rank@K, mAP trên CUHK-PEDES, ICFG-PEDES, RSTPReid; so sánh: baseline (no augmentation), text-aug only, image-synth only, both. (tham khảo cách các paper làm splits/metrics). 
arXiv
+1

**Điểm mới:** kết hợp có hệ thống giữa prompted text augmentation và diffusion image augmentation cho TBPS under zero/few-shot — hiện literature có các mảnh rời rạc nhưng chưa đầy đủ hay đánh giá kỹ lưỡng.

## 4. Khuyến nghị thực nghiệm & rủi ro
- **Backbone:** CLIP/BLIP or ALBEF; thử cả fine-tune vs prompt-tuning. [arXiv](https://arxiv.org/html/2308.10045v2)
- **Diffusion:** fine-tune tiny checkpoint trên pedestrian crop (cẩn trọng với privacy), hoặc sử dụng ControlNet conditioned on pose/keypoints để giữ cấu trúc. (tham khảo DA-Fusion, diffusion augmentation). [arXiv](https://arxiv.org/html/2302.07944v3)
- **Validation:** kiểm tra kỹ chất lượng ảnh sinh (không tạo leak / bias), tiến hành ablation (text vs image augmentation). Có paper báo rằng ảnh sinh không luôn giúp — cần kiểm chứng. [review](https://research-explorer.ista.ac.at/download/14949/14950/Burg_et_al_2023_Image_retrieval_outperforms.pdf)
- **Rủi ro:** đạo đức/riêng tư khi sinh ảnh người; tài nguyên tính toán lớn (diffusion fine-tuning).

## 5. Tài liệu khởi điểm / đọc thêm (nên đọc)
- Dong et al., Person Search by Text Attribute Query as Zero-Shot Learning (ICCV 2019). [Xiatian Zhu](https://xiatian-zhu.github.io/papers/DongEtAl_ICCV2019.pdf)
- An Empirical Study of CLIP for Text-based Person Search (2023). [arXiv](https://arxiv.org/html/2308.10045v2)
- Papers on diffusion/data augmentation (DA-Fusion, Effective Data Augmentation With Diffusion Models). [arXiv](https://arxiv.org/html/2302.07944v3)
- Recent empirical studies on synthetic data for TBPR (2025). [arXiv](https://arxiv.org/html/2503.22171v1)