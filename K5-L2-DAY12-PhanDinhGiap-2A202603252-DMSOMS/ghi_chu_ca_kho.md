# Ghi chú ca khó — G12 (task day12-g12)

## Ca có trẻ em ngồi: gán theo coaching update

| Ảnh | Ghế (theo xe) | Thấy gì | Đã làm |
|---|---|---|---|
| S01_rgb / S01_ir | rear_left (phải ảnh) | Trẻ em ngồi trong ghế trẻ em | Gán `child_seat`, `occupied = yes` |
| S03_rgb / S03_ir | rear_right (trái ảnh) | Trẻ em ngồi trong ghế trẻ em | Gán `child_seat`, `occupied = yes` |
| S03_rgb / S03_ir | rear_left (phải ảnh) | Em bé trong ghế em bé (nôi xách) | Gán `infant_seat`, `facing = rear` |
| S07_rgb / S07_ir | rear_left (phải ảnh) | Trẻ em ngồi trong ghế trẻ em | Gán `child_seat`, `occupied = yes` |

## Ghế trống (không vẽ box)

| Cảnh | Ghế trống |
|---|---|
| S01 | 0 (rear_middle có vật) |
| S02 | 1: rear_middle |
| S03 | 0 (rear_middle có hộp; hai ghế bên có trẻ em) |
| S04 | 1: rear_middle |
| S05 | 1: rear_middle |
| S06 | 2: rear_middle, rear_left |
| S07 | 1: rear_middle |
| S08 | 2: rear_middle, rear_left |

## Lựa chọn unknown / other

- S01: vật hình trụ tối ở rear_middle, không rõ cốc hay bình → `type = other`.
- S02: `infant_seat` rear_right: mui che ở phía lưng ghế, lòng nôi quay về camera (phía trước xe) → `facing = front`. Đây là ca dễ cãi, nên đối chiếu khi review chéo.
- Mặt người SVIRO (S01, S07, S08): chỉ vẽ box `face`, thuộc tính mắt để `unknown`, không vẽ face13.
- Mọi khung D##: `light = unknown` (không có dấu hiệu ngày/đêm trên NIR). D01, D02 ghi `gaze_zone = center`, D04 ghi `cluster`. D03, D05, D06 ghi `other` vì đầu ngửa hoặc nghiêng, nhìn vào tay hoặc điện thoại, không thuộc vùng nào trong danh sách.
- D02, D04, D06: export cuối có cả box `face` và `face13`. D02, D04 không điểm nào có cờ; D06 có 5 điểm `occluded`: 4 miệng, 1 mắt phải.
- D04: tay trái đang kéo chốt dây về khóa → đang thắt dở → `seatbelt = unknown`.
- D03: tay trái chạm gần vành vô lăng nhưng không chắc → `on_wheel = unknown`.
- D02: vật trên tay là chai nhựa, không vẽ (DMS không có object). Tài xế đeo kính → `glasses = clear`.
- D06 (ca khó, còn sót chưa sửa): khó xác định trạng thái mắt. `export_final_v2` ghi `eye_left = unknown` nhưng 4 điểm `left_eye_*` chưa có cờ `occluded`/`outside`, trong khi `eye_right = open` và `right_eye_upper` có cờ `occluded`. `cabin_check` còn 1 cảnh báo ở ảnh này, cần xem lại từng mắt theo GUIDE §4.5–4.6.
