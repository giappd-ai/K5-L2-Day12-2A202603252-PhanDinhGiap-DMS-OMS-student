# Danh sách lớp v0 — Phan Dinh Giap · G12

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face.eye_left` / `face.eye_right` | open / closed / unknown | Phát hiện buồn ngủ: mắt nhắm kéo dài nhiều khung liên tiếp (PERCLOS, microsleep) | Có; trừ khi đốm phản xạ LED trên tròng kính che mắt (G12 D04) → `unknown` |
| 2 | `face.gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung: không nhìn `road` quá ngưỡng thời gian (ví dụ > 2 s); phân biệt liếc gương (hợp lệ) với nhìn đùi/màn hình | Có (hướng đầu); hướng mắt khó khi kính phản xạ |
| 3 | `phone` (`state`) | held / mounted / lap | Cảnh báo dùng điện thoại khi lái; `mounted` (dẫn đường trên giá) ít rủi ro hơn `held` | Có khi thấy cạnh/góc/màn hình; vật tối áp tai thì không chắc → không vẽ |
| 4 | `hand.on_wheel` (+ `whose`, `side`) | yes / no / unknown | Đo mức sẵn sàng tiếp quản khi xe tự lái trả quyền điều khiển (hands-off detection) | Có (hình dạng bàn tay, vành vô lăng) |
| 5 | `face.glasses` | none / clear / sun / unknown | Biết khi nào kết quả mắt kém tin cậy (kính phản xạ); chọn ngưỡng/model riêng cho người đeo kính | Một phần: NIR xuyên qua nhiều loại kính râm, khó phân biệt clear với sun (G12 D06) |
| 6 | `face13` (13 điểm mặt) | 13 điểm + cờ occluded/outside | Tính độ mở mắt (EAR), độ mở miệng (ngáp), ước lượng tư thế đầu cho gaze | Có trên mặt ≥ 60 px; điểm bị che phải có cờ |
| 7 | `person.seatbelt` (tài xế) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây, phát hiện thắt sai (dây dưới nách, sau lưng) | Khó: dây tối trên áo tối, đang thắt dở → `unknown` |
| 8 | `frame_meta.camera_blocked` | yes / no | Báo hệ thống DMS mất tầm nhìn (tay/vật che camera) thay vì báo sai "tài xế ổn" | Có |

## OMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` + `seat` | driver / front_passenger / rear_left / rear_middle / rear_right | Bản đồ chỗ ngồi: đếm người, nhắc dây an toàn đúng ghế, bật/tắt túi khí theo ghế | Có (hình dạng cơ thể) |
| 2 | `person.seatbelt` theo ghế | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây cho hàng ghế sau | Khó; SVIRO không có ai thắt dây nên chưa dạy được `worn_correct` |
| 3 | `child_seat` (`seat`, `occupied`) | occupied: no / yes / unknown | Phát hiện ghế trẻ em để điều chỉnh túi khí và nhắc quên trẻ trong xe | Có (hình dạng); lòng ghế tối → `unknown` |
| 4 | `infant_seat` (`seat`, `facing`) | facing: rear / front | Ghế em bé quay về sau đặt ở ghế trước → bắt buộc tắt túi khí; nhắc quên em bé | Có hình dạng; `facing` khó khi mui che (G12 S02) |
| 5 | `object` (`seat`, `type`) | bag / box / cup / other | Nhắc quên đồ khi rời xe; phân biệt vật với người để không nhắc dây hay kích túi khí sai | Có hình dạng; không dùng màu để nhận loại |
| 6 | Ghế trống (không box, đếm trong ghi chú) | số ghế trống / tổng ghế | Xác nhận ghế không có người để tắt nhắc dây và túi khí ghế đó | Có |
| 7 | `face` (box, OMS) | box | Xác nhận đối tượng là người thật (có mặt), hỗ trợ ước lượng vóc dáng người lớn hay trẻ | Có |
| 8 | `frame_meta.modality` / `light` | rgb / ir · day / night / unknown | Chọn model/ngưỡng theo loại ảnh; phân tích lỗi theo điều kiện sáng | — (siêu dữ liệu) |

## 2 câu hỏi leo thang

1. Câu hỏi: Trẻ em đang ngồi trong ghế trẻ em (hoặc em bé trong ghế em bé) thì gán thế nào? Guideline v1.0 chỉ có `child_seat.occupied` = no/unknown (không có `yes`), không có lớp `child`, và dặn "thấy hình giống trẻ em thì báo coach, không gán". Nếu không gán gì, model OMS sẽ học rằng ghế đó trống, đúng ca nguy hiểm nhất (quên trẻ trong xe, túi khí).
   Khung ví dụ: G12 S01 (rear_left), G12 S03 (rear_right; và em bé trong nôi ở rear_left), G12 S07 (rear_left).
   Bạn tạm gán thế nào và vì sao: gán `child_seat`, `occupied = yes` theo coaching update để model biết ghế có trẻ. Nôi S03 gán `infant_seat`, `facing = rear`; lớp này chưa có thuộc tính `occupied`.

2. Câu hỏi: Ghế em bé có mui che lòng nôi thì xác định `facing` dựa vào đâu (hướng mui, hướng tay cầm, hay hướng lòng nôi so với xe)? Trả lời sai trường này làm sai quyết định tắt túi khí.
   Khung ví dụ: G12 S02 (`infant_seat` ở rear_right).
   Bạn tạm gán thế nào và vì sao: `facing = front` vì mui ở phía lưng ghế và lòng nôi quay về camera (tức là về phía trước xe). Đây là ca đã đánh dấu để đối chiếu khi review chéo.
