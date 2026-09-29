# Escalation ticket

## Ticket 1

- **Frame:** `adasind_258420.jpg`, object `L9` (A gán Bike) và `M7` (model gán Pedestrian), vùng mid, khoảng [239,793,256,845].
- **Ảnh chụp:** `submission/screenshots/c_adasind_258420_LRM_overlay.png` (cyan = L, đỏ = reference, vàng = model). Ảnh gốc không overlay nằm ở `assets/images/adasind_258420.jpg`.
- **Expected impact:** một vật bị che gần hết có ba kết quả khác nhau: A gán Bike, model gán Pedestrian, reference bỏ qua. Nếu không có luật thì mọi frame đông xe sẽ có cùng loại bất đồng, làm số spurious/missing ở zone mid của slice này không giải thích được (hiện L có 3 spurious ở mid, trong đó L8 và L9 thuộc nhóm này). Không nên tính đây là lỗi annotator.
- **Owner:** `guideline`
- **Recommendation:** thêm vào R03 (bản đề xuất trong `20_guideline_patch.md`) một quy tắc cho vật bị che phần lớn: nếu không đọc được loại vật, chỉ hiện một phần người thì dùng `ignore_region` với reason `unreadable`, không đoán class. Trong lúc chờ, giữ L9 như A đã vẽ và không tính vào số lỗi.
