# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.11 khớp có v > 0 mỗi người
- Tổng: v=2 320 | v=1 115 | v=0 24

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 4 | 0 | 15% |
| 1 | left_eye | 19 | 8 | 0 | 30% |
| 2 | right_eye | 19 | 8 | 0 | 30% |
| 3 | left_ear | 12 | 15 | 0 | 56% |
| 4 | right_ear | 13 | 14 | 0 | 52% |
| 5 | left_shoulder | 26 | 1 | 0 | 4% |
| 6 | right_shoulder | 26 | 1 | 0 | 4% |
| 7 | left_elbow | 23 | 4 | 0 | 15% |
| 8 | right_elbow | 23 | 4 | 0 | 15% |
| 9 | left_wrist | 19 | 8 | 0 | 30% |
| 10 | right_wrist | 20 | 7 | 0 | 26% |
| 11 | left_hip | 19 | 8 | 0 | 30% |
| 12 | right_hip | 20 | 7 | 0 | 26% |
| 13 | left_knee | 17 | 6 | 4 | 22% |
| 14 | right_knee | 15 | 8 | 4 | 30% |
| 15 | left_ankle | 13 | 6 | 8 | 22% |
| 16 | right_ankle | 13 | 6 | 8 | 22% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
