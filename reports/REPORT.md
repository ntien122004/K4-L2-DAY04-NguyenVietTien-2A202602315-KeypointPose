# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Việt Tiến  Nhóm: ______   Ngày: 2026-09-16

## 1. Nhãn của tôi

Số liệu lấy từ `outputs/visibility_report.json` và `reports/visibility_report.md`.

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 27 |
| v=2 / v=1 / v=0 | 320 / 115 / 24 |
| Thời gian trung bình mỗi ảnh | 6.45 phút |

Ba nhóm khớp có `%v=1` cao nhất:

1. `left_ear`: 56% (15/27)
2. `right_ear`: 52% (14/27)
3. `left_eye`, `right_eye`, `left_wrist`, `left_hip` và `right_knee`: 30% (đồng hạng)

Tai có tỷ lệ bị che cao nhất, phù hợp với việc tóc hoặc vật thể che một phần vùng tai. Mắt,
cổ tay, hông và đầu gối cũng có nhiều trường hợp không nhìn rõ hoặc bị che bởi tư thế và vật
thể. Đây là bằng chứng về tình trạng che khuất trong ảnh; chưa đủ để kết luận rằng mọi điểm
đó đều khó xác định vị trí giải phẫu.

## 2. Chấm với gold

Lần chạy hiện tại được xem là **trước rework**; file gold đã được dùng để sinh
`outputs/eval_vs_gold.json`. Chưa có lần chạy sau rework.

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9288 | Chưa chạy |
| OKS@0.50 | 0.9310 | Chưa chạy |
| OKS@0.75 | 0.9310 | Chưa chạy |
| Lỗi `dao_trai_phai` | 0 | Chưa chạy |
| Lỗi `nham_nguoi` | 0 | Chưa chạy |
| Lỗi `xoa_khop_bi_che` | 0 | Chưa chạy |

Ngoài ba nhóm lỗi trên, lần chạy này phát hiện 2 lỗi `thieu_nguoi` và 8 lỗi `lech_nhe`.
Gold có 29 skeleton, trong khi nhãn hiện tại có 27 skeleton.

**Tôi đã sửa gì giữa hai lần chạy**

Chưa có lần chạy sau rework. Hai skeleton còn thiếu đều nằm ở `train_13.jpg`:

- `train_13.jpg`, người thứ 1: cần bổ sung toàn bộ skeleton còn thiếu theo gold.
- `train_13.jpg`, người thứ 2: cần bổ sung toàn bộ skeleton còn thiếu theo gold.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**

Không có lỗi đảo trái/phải trong lần chạy hiện tại. `train_13.jpg` vẫn là ảnh cần rework vì
nhãn hiện tại bỏ sót hai người ở phía sau/bên trái ảnh, không phải vì lỗi đảo trái/phải.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Chưa có bảng visibility của bạn cùng nhóm nên chưa thể tính độ lệch `%v=1`.

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| Chưa có dữ liệu | - | - | - | Chưa thể kết luận |

Luật evidence bổ sung:

- Nếu khớp còn nằm trong khung nhưng bị tóc, quần áo, vật thể hoặc người khác che, vẫn đặt
  điểm tại vị trí giải phẫu ước lượng và dùng `v=1`; chỉ dùng `v=0` khi khớp thật sự nằm ngoài
  mép ảnh và không thể đặt điểm.

## 4. Model

Số liệu lấy từ `outputs/eval_model.json`. Kết quả được đo trên tập test của
`data.yaml`; đây là quan sát của vòng lặp dữ liệu, không phải chất lượng sản phẩm.

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng từ 0.6853 lên 0.6908, chênh +0.0055. Fine-tune giúp model thích
   nghi nhẹ với cách gán và các tư thế trong 20 ảnh của bài, nhưng mức tăng nhỏ cho thấy
   dữ liệu ít. Việc box mAP50-95 giảm đồng thời từ 0.8119 xuống 0.8041 cho thấy không nên
   diễn giải kết quả này là model đã tốt hơn ở mọi mặt.

2. Ở baseline, box mAP50-95 cao hơn pose mAP50-95 là 0.1266 (0.8119 so với 0.6853).
   Sau fine-tune, khoảng cách là 0.1133 (0.8041 so với 0.6908). Model tìm vùng người
   dễ hơn tìm chính xác các khớp, vì hộp bao quát toàn thân ít nhạy với sai lệch từng khớp.

3. Chưa có ảnh dự đoán hoặc bảng lỗi model được lưu từ notebook để xác nhận một trường hợp
   cụ thể. Vì vậy chưa gán loại lỗi slide 43 cho model; việc gán loại lỗi mà không có ảnh
   đối chiếu sẽ không có căn cứ. Ở nhãn người, lỗi đã xác nhận là **thiếu hẳn người** ở
   `train_13.jpg`, không phải lỗi lệch nhẹ hay đảo trái/phải.

4. Chưa có bảng OKS model-vs-nhãn trong artifact đã lưu. Với gold, hai skeleton thiếu ở
   `train_13.jpg` cùng có OKS 0.000 theo `outputs/eval_vs_gold.json`; đây là kết quả của
   nhãn, không phải kết quả dự đoán model.

5. Chưa thể kết luận ảnh gán tệ nhất có trùng ảnh model đoán tệ nhất không vì chưa có bảng
   OKS model-vs-nhãn được lưu. Ảnh gán tệ nhất theo gold hiện là `train_13.jpg` do thiếu
   hai người; cần chạy và lưu phần dự đoán trên tập train để so sánh công bằng.

## 5. Một rule evidence đã dùng

Ở `train_13.jpg`, người trung tâm có phần đầu gối nằm ngoài vùng ảnh nhìn thấy, trong khi
phần thân trên vẫn nằm trong khung. Với một keypoint thật sự vượt qua mép ảnh như đầu gối,
không đặt điểm và chọn `v=0`; không dùng `v=0` chỉ vì điểm bị một vật thể che. Ngược lại,
nếu vị trí khớp còn suy ra được từ phần cơ thể liền kề nhưng bị túi hoặc người khác che, phải
đặt điểm ước lượng và chọn `v=1`. Rule này phân biệt rõ “bị che” với “ra ngoài khung” và có
thể kiểm chứng trực tiếp trên ảnh cùng nhãn.
