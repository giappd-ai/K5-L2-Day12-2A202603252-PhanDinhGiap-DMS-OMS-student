# Exit ticket — Ngày 12

Làm cá nhân, không mở slide, khoảng 10 phút. Ghi đáp án ngay dưới mỗi câu.

**Họ tên:** Phan Dinh Giap · **Mã:** G12

---

**Câu 1 · Chọn một.** Camera DMS trên nhiều xe tự chiếu đèn LED cận hồng ngoại khoảng 940 nm lên mặt tài xế. Lý do chính là gì?

- A. Để đo nhiệt độ da và phát hiện tài xế bị sốt
- B. Để có ảnh mặt ổn định cả ngày lẫn đêm mà không làm chói mắt tài xế
- C. Để có ảnh màu đẹp hơn cho cuộc gọi video
- D. Để camera nhìn xuyên qua mọi loại kính râm

Đáp án: B
Giải thích:

Đèn hồng ngoại tự chiếu sáng mặt cả ngày lẫn đêm. Mắt người gần như không thấy 940 nm nên không bị chói. (Kiến thức chung về NIR.)
A sai: camera NIR ghi ánh sáng phản xạ, không đo nhiệt độ.
C sai: ảnh IR chỉ có độ sáng, không có màu (§4.4, quy tắc 4).
D sai: kính râm có thể trông trong suốt trên NIR (§4.5), nhưng cũng có tròng vẫn tối, khi đó mắt ghi unknown (§4.4, quy tắc 1).

**Câu 2 · Số, 2 chữ số thập phân.** Camera DMS chạy 30 khung hình/giây. Model ước lượng yaw của đầu tài xế lớn hơn 30° liên tục từ khung hình số 240 đến khung hình số 314, **tính cả khung 240 và khung 314**. Tài xế không nhìn đường trong bao nhiêu giây?

Đáp án: 2.50 s
Giải thích:

Tính cả hai đầu: 314 − 240 + 1 = 75 khung.
75 khung ÷ 30 khung/giây = 2.50 s.

**Câu 3 · Chọn tất cả đáp án đúng.** Những câu nào đúng về dữ liệu cabin so với dữ liệu camera ngoài xe?

- A. Không thể làm mờ mặt tài xế như làm mờ mặt người qua đường, vì model DMS cần chính khuôn mặt.
- B. Ban đêm, ảnh cabin thường là ảnh hồng ngoại đen trắng, nên thuộc tính màu áo không gán được.
- C. Camera OMS nhìn về phía sau xe, nên ghế ở bên trái ảnh là ghế bên phải của xe.
- D. Vật trong cabin thường ở xa và nhỏ, nhiều vật trong một ảnh.
- E. Cảnh cabin cố định nên không cần guideline hay QC riêng.

Đáp án: A, B, C
A đúng: DMS cần khuôn mặt để gán mắt, hướng nhìn và 13 điểm mặt (§4.5, §4.6).
B đúng: ảnh IR chỉ có độ sáng, không đoán được màu (§4.4, quy tắc 4).
C đúng: ghế gọi theo xe. Camera OMS nhìn về phía sau nên trái ảnh là rear_right (§3; §4.4, quy tắc 3).
D sai: vật trong cabin thường gần camera, tương đối lớn và ít. Xa, nhỏ, nhiều vật là đặc điểm của camera ngoài xe. (Suy luận từ vị trí camera.)
E sai: cabin vẫn cần guideline riêng (§4.1–4.8) và bước tự kiểm (§4.9).
