# Phân công Day11 · SVM 360 Fisheye — Nhóm 3 người

| Vai | Họ tên | Tên định danh (mode) | Trách nhiệm chính |
|---|---|---|---|
| **A · Gán nhãn** | Hồ Minh Trí | `tri` | Parking, C0, slice chính, self-QC, lock, rework |
| **B · QA độc lập** | Nguyễn Quý Toàn | `toan` | Review mù bản đã khóa, finding QA, kiểm lại ca sửa |
| **C · Chẩn đoán & điều phối** | Thái Đức Cường | `cuong` | Báo cáo, phân xử, kế hoạch, tích hợp hồ sơ, check & nộp |

> **Quan trọng:** làm theo hướng dẫn HTML cho nhóm (một hồ sơ chung), **không** theo GUIDE.md (mỗi người một slice).
> Repo đã chạy `mode --members cuong,toan,tri --self tri` → **slice chung = `B4-dense`**
> (ảnh `adasind_258420.jpg`, `adasind_270517.jpg`, `adasind_310008.jpg`).
> **Toàn và Cường KHÔNG chạy lại `mode --self`** bằng tên mình — sẽ đổi slice của cả nhóm.
>
> Windows: thay `python3` bằng `py` ở đầu mọi lệnh.

## Trạng thái repo hiện tại

- [x] `doctor.txt`, `mode.json`, `team.json`
- [x] `parking/annotations.xml` + `parking/observations.md` (nên bổ sung vị trí cụ thể của 2 vạch — rubric chấm 3 điểm)
- [ ] `00_setup/sensor_context.md` — còn TODO
- [ ] `TEAMMATES.md` — chưa có
- [ ] Toàn bộ P1 → P6

---

## Thứ tự công việc

### P0 · 0–40 phút — phần còn thiếu

| # | Người | Việc |
|---|---|---|
| 1 | Cường | Tạo `TEAMMATES.md` ở thư mục gốc (mẫu trong HTML): họ tên, MSSV, vai, slice `B4-dense`, tên định danh A = `tri` |
| 2 | Cường | Điền `submission/00_setup/sensor_context.md` theo quan sát ảnh (vị trí camera, ego_body, vòng kính) |
| 3 | Toàn | Soát `sensor_context.md` và `parking/observations.md` (2 vạch ở đâu, vạch đã loại và lý do, free_space dừng ở đâu) |
| 4 | Cường | Phác `45_sampling_plan.csv` (8 dòng, tổng 200) và ý đầu tiên cho `46_gold_set_plan.md` |

### P1 · 40–70 phút — Hiệu chuẩn C0

| # | Người | Việc |
|---|---|---|
| 5 | Trí | `python3 lab11.py cvat C0` → tạo task, dán `assets/labels.json`, import `assets/prefill/C0.xml`, soát/sửa nhãn, Ctrl+S, export CVAT 1.1 (tắt Save images) |
| 6 | Trí | `python3 lab11.py lock calib "<zip-C0>"` |
| 7 | Toàn | Đọc `docs/02-rules-vi.md`, đưa nhận xét theo ảnh |
| 8 | Cường | **Sau lock:** `python3 lab11.py reference calib` → `python3 lab11.py compare calib`; ghi 3 finding đầu vào `findings.csv` |

### P2 · 70–125 phút — Slice chính `B4-dense`

| # | Người | Việc |
|---|---|---|
| 9 | Trí | `python3 lab11.py cvat B4-dense` → tạo task, import prefill, gán nhãn, vẽ K12 cho 4 đối tượng gợi ý (polygon + box, Ctrl+click, G) |
| 10 | Trí | Export nháp → `draft "<zip-nháp>"` → `fill` → `selfqc r1_craft`; tick 9 mục **sau khi kiểm trên ảnh** |
| 11 | Trí | Sửa, export bản cuối → `lock r1_craft "<zip-cuối>"`; ghi mã khóa; ghi dòng `r1_craft` vào findings |
| 12 | Trí → Toàn, Cường | Bàn giao `r1_craft/annotations.xml`, `lock.txt`, mã khóa |
| 13 | Cường | Ghi mốc bàn giao vào `TEAMMATES.md`, commit |

> Toàn & Cường **không mở reference/model** của slice chính trong P2.

**Nghỉ 125–140 phút** — giữ nguyên bản khóa.

### P3 · 140–165 phút — QA mù (Toàn)

| # | Người | Việc |
|---|---|---|
| 14 | Toàn | `python3 lab11.py qa --slice B4-dense --file "submission/r1_craft/annotations.xml" --code <MÃ_KHÓA_CỦA_TRÍ>` |
| 15 | Toàn | Mở `r2_qa/qa_overlay.html`, điền `r2_qa/qa_review.md` (frame → object_ref → rule_id → điều thấy → cần kiểm lại); ghi reviewer B, chủ nhãn A, slice, mã khóa ở đầu file |
| 16 | Toàn | Thêm ≥3 dòng `round=r2_qa`, `cell=L_only`, có `rule_id`, **để trống `why`**; lưu ảnh bằng chứng vào `screenshots/` |
| 17 | Toàn | Báo "QA đã chốt" (file, mã khóa, số nhận xét, ca chưa rõ) |
| 18 | Cường | Kiểm nhận xét đủ bằng chứng, ghi mốc, commit. Trí chỉ phản hồi **sau** mốc này |

### P4 · 165–200 phút — Chẩn đoán (Cường chủ trì)

