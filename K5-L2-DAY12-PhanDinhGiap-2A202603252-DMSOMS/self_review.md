# Tự đánh giá — Phan Dinh Giap · G12

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| D03 | `face` + `face13` mắt phải/trái | `flag` | `eye_right = open` nhưng cả 4 điểm `right_eye_*` có cờ `occluded`; `eye_left = unknown` nhưng 4 điểm `left_eye_*` không có cờ. Thuộc tính mắt và cờ điểm không khớp nhau | đã sửa |
| D04 | `face13` hai mắt | `flag` | `eye_left` và `eye_right` đều `unknown` (đốm phản xạ trên kính) nhưng không điểm mắt nào bật `occluded`/`outside` | đã sửa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: gán thuộc tính mắt ở `face` không khớp với cờ điểm mắt ở `face13` (D03, D04).

Vì sao mình mắc: Mình ghi thuộc tính ở `face` mà không đối chiếu với cờ ở `face13`, nên có mắt còn thấy được lại ghi `unknown`, có mắt bị che lại ghi `open`.

Lần sau tránh bằng cách: tăng độ sáng hiển thị trên CVAT rồi xét riêng từng mắt. Thấy rõ thì chọn `open` hoặc `closed`. Không thấy thì chọn `unknown` và bật cờ ngay cho điểm bị che: `occluded` nếu đoán chắc vị trí, `outside` nếu không đoán được. Chạy `cabin_check` để soát lại cảnh báo.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: dòng D03 · `face.eye_left`, mình ghi `unknown`, bạn ghi `open`. Sau khi thảo luận, hai bạn thống nhất bạn đúng và mình đã sửa thành `open` ở `export_final_v2`. Đây cũng là lỗi mình hay mắc ở phần `face`. Mình học được là phải xét riêng từng mắt và đối chiếu với cờ của `face13`: còn thấy khe mắt thì ghi `open`, chỉ ghi `unknown` khi thật sự không phân biệt được (GUIDE §4.5).
