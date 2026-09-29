# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 6 | 0 | 0 | 4 | 5 | — |
| mid | 7 | 1 | 3 | 2 | 9 | SPURIOUS (2) |
| edge | 7 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: L gãy ở `mid` (1 thiếu, 3 thừa; center và edge không lỗi, IoU 0.5). Cả ba ca thừa/thiếu của L đều ở frame `adasind_258420.jpg` (L3 box Car lệch với reference, L8 và L9 hai Bike nhỏ bị che phía sau). M gãy nhiều nhất cũng ở `mid` (2 thiếu, 9 thừa); `center` có 4 thiếu và 5 thừa, `edge` chỉ 1 thiếu và 1 thừa. Nhiều box thừa của M là do gán ThreeWheeler thành Car/Truck/Bus và tách người trong xe hoặc rider thành Pedestrian.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: lỗi của L tập trung ở vật nhỏ, bị che, nằm xa và dày đặc (mid của frame đông xe), nên là vấn đề che khuất và luật cho vật bị che (R03, R05), không phải méo fisheye. Lỗi của M chủ yếu do lệch miền dữ liệu: không có class ThreeWheeler, không biết R03 và không biết vùng `ego_body`, nên không thể gán cho zone hình học. Giới hạn: chỉ 3 frame, 20 box tham chiếu, `center` và `edge` mỗi zone chỉ có 6-7 vật và reference là bản sửa tay có thể sai (E0 ở L3/R5, L8). Số zone không đủ để kết luận rìa ảnh khó hơn tâm; mọi ca cần soi lại trên ảnh. Kết quả ghép cũng đổi theo IoU (edge L thiếu 0 ở 0.5 nhưng 2 ở 0.7), nên ưu tiên đọc ở IoU 0.5.
