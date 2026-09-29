# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | MISSING | 4 |
| center | B4 | SPURIOUS | 5 |
| center | C0 | SPURIOUS | 3 |
| edge | B4 | MISSING | 1 |
| edge | B4 | SPURIOUS | 1 |
| mid | B4 | BOX_GEOMETRY | 1 |
| mid | B4 | MISSING | 2 |
| mid | B4 | SPURIOUS | 13 |
| unknown | B4 | BOX_GEOMETRY | 4 |
| unknown | B4 | DUPLICATE | 1 |

## Top defects
- SPURIOUS: 22 (ví dụ frame adasind_019560.jpg)
- MISSING: 7 (ví dụ frame adasind_258420.jpg)
- BOX_GEOMETRY: 5 (ví dụ frame adasind_258420.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

Lỗi nổi bật nhất là `SPURIOUS` (22 dòng), tập trung ở zone `mid` (13) và `center` (5) của block B4.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: trong 19 dòng SPURIOUS của B4, 15 dòng đến từ phía model (14 `M_only` và `L3+M6`), phần lớn là `E4_model_domain`. Model gán ThreeWheeler thành Car/Truck/Bus, ví dụ `adasind_258420.jpg` M4, M8, M11 và `adasind_270517.jpg` M6, M7, M9. Model cũng tách người ngồi trong xe hoặc rider thành Pedestrian, ví dụ `adasind_258420.jpg` M5, M10 và `adasind_270517.jpg` M5, M10, M11; R03 quy định các vật này không có box riêng. Lặp lại ở cả ba frame và cả ba zone nên đây là mẫu hệ thống, không phải nhiễu. Phần còn lại là L8 và L9 của `adasind_258420.jpg` (hai Bike bị che ở `mid`), một ca `E0_reference_defect` và một ca `E5_unresolved`. Lưu ý L8 và L9 xuất hiện hai lần trong bảng (một lần ở vòng `r1_craft`, một lần ở `r3_diag`), nên con số 22 hơi bị đếm dôi; dòng `unknown` (BOX_GEOMETRY 4, DUPLICATE 1) là 5 dòng `r2_qa` của B, chưa qua chẩn đoán nên không có zone.
- Cách sửa và ai nhận việc (`owner`): không sửa nhãn L vì L khớp reference ở phần lớn ca. Lỗi model thuộc `ai_team`: bổ sung class ThreeWheeler và huấn luyện lại trên ảnh fisheye, dạy luật R03 và vùng `ego_body`. Ca L9 thuộc `guideline`: thêm luật cho vật bị che gần hết (đề xuất R03b trong `20_guideline_patch.md`). L3/R5 và L8 thuộc `data_ops`: sửa hoặc bổ sung reference. A nên đặt `occluded=true` cho L3 và L8 nếu có vòng sửa sau.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `submission/screenshots/c_adasind_258420_LRM_overlay.png` và `c_adasind_310008_four_people_raw.png`; các dòng `r3_diag` có `why=E4_model_domain` trong `findings.csv`; `r3_diag/model_compare.md` và `local_quality_confusion.csv`; quyết định D4, D5 trong `40_decision_log.csv`; ticket 1 trong `30_escalation_ticket.md`; rule R03, R04, R06.
