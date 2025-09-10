# CUHK-PEDES Dataset

## 🧾 Tổng quan
**CUHK-PEDES** là bộ dữ liệu đầu tiên, được giới thiệu bởi Đại học Trung Văn Hồng Kông (The Chinese University of Hong Kong) [7], nhằm phục vụ việc huấn luyện và đánh giá các tác vụ mô tả người cũng như truy xuất hình ảnh trong thị giác máy tính **Text-Based Person Search (TBPS)**.
Bộ dữ liệu này được xây dựng bằng cách tổng hợp ảnh người từ năm bộ dữ liệu Re-ID nổi tiếng: CUHK03 [72], Market-1501 [47], SSM [73], VIPER [74], và CUHK01 [75], sau đó được gán nhãn bằng các mô tả ngôn ngữ tự nhiên do cộng tác viên trên nền tảng Amazon Mechanical Turk (AMT) thực hiện.

Bộ dữ liệu bao gồm hơn 40.206 ảnh cùng 80.412 mô tả bằng văn bản, mỗi mô tả có ít nhất 23 từ, tương ứng với 13.003 danh tính khác nhau.
- Tập huấn luyện: 34.054 ảnh, 11.003 danh tính, 68.108 mô tả.
- Tập kiểm định (validation): 3.078 ảnh, 1.000 danh tính, 6.158 mô tả.
- Tập kiểm thử (test): 3.074 ảnh, 1.000 danh tính, 6.156 mô tả.

Các cá nhân trong bộ dữ liệu được chụp tại đường phố và không gian công cộng ở khu vực Hồng Kông.
CUHK-PEDES cung cấp mô tả đa dạng về con người, bao gồm ngoại hình, hành động, tư thế, và tương tác, với mỗi mô tả được liên kết trực tiếp tới ID của hình ảnh tương ứng.

Việc phát hành CUHK-PEDES đánh dấu một bước tiến quan trọng, thúc đẩy nghiên cứu liên ngành giữa thị giác máy tính và xử lý ngôn ngữ tự nhiên, đồng thời mang lại bộ dữ liệu chuẩn đầu tiên cho các nghiên cứu về mô tả người và truy xuất hình ảnh.

Bộ dữ liệu này được giới thiệu trong bài báo:

> Li, D., Xu, Z., Huang, Z., & Sebe, N. (2017). *Person Search with Natural Language Description*. CVPR 2017.  
> [Paper link](https://arxiv.org/pdf/1702.05729)


[7] S. Li, T. Xiao, H. Li, B. Zhou, D. Yue, X. Wang, Person search with natural language description, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2017, pp. 1970–1979.
[72] W. Li, R. Zhao, T. Xiao, X. Wang, Deepreid: Deep filter pairing neural network for person re-identification, in: Proceedings of the 2014 IEEE Conference on Computer Vision and Pattern Recognition, 2014, pp. 152–159.
[47] L. Zheng, L. Shen, L. Tian, S. Wang, J. Bu, Q. Tian, Person reidentification meets image search, arXiv preprint arXiv:1502.02171 (2015).
[73] T. Xiao, S. Li, B. Wang, L. Lin, X. Wang, Joint detection and identification feature learning for person search, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2017, pp. 3415–3424.
[74] D. Gray, S. Brennan, H. Tao, Evaluating appearance models for recognition, reacquisition, and tracking, in: Proc. IEEE international workshop on performance evaluation for tracking and surveillance (PETS), volume 3, 2007, pp. 1–7.
[75] W. Li, R. Zhao, X. Wang, Human reidentification with transferred metric learning, in: Computer Vision–ACCV 2012: 11th Asian Conference on Computer Vision, Daejeon, Korea, November 5-9, 2012, Revised Selected Papers, Part I 11, Springer, 2013, pp. 31–44.

---

## 📊 EDA tập dữ liệu

| Thuộc tính                 | Giá trị                   |
|----------------------------|---------------------------|
| Tổng số ảnh                | ~40,206                   |
| Tổng số người (Identities) | 13,003                    |
| Tổng số mô tả              | ~80,440 (mỗi ảnh 2 mô tả) |
| Tập Train                  | ~11.0k IDs (34,054 ảnh)   |
| Tập Val                    | 1.0k IDs (3,078 ảnh)      |
| Tập Test                   | 1.0k IDs (3,074 ảnh)      |

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
