# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Zone `mid` của frame đông xe (`adasind_258420.jpg`) | L có 3 spurious và 1 missing ở `mid` (bảng `zone_table.md`): L3/R5 box Car lệch (E0), L8 Bike bị che mà reference bỏ sót (E0), L9 chưa rõ Bike hay Pedestrian (E5, đã escalate). | Nhiều vật nhỏ, chồng lấn và bị che trong cùng vùng; ba cách đọc (A, reference, model) khác nhau nên số spurious/missing ở đây không nói được ai sai. | `findings.csv` (dòng L3, L8, L9, R5, M7), `30_escalation_ticket.md`, `40_decision_log.csv` D3 và D4, ảnh `submission/screenshots/c_adasind_258420_LRM_overlay.png`. |
| Vật ThreeWheeler và người ngồi trong xe ở cả ba frame (`258420`, `270517`, `310008`) | 19 dòng E4: 13 `M_only` và 6 `LR_noM`. Model gán ThreeWheeler thành Car, Truck hoặc Bus và tách người trong xe hoặc rider thành Pedestrian. | Lỗi lặp ở cả ba frame và cả ba zone (center, mid, edge), nên là mẫu hệ thống của model chứ không phải lỗi nhãn ngẫu nhiên; cần chắc chắn nhãn L và R đúng trước khi dùng làm chuẩn so model. | `findings.csv` (các dòng `E4_model_domain`), `r3_diag/model_compare.md`, `r3_diag/local_quality_confusion.csv`, `D5` trong `40_decision_log.csv`. |

Giới hạn của kết luận từ ba frame ADASIND: chỉ có 3 frame và 20 box tham chiếu; mỗi zone chỉ có 6-7 vật, nên không đủ để nói rìa ảnh khó hơn tâm hay ngược lại. Kết quả ghép đổi theo ngưỡng IoU (edge của L thiếu 0 vật ở IoU 0.5 nhưng thiếu 2 ở 0.7, `iou_sweep.md`). Reference là bản sửa tay có lỗi (E0 ở L3/R5, L8), và `delta.md` cho thấy rework không đổi số nào vì slice không có ca P0/P1.

## Chuyển sang kế hoạch bốn camera giả lập

Với 200 frame ở `45_sampling_plan.csv`, soát độ phủ theo từng cặp camera × normal/hard chứ không gộp chung; mỗi camera phải có đủ hard case riêng (vật sát thân xe, che khuất, seam). Không đếm các frame liền nhau trong cùng cảnh là nhiều ca độc lập: nên lấy theo cảnh hoặc đoạn video, và ghi số cảnh khác nhau bên cạnh số frame. Kế hoạch này chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi: 200 frame trên 50.000 frame là mẫu có chủ đích thiên về ca khó, không phải mẫu ngẫu nhiên, và đồng thuận giữa hai người soát chỉ cho biết họ cùng cách đọc, không chứng minh cách đọc đó đúng. Muốn ước lượng tỷ lệ lỗi cần thêm một mẫu ngẫu nhiên riêng cho mỗi camera.
