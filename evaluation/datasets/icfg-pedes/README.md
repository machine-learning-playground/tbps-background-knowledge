# ICFG-PEDES  Dataset

## 🧾 Tổng quan
**ICFG-PEDES** (Identity-Centric and Fine-Grained Person Description Dataset) là một bộ dữ liệu mô tả người do Đại học Công nghệ Hoa Nam (South China University of Technology) [70] giới thiệu. Đây cũng là một bộ dữ liệu chuẩn (benchmark) mới phục vụ nghiên cứu trong các lĩnh vực mô tả người và truy xuất hình ảnh.

Tương tự như CUHK-PEDES, bộ dữ liệu này bao gồm số lượng lớn mô tả người được ghép cặp với ID hình ảnh tương ứng. Cụ thể, ICFG-PEDES gồm 54.522 ảnh người đi bộ của 4.102 danh tính khác nhau, tất cả đều được thu thập từ cơ sở dữ liệu MSMT17 [76].
- Tập huấn luyện: 34.674 cặp ảnh–văn bản của 3.102 người.
- Tập kiểm thử: 19.848 cặp ảnh–văn bản của 1.000 người.

Mỗi ảnh chỉ đi kèm một mô tả văn bản, với độ dài trung bình 37,2 từ mỗi mô tả.

So với CUHK-PEDES, bộ dữ liệu ICFG-PEDES nhấn mạnh nhiều hơn vào mô tả chi tiết, tinh vi (fine-grained) về con người, đồng thời loại bỏ bớt thông tin không liên quan về hành động và bối cảnh nền. Ngoài ra, nó cũng giải quyết hạn chế về nền ảnh đồng nhất của CUHK-PEDES bằng cách nhấn mạnh hơn vào sự thay đổi về ngoại hình.

Nhờ vậy, ICFG-PEDES trở thành bộ dữ liệu phù hợp cho những bài toán truy xuất hình ảnh và mô tả người ở mức độ khó và thách thức hơn.

Dataset ICFG-PEDES được xây dựng và giới thiệu trong bài báo:

> Zefeng Ding, Changxing Ding, Zhiyin Shao, Dacheng Tao (2021). *Semantically Self-Aligned Network for Text-to-Image Part-aware Person Re-identification*
> Được trình bày tại ACM International Conference on Multimedia 2021 (ACM MM 2021)
> [Paper link](https://arxiv.org/abs/2109.05534)

[70] Z. Ding, C. Ding, Z. Shao, D. Tao, Semantically self-aligned network for text-to-image part-aware person re-identification, arXiv preprint arXiv:2107.12666 (2021).

[76] L. Wei, S. Zhang, W. Gao, Q. Tian, Person transfer gan to bridge domain gap for person re-identification, in: Proceedings of the IEEE conference on computer vision and pattern recognition, 2018, pp. 79–88.

---

## 📊 EDA tập dữ liệu

| Thuộc tính                 | Giá trị                           |
|----------------------------|-----------------------------------|
| Tổng số ảnh                | 54.522                            |
| Tổng số người (Identities) | 4.102                             |
| Tập Train                  | 3.102 IDs                         |
| Tập Test                   | 1000 IDs                          |

---