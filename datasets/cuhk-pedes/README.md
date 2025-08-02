# CUHK-PEDES Dataset

## 🧾 Tổng quan
CUHK-PEDES là một trong những bộ dữ liệu được sử dụng rộng rãi nhất cho bài toán **Text-Based Person Search (TBPS)**.

Bộ dữ liệu này được giới thiệu trong bài báo:

> Li, D., Xu, Z., Huang, Z., & Sebe, N. (2017). *Person Search with Natural Language Description*. CVPR 2017.  
> [Paper link](https://arxiv.org/pdf/1702.05729)

---

## 📊 EDA tập dữ liệu

| Thuộc tính                 | Giá trị                  |
|----------------------------|--------------------------|
| Tổng số ảnh                | ~40,206                  |
| Tổng số người (Identities) | 13,003                   |
| Tổng số mô tả              | 80,440 (mỗi ảnh 2 mô tả) |
| Tập Train                  | ~11.0k IDs (34,054 ảnh)  |
| Tập Val                    | 1.0k IDs (3,078 ảnh)     |
| Tập Test                   | 1.0k IDs (3,074 ảnh)     |

Mỗi ảnh được ghép với **2 mô tả bằng ngôn ngữ tự nhiên**, được viết bởi hai người khác nhau trên nền tảng Amazon Mechanical Turk.

---

## 📁 Cấu trúc thư mục CUHK-PEDES

```bash
CUHK-PEDES/
├── imgs               # Thư mục chứa toàn bộ ảnh người
|    ├── cam_a
|    ├── cam_b
|    ├── ...
├── caption_all.json   # File chú thích (annotation) ở định dạng JSON
```

File [`reid_raw.json`](https://drive.google.com/file/d/0B-GOvBat1maObWN1eDV6cFNYV2M/view?usp=sharing&resourcekey=0-CStaTaSQeHN60VYIjlVTAg) (dùng trong RaSa làm annotation thay cho `caption_all.json`)
Mỗi phần tử (entry) trong file JSON này chứa các thông tin sau:
- **split**: cho biết mẫu dữ liệu thuộc tập nào (`train`, `val`, hoặc `test`).
- **captions**: danh sách các mô tả bằng ngôn ngữ tự nhiên (thường có 2 câu mô tả cho mỗi ảnh).
- **file_path**: đường dẫn tới ảnh người (ví dụ: `imgs/000001.jpg`).
- **processed_tokens**: phiên bản đã được tách từ (tokenized) của phần `captions`, để tiện sử dụng trong mô hình.
- **id**: mã định danh (identity) của người trong ảnh.

### Ví dụ một entry trong `reid_raw.json`
```json
{
  "split": "train",
  "captions": [
    "A pedestrian with dark hair is wearing red and white shoes, a black hooded sweatshirt, and black pants.",
    "The person has short black hair and is wearing black pants, a long sleeve black top, and red sneakers."
  ],
  "file_path": "CUHK01/0363004.png",
  "processed_tokens": [
    ["a", "pedestrian", "with", "dark", "hair", "is", "wearing", "red", "and", "white", "shoes", "a", "black", "hooded", "sweatshirt", "and", "black", "pants"],
    ["the", "person", "has", "short", "black", "hair", "and", "is", "wearing", "black", "pants", "a", "long", "sleeve", "black", "top", "and", "red", "sneakers"]
  ],
  "id": 1
}
```
---
