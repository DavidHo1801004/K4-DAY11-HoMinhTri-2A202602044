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
- Commit chốt bài: (https://github.com/DavidHo1801004/K4-DAY11-Varcoach)

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Hồ Minh Trí | 2A202602044 | Trí | Parking/C0/slice, self-QC, lock, rework | Commit 613176b (P0 parking), 95e8238 (P1 C0) và 51cc24c (P2 B4-dense, mã khóa A5E8-147C): `submission/parking/annotations.xml`, `observations.md`, `00_setup/mode.json`, `team.json`, `doctor.txt`, `p1_calib/{annotations.xml,lock.txt,reference.txt,compare.md,compare.html}`. Slice B4-dense: `r1_craft/{annotations.xml,lock.txt,selfqc.md}` (22 box, 9 polygon). P5: `rework/{annotations-v2.xml,lock2.txt,delta.md}` (mã khóa F395-252E, commit cbf648c). |
| B · QA độc lập | Nguyễn Quý Toàn | 2A202602052 | Toàn | Review trước reference, finding QA, kiểm lại ca sửa | `submission/r2_qa/qa_review.md`, `qa_overlay.html` (commit 48ab9d9), 5 dòng `round=r2_qa` trong `findings.csv`, ảnh `submission/screenshots/b_qa_*.png`. B đã đọc lại và xác nhận phần Cường sửa thay (mã khóa A5E8-147C, dòng r2_qa, ảnh). Kiểm lại ca sửa ở P5: B đã xác nhận. |
| C · Chẩn đoán & điều phối | Thái Đức Cường | 2A202602065 | Cường | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | P0: `submission/00_setup/sensor_context.md`, `submission/45_sampling_plan.csv` (8 dòng, tổng 200), `submission/46_gold_set_plan.md`, `TEAMMATES.md`. P1: chẩn đoán 3 finding calib C0 (L1, L5, L6 frame adasind_019560.jpg) trong `submission/findings.csv`, đối chiếu `p1_calib/compare.md` với reference sau lock 133A-F3D3. P4: `r1_craft/{reference.txt,compare.md,compare.html}`, `r3_diag/*`, 24 dòng `r3_diag`, `40_decision_log.csv`, `30_escalation_ticket.md`. P5: đọc `rework/delta.md`. P6: `10_error_card.md`, `20_guideline_patch.md`, `45_review_plan.md`, `50_exit_ticket.md`, `manifest.json`; chạy `triage` và `check`. |

Bảng này xác định vai của nhóm. Vòng CLI tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng
một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | `mode.json` (slice chung B4-dense), `TEAMMATES.md`, `sensor_context.md` | A dùng mode.json để lấy slice; B đọc parking/observations của A | Xong. Parking chờ B soát luật vạch. |
| P1 · Calib C0 | A → C | `p1_calib/lock.txt` (mã 133A-F3D3), `compare.md`, commit 95e8238 | C mở reference sau lock, xem ảnh gốc | Xong: L5 (rider vẽ riêng) và L6 (box trùng xe đạp) là lỗi annotator theo R03; L1 chưa phân xử (người đứng cạnh xe đạp, R03 chưa nói rõ), cần thống nhất trước P2. |
| P2 · Khóa bản đầu | A → B, C | `r1_craft/annotations.xml`, `lock.txt` (slice B4-dense, mã A5E8-147C, sha256 a5e8147c…), commit 51cc24c | C đã đọc lock.txt và selfqc.md; B đã nhận đúng bản khóa để QA | Xong. Cảnh báo `adasind_310008.jpg` hai box cùng class IoU > 0.7 đã được phân xử ở P4 (D1: hai người khác nhau, giữ cả hai). A đã xác nhận nhãn và export đúng phiên bản. |
| P3 · Chốt QA mù | B → C, A | `r2_qa/qa_overlay.html`, `qa_review.md` (5 nhận xét, 3 frame), commit 48ab9d9 | C đọc đủ 5 nhận xét, đối chiếu ảnh ở P4 | B đã chốt QA. Cường đã sửa thay B: mã khóa trong `qa_review.md` thành `A5E8-147C`, thêm 5 dòng `round=r2_qa` vào `findings.csv`, thêm 3 ảnh `b_qa_*.png` (vẽ từ nhãn L, chưa có reference/model). B đã xác nhận. |
| P4 · Quyết định sửa | C → A, B | `r3_diag/*`, 24 dòng `r3_diag` trong `findings.csv`, `40_decision_log.csv` (D1-D5), `30_escalation_ticket.md`, commit 4ab184d và các commit sau | A đã xác nhận và B đã xác nhận các quyết định D1-D5 trong `40_decision_log.csv` | C đã chạy reference/compare/local-quality/model/iou-sweep, `triage` báo hợp lệ. 5 nhận xét QA của B đã có quyết định: giữ D1, D2, D3; escalate D4 (L9); D5 là lỗi model. |
| P5 · Kiểm bản sửa | A → B → C | `rework/annotations-v2.xml`, `lock2.txt` (mã F395-252E), `delta.md`, commit cbf648c | C đọc `delta.md`; B kiểm lại, B đã xác nhận | Xong. Số matched/missing/spurious không đổi ở cả 3 zone vì slice B4-dense không có ca rework P0/P1; hai dòng calib `action=rework` (L5, L6) ghi "không áp dụng". Giữ nguyên kết quả, không sửa số. |
| P6 · Chốt nộp | A, B → C | `manifest.json` (`failed_gates` rỗng), `10_error_card.md`, `20_guideline_patch.md`, `45_review_plan.md`, `50_exit_ticket.md`, commit chốt (mục 1) | A đã xác nhận và B đã xác nhận | C chạy `triage` và `check` (exit 0); chờ commit chốt và push. |

## 4. Bất đồng và phối hợp

- Một ca bất đồng đã phân xử: `adasind_310008.jpg` L2/L5. B nghi hai Pedestrian trùng (R01/R02), C xem ảnh gốc thấy hai người khác nhau (áo xanh nhạt và áo tối), reference và model cũng có hai box, nên giữ cả hai (D1 trong `40_decision_log.csv`, ảnh `submission/screenshots/c_adasind_310008_four_people_raw.png`). A đã xác nhận cách giải thích trong log là đúng. Ca tham chiếu P1: adasind_019560.jpg L5, rider vẽ riêng thành Pedestrian; C kết luận E1 theo R03, xem `submission/findings.csv`.
- Ca còn mở: `adasind_258420.jpg` L9/M7 (đã escalate, D4 và ticket 1, người theo dõi Cường, cần luật cho vật bị che gần hết). Ca calib: adasind_019560.jpg L1 (người đứng cạnh xe đạp, Pedestrian+Bike hay một Bike). Người theo dõi: Cường. Phép kiểm: hỏi Lab Coach hoặc đề xuất bổ sung R03 trong `20_guideline_patch.md`.
- Đóng góp của A/B/C vào kế hoạch và exit ticket: C soạn `45_sampling_plan.csv`, `46_gold_set_plan.md`, `45_review_plan.md`, `20_guideline_patch.md` và bản nháp `50_exit_ticket.md`. A đã xác nhận và B đã xác nhận. Câu 3 của exit ticket hiện viết từ góc nhìn của C (ca D1).
- Thay đổi phân công nếu có: chưa đổi.

## 5. Xác nhận trước khi nộp

- [x] A xác nhận nhãn và export đúng phiên bản: Hồ Minh Trí; `r1_craft/lock.txt` (A5E8-147C) và `rework/lock2.txt` (F395-252E)
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Nguyễn Quý Toàn; `r2_qa/qa_review.md`, ảnh `screenshots/b_qa_*.png`, reference chỉ mở sau `r2_qa` (`r1_craft/reference.txt`)
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: Thái Đức Cường; `python lab11.py check` báo "Hồ sơ hình thức đầy đủ"
- [x] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
