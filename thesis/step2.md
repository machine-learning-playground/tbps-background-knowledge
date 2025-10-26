# (Step 2) Zero-shot / few-shot
Nếu chỉ dừng lại ở việc **kết hợp TBPS-CLIP và VFE-TPS**, thì đó **chưa đủ “unique contribution”** cho luận văn thạc sĩ, vì nó vẫn nằm trong phạm vi “engineering fusion” (lấy loss A + loss B).

→ Để nâng cấp thành **một đề tài có tính nghiên cứu thực nghiệm và đóng góp riêng**, bạn cần **làm rõ cơ chế zero/few-shot adaptation, cách tận dụng prompting / augmentation / lightweight adaptation**, chứ không chỉ mix loss.

## 🎯 Mục tiêu: làm rõ yếu tố unique contribution về Zero/Few-Shot Generalization
Dưới đây là 3 hướng mà bạn có thể **chứng minh rõ ràng tính mới** — đều dựa trên nền TBPS-CLIP + VFE-TPS nhưng mở rộng đúng hướng “Empirical Zero/Few-Shot”.

### 🔹 (1) Zero/Few-Shot thông qua Prompt-Guided Adaptation (unique điểm 1)
- **Ý tưởng cốt lõi:** CLIP backbone đã có khả năng zero-shot (từ pretrain). Thay vì fine-tune toàn bộ, bạn chỉ **thêm một module “prompt adapter”** (learnable textual/visual prompt) được huấn luyện để điều chỉnh không gian embedding phù hợp với task TBPS.
- **Cụ thể:**
    - **Text side:** thêm learnable textual prompts [P₁, P₂, …, Pₖ] nối trước/sau caption, ví dụ "person with" [P₁] "red jacket" [P₂] "in the street".
    Chỉ update [P], freeze CLIP encoder → giảm chi phí train/inference.
    - **Visual side:** thêm Visual Prompt Tuning (VPT): một số learnable tokens chèn vào ViT input (giống Prefix-tuning).
    - **Cách học:** supervision qua contrastive (ITC/R-ITC) + TG-MIM (để giữ grounding).
    => Cho phép **quick adaptation** sang unseen ID descriptions (zero-shot) hoặc với vài sample (few-shot).
- **Giá trị:**
    - **Giảm training cost** (freeze backbone).
    - **Zero/Few-shot capability:** vì backbone vẫn giữ knowledge từ pretrain CLIP, prompt học ngữ cảnh mới nhanh.
    - Đây là điểm “unique” dễ mô tả, có thể chứng minh bằng ablation.

### 🔹 (2) Data Augmentation hướng ngữ nghĩa (unique điểm 2)
**Không chỉ paraphrase text**, mà bạn cho phép **semantic interpolation** giữa captions của cùng ID.
- **Ví dụ:**
    - 2 mô tả của cùng ID:
    - T₁ = “man wearing red jacket”,
    - T₂ = “male with a crimson coat”
    - → sinh vector trung gian T_mix = α·E(T₁) + (1−α)·E(T₂)
    - → xem như 1 caption “latent paraphrase” để augment training contrastive pairs.
- **Giá trị:**
    - Tạo “semantic continuum” trong text space → model học khái niệm thay vì caption cụ thể.
    - Giúp zero-shot generalization vì model không phụ thuộc vào phrasing cụ thể.
- **Triển khai:**
    - Rất nhẹ (không cần LLM sinh thêm caption).
    - Có thể kết hợp với CLIP tokenizer (cosine blending trong embedding space).

### 🔹 (3) Meta-Learning Episodic Training (unique điểm 3)
**Mô phỏng zero/few-shot scenario trong training.**
- Cụ thể:
    - Chia tập train thành các episodic tasks: mỗi episode chỉ gồm một số ID (support set + query set).
    - Trong mỗi episode: model học alignment text↔image của những ID “nhỏ”, rồi cập nhật qua meta-gradient (MAML-like hoặc ProtoNet-style).
    - Khi test trên unseen ID, mô hình đã học cách “thích nghi nhanh” chỉ từ vài ví dụ (few-shot).
- Giá trị:
    - Đưa ra cách huấn luyện mô phỏng đúng zero/few-shot setting, thay vì chỉ nói “test unseen”.
    - Có thể dùng framework như Higher (PyTorch) để cài đặt nhanh.

## 💡 Tóm tắt cách kết hợp (để có đóng góp độc lập thật sự)
| Thành phần                                 | Từ đâu                               | Vai trò chính                            | Đóng góp mới                           |
|--------------------------------------------|--------------------------------------|------------------------------------------|----------------------------------------|
| **TBPS-CLIP**                              | Baseline mạnh (contrastive, SS, MVS) | Global alignment, backbone               | Nền để freeze cho adaptation           |
| **TG-MIM (VFE-TPS)**                       | VFE-TPS                              | Local text-guided reconstruction         | Giữ grounding khi prompt tuning        |
| **IS-GVFC (VFE-TPS)**                      | VFE-TPS                              | Intra-visual calibration                 | Giữ cluster ID ổn định trong few-shot  |
| 🔸 **Prompt-Guided Adaptation**            | (đề xuất mới)                        | Lightweight fine-tune cho zero/few-shot  | ✅ Unique                             |
| 🔸 **Semantic Interpolation Augmentation** | (đề xuất mới)                        | Làm mịn không gian ngữ nghĩa             | ✅ Unique                             |
| 🔸 **Meta-Learning Episodic Training**     | (đề xuất mới)                        | Huấn luyện mô phỏng zero/few-shot        | ✅ Unique                             |

## 🔬 Bạn có thể mô tả khung thực nghiệm như sau:
- Giai đoạn 1: pretrain base model (TBPS-CLIP + IS-GVFC + TG-MIM).
- Giai đoạn 2: freeze backbone, train Prompt Adapter + semantic augmentation.
- Giai đoạn 3: episodic fine-tune (meta-learning) cho few-shot adaptation.
- Giai đoạn 4: evaluate zero-shot (unseen IDs) và few-shot (k=1, 5, 10) trên CUHK-PEDES / RSTPReid.

## 🔎 Gợi ý tên cho đóng góp chính:
**Prompt-Adaptive Cross-Modal Alignment for Zero/Few-Shot Text-based Person Search (PACA-TPS)**

hoặc ngắn hơn: **Prompt-Enhanced TBPS**