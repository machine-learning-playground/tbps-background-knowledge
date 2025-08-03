# RSTP-Reid Dataset

## 🧾 Tổng quan
**RSTP-Reid** (Real Scenario Text‑based Person Re‑identification) được xây dựng dựa trên tập dữ liệu MSMT17 để phục vụ các kịch bản thực tế phức tạp hơn.

Dataset RSTP‑Reid được xây dựng và giới thiệu trong bài báo:

> Zhu A., Wang Z., Li Y., et al. (2021). *DSSL: Deep Surroundings‑person Separation Learning for Text‑based Person Retrieval.*
> Được trình bày tại ACM International Conference on Multimedia 2021 
> [Paper link](https://arxiv.org/abs/2109.05534)

---

## 📊 EDA tập dữ liệu

| Thuộc tính                 | Giá trị                           |
|----------------------------|-----------------------------------|
| Tổng số ảnh                | 20.505                            |
| Tổng số người (Identities) | 4.101                             |
| Tổng số mô tả              | ≈ 41.010 (mỗi ảnh 2 mô tả)        |
| Số từ tối thiểu mô tả      | ≥ 23 từ                           |
| Tập Train                  | 3.701 IDs                         |
| Tập Val                    | 200 IDs                           |
| Tập Test                   | 200 IDs                           |


Dataset bao gồm:
- **20.505 ảnh**, thuộc về **4.101 danh tính (IDs)**, chụp từ **15 camera** khác nhau trong cả môi trường trong nhà và ngoài trời  
- Mỗi danh tính có **5 ảnh**, với bối cảnh, góc nhìn và nền phức tạp tạo ra tính đa dạng cao hơn so với CUHK‑PEDES 
- Mỗi ảnh được gắn **2 mô tả văn bản**, mỗi mô tả dài **không dưới 23 từ**

---

## 📁 Cấu trúc thư mục RSTP‑Reid

```bash
RSTP‑Reid/
├── imgs                 # Thư mục chứa toàn bộ ảnh người
├── data_captions.json   # File chú thích (annotation) ở định dạng JSON
```

Mỗi phần tử (entry) trong file JSON này chứa các thông tin sau:
- **id**: mã định danh (identity) của người trong ảnh.
- **img_path**: đường dẫn tới ảnh người (ví dụ: `0000_c14_0031.jpg`).
- **captions**: danh sách các mô tả bằng ngôn ngữ tự nhiên (thường có 2 câu mô tả cho mỗi ảnh).
- **split**: cho biết mẫu dữ liệu thuộc tập nào (`train`, `val`, hoặc `test`).

### Ví dụ một entry trong `data_captions.json`
```json
{
  "id": 0,
  "img_path": "0000_c14_0031.jpg",
  "captions": [
    "The man is wearing a grey jacket and a blue shirt.His trousers are black and his shoes are brown.He is walking.",
    "The man is a strong man who is in a grey jacket.His shirt is blue and his trousers is black."
  ],
  "split": "train"
}
```
---
