# Guideline patch

Đề xuất sửa luật (không sửa trực tiếp `docs/02-rules-vi.md`). Xuất phát từ ca escalate ở `30_escalation_ticket.md` (`adasind_258420.jpg` L9/M7, decision D4) và ca calib `adasind_019560.jpg` L1.

- **Rule mới đề xuất:** R03b — vật bị che gần hết. Nếu phần còn thấy của một vật không đủ để phân biệt Bike, Pedestrian hay người ngồi sau xe khác (ví dụ chỉ thấy một mảng áo, mặt bị làm mờ, phần lớn bị xe khác che), không đoán class: vẽ `ignore_region` với `reason=unreadable` (R06) thay vì box. Bổ sung thêm cho R03: người đứng cạnh và tay đặt trên xe đạp mà không thấy đang ngồi trên yên thì coi là người dắt xe (một `Pedestrian` và một `Bike` tách); nếu không phân biệt được thì áp dụng R03b.
- **Áp dụng cho:** class `Bike` và `Pedestrian`; attribute `occluded`; `ignore_region` với `reason=unreadable`; zone `mid` là nơi ca này xuất hiện (L8 và L9 của `adasind_258420.jpg`).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R03 chỉ nói người lái ngồi trên xe hai bánh là một `Bike` và người dắt xe là `Pedestrian` cộng `Bike`. R06 có `unreadable` nhưng không nói dùng khi nào cho vật bị che một phần. Vì vậy cùng một vật bị ba cách đọc khác nhau: A gán `Bike`, model gán `Pedestrian`, reference bỏ qua (`findings.csv`, dòng L9/M7). Ca `adasind_019560.jpg` L1 cũng chưa phân xử được vì R03 không nói ca đứng cạnh xe đạp (reference gộp một `Bike`, A vẽ `Pedestrian`).
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** round gán nhãn kế tiếp sau khi Lab Coach hoặc guideline owner duyệt. Không áp dụng ngược cho `r1_craft` đã khóa (`A5E8-147C`); bản `rework` (`F395-252E`) cũng giữ nguyên.