| # | Người | Việc |
|---|---|---|
| 19 | Cường | Chạy lần lượt: `reference r1_craft`, `compare r1_craft`, `local-quality`, `model`, `iou-sweep --iou 0.3,0.5,0.7` |
| 20 | Cường | Điền dòng `r3_diag`: `why`, `severity`, `owner`, `action`, `rule_id`, `evidence` |
| 21 | Cường | Viết mục **Nhận xét** trong `r3_diag/zone_table.md` |
| 22 | Toàn → Trí → Cường | Phân xử từng ca: B nêu hiện tượng + rule → A giải thích → C đối chiếu ảnh → quyết định giữ/sửa/escalate; ghi vào `40_decision_log.csv` |
| 23 | Cường | `python3 lab11.py triage` đến khi hết lỗi |

### P5 · 200–215 phút — Rework

| # | Người | Việc |
|---|---|---|
| 24 | Trí | Sửa các ca `action=rework` + `severity=P0/P1`, export → `lock rework "<zip>"` |
| 25 | Cường | `python3 lab11.py rework` → đọc `rework/delta.md` |
| 26 | Toàn | Kiểm lại đúng các ca đã sửa |

### P6 · 215–240 phút — Hoàn thiện & nộp

| # | Người | Việc |
|---|---|---|
| 27 | Cường | `python3 lab11.py card` → viết "Phân tích của bạn" trong `10_error_card.md` |
| 28 | Cường | Hoàn thiện `20_guideline_patch.md`, `30_escalation_ticket.md`, `40_decision_log.csv` (≥4 dòng, ≥1 `escalated`), `45_review_plan.md` |
| 29 | Cường | Hoàn thiện `45_sampling_plan.csv` (8 dòng, số nguyên dương, tổng 200) và `46_gold_set_plan.md` (4 camera, refresh, ca seam) |
| 30 | Cả nhóm | `50_exit_ticket.md`: 3 câu (seam & hai box; identity/keyframe/Outside & nối track; một bất đồng thực tế) |
| 31 | Trí | Xác nhận nhãn & phiên bản export trong `TEAMMATES.md` |
| 32 | Toàn | Xác nhận QA độc lập, ≥2 screenshot, ca sửa đã kiểm lại |
| 33 | Cường | `triage` → `status` → `check` (exit 0, `failed_gates` rỗng) |
| 34 | Cường | Commit + push, kiểm repo **Public**, gửi **link repo + SHA commit chốt** qua kênh lớp |

---

## Danh sách file phải nộp

**Nộp:** 1 repo Public `KX-DAY11-TenNhom` + SHA commit chốt (Cường gửi).

| Phụ trách | File trong repo |
|---|---|
| **Trí (A)** | `submission/parking/` · `p1_calib/annotations.xml`, `lock.txt` · `r1_craft/annotations.xml`, `lock.txt`, `selfqc.md` · `rework/annotations-v2.xml`, `lock2.txt` · dòng `r1_craft` trong findings |
| **Toàn (B)** | `r2_qa/qa_review.md`, `qa_overlay.html` · dòng `r2_qa` trong findings · `screenshots/` (≥2 ảnh) |
| **Cường (C)** | `TEAMMATES.md` · `00_setup/sensor_context.md` · `p1_calib/reference.txt`, `compare.md`, `compare.html` · `r3_diag/*` · `rework/delta.md` · dòng `r3_diag` · `10_error_card.md` · `20_guideline_patch.md` · `30_escalation_ticket.md` · `40_decision_log.csv` · `45_review_plan.md` · `45_sampling_plan.csv` · `46_gold_set_plan.md` · `50_exit_ticket.md` · `manifest.json` |

## Điều kiện để `check` qua

- [ ] `findings.csv` ≥ 12 dòng; mỗi round `r1_craft`, `r2_qa`, `r3_diag` ≥ 3 dòng
- [ ] ≥ 4 giá trị `cell` khác `na`; ≥ 8 dòng có M (`LRM`, `LM_noR`, `RM_noL`, `M_only`)
- [ ] Bằng chứng phủ 3 zone: center / mid / edge
- [ ] Dòng `r2_qa` có `rule_id`, `why` trống; dòng `r3_diag` đủ why/severity/owner/action
- [ ] Decision log ≥ 4 mục, ≥ 1 escalation (khớp `action=escalate` trong findings)
- [ ] ≥ 2 screenshot; `delta.md` có số trước/sau
- [ ] Sampling 8 tổ hợp, tổng 200; gold plan đủ 4 camera
- [ ] Không còn TODO trong file bắt buộc
- [ ] `check` exit 0, `manifest.json` có `failed_gates` rỗng
- [ ] Repo Public; cả 3 người đã xác nhận trong `TEAMMATES.md`

## Nguyên tắc không được vi phạm

- Thứ tự: **A vẽ → A self-QC/lock → B QA mù → C mở reference/chẩn đoán → A sửa → B kiểm lại**.
- Không sửa tay XML sau khi khóa; cần khóa lại thì ghi lý do vào decision log và dùng `--relock`.
- Mỗi file chỉ một người sửa tại một thời điểm; với `findings.csv` giữ nguyên header và dòng cũ.
- Không dùng `docker compose down -v`; không đưa mật khẩu/token/.env vào repo.
- Chậm quá 5 phút → báo Lab Coach, cắt giảm theo `docs/08-degrade-vi.md` bằng `python3 lab11.py degrade <bước>`.
