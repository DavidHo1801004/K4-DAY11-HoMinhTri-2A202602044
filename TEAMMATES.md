# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: K4 - VinAI20K
- Tên nhóm: Varcoach
- Repo Public: https://github.com/DavidHo1801004/K4-DAY11-Varcoach
- Máy giữ hồ sơ chính / người quản lý: Cường (vai C, chạy check và push); Trí và Toàn push phần việc của mình lên repo nhóm rồi Cường pull
- Slice chung lấy từ mode.json: B4-dense
- Tên định danh vai A dùng cho --self: Trí (members: Cường, Toàn, Trí)
- Kênh trao đổi nội bộ: Discord và trao đổi trực tiếp tại phòng Lab
- Đại diện nộp (vai C): Thái Đức Cường, MSSV 2A202602065
- Commit chốt bài: [SHA hoặc URL commit]

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Hồ Minh Trí | 2A202602044 | Trí | Parking/C0/slice, self-QC, lock, rework | Commit 613176b (P0 parking), 95e8238 (P1 C0) và 51cc24c (P2 B4-dense, mã khóa A5E8-147C): `submission/parking/annotations.xml`, `observations.md`, `00_setup/mode.json`, `team.json`, `doctor.txt`, `p1_calib/{annotations.xml,lock.txt,reference.txt,compare.md,compare.html}`. Slice B4-dense: `r1_craft/{annotations.xml,lock.txt,selfqc.md}` (22 box, 9 polygon). Chưa có `rework/` (P5). |
| B · QA độc lập | Nguyễn Quý Toàn | 2A202602052 | Toàn | Review trước reference, finding QA, kiểm lại ca sửa | Chưa có commit nào của B trên repo nhóm (`git log` chỉ có commit của A). B cần push `r2_qa/` và ghi link vào đây. |
| C · Chẩn đoán & điều phối | Thái Đức Cường | [Điền] | Cường | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | P0: `submission/00_setup/sensor_context.md`, `submission/45_sampling_plan.csv` (8 dòng, tổng 200), `submission/46_gold_set_plan.md` (bản nháp), `TEAMMATES.md`. P1: chẩn đoán 3 finding calib C0 (L1, L5, L6 frame adasind_019560.jpg) trong `submission/findings.csv`, đối chiếu `p1_calib/compare.md` với reference sau lock 133A-F3D3. |

Bảng này xác định vai của nhóm. Vòng CLI tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng
một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | `mode.json` (slice chung B4-dense), `TEAMMATES.md`, `sensor_context.md` | A dùng mode.json để lấy slice; B đọc parking/observations của A | Xong. Parking chờ B soát luật vạch. |
| P1 · Calib C0 | A → C | `p1_calib/lock.txt` (mã 133A-F3D3), `compare.md`, commit 95e8238 | C mở reference sau lock, xem ảnh gốc | Xong: L5 (rider vẽ riêng) và L6 (box trùng xe đạp) là lỗi annotator theo R03; L1 chưa phân xử (người đứng cạnh xe đạp, R03 chưa nói rõ), cần thống nhất trước P2. |
| P2 · Khóa bản đầu | A → B, C | `r1_craft/annotations.xml`, `lock.txt` (slice B4-dense, mã A5E8-147C, sha256 a5e8147c…), commit 51cc24c | C đã đọc lock.txt và selfqc.md; B chưa nhận | A đã khóa. Còn vướng: 9 ô checklist trong `selfqc.md` chưa tick, cảnh báo `adasind_310008.jpg` hai box cùng class IoU > 0.7 và "Tên task thiếu raw_fisheye" chưa được giải thích; A cần xác nhận đã soát tay trước khi B nhận. |
| P3 · Chốt QA mù | B → C, A | `r2_qa/qa_overlay.html`, `qa_review.md`, dòng `r2_qa` trong findings, commit | [Điền] | Chờ B: `r2_qa/` và `screenshots/` hiện còn trống. B chạy `qa --slice B4-dense --file submission/r1_craft/annotations.xml --code A5E8-147C`. |
| P4 · Quyết định sửa | C → A, B | [finding, decision log, commit] | [Điền] | Chờ P3 |
| P5 · Kiểm bản sửa | A → B → C | [v2, lock2, review kiểm lại, delta] | [Điền] | Chờ P4 |
| P6 · Chốt nộp | A, B → C | [manifest, commit chốt] | [Điền] | Chờ P5 |

## 4. Bất đồng và phối hợp

- Một ca bất đồng đã phân xử: chưa có (chờ QA của B ở P3). Ca tham chiếu P1: adasind_019560.jpg L5, rider vẽ riêng thành Pedestrian; C kết luận E1 theo R03, xem `submission/findings.csv`.
- Ca còn mở: adasind_019560.jpg L1 (người đứng cạnh xe đạp, Pedestrian+Bike hay một Bike). Người theo dõi: Cường. Phép kiểm: hỏi Lab Coach hoặc đề xuất bổ sung R03 trong `20_guideline_patch.md`.
- Đóng góp của A/B/C vào kế hoạch và exit ticket: [Điền phần việc thực tế]
- Thay đổi phân công nếu có: chưa đổi.

## 5. Xác nhận trước khi nộp

- [ ] A xác nhận nhãn và export đúng phiên bản: [Tên / bằng chứng]
- [ ] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: [Tên / bằng chứng]
- [ ] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: [Tên / bằng chứng]
- [ ] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
