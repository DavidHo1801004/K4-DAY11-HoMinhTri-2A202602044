# QA review · B4-dense

- **Reviewer (B, QA độc lập):** Nguyễn Quý Toàn (`toan`)
- **Chủ nhãn (A):** Hồ Minh Trí (`tri`)
- **Slice:** `B4-dense` (`adasind_258420.jpg`, `adasind_270517.jpg`, `adasind_310008.jpg`)
- **Mã khóa:** `A5E8-147C` (theo `submission/r1_craft/lock.txt`, sha256 `a5e8147c…`)

**Loại review:** QA mù — chỉ đối chiếu ảnh, annotation và rules; chưa dùng teaching reference/model.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| `adasind_310008.jpg` | `L2 Pedestrian` + `L5 Pedestrian` | `R01/R02` | Hai bounding box Pedestrian chồng lấn rất lớn và có kích thước/vị trí gần tương đương. Cần kiểm trực tiếp trên ảnh xem đây là hai người riêng biệt hay cùng một người bị gán nhãn trùng. Nếu là cùng một đối tượng thì cần loại một box duplicate. |
| `adasind_270517.jpg` | `L5 Bike` | `R02`, `R05` | Bike nằm sát mép phải ảnh và bounding box chạm biên ảnh. Cần kiểm box có bám đúng phần nhìn thấy trên ảnh fisheye gốc hay không, đồng thời kiểm attribute `truncated` nếu vật thực sự bị biên ảnh cắt. |
| `adasind_258420.jpg` | `L3 Car` + `L9 Bike` | `R02`, `R05` | Hai box có vùng giao nhau đáng kể. Cần soi ảnh để xác nhận đây là hai object độc lập và geometry của từng box chỉ bám phần nhìn thấy; nếu Bike bị Car/vật khác che thì kiểm lại `occluded`. |
| `adasind_258420.jpg` | `L6 Pedestrian` | `R01`, `R02` | Pedestrian có box khá hẹp ở vùng đông đối tượng. Cần kiểm lại trên ảnh xem box đã bao đủ phần người nhìn thấy chưa và không bỏ sót phần visible do fisheye distortion. |
| `adasind_270517.jpg` | `L7 ThreeWheeler` | `R02`, `R04` | Bounding box có kích thước lớn so với các object lân cận. Cần kiểm lại visible extent và xác nhận đúng class `ThreeWheeler`, tránh box ăn sang background hoặc nhiều object khác. |

## Kết luận QA

Đã thực hiện review mù trên cả 3 frame của slice `B4-dense`.

Ưu tiên kiểm lại:
1. Khả năng duplicate `L2/L5 Pedestrian` ở `adasind_310008.jpg`.
2. Geometry và `truncated` của `L5 Bike` ở `adasind_270517.jpg`.
3. Geometry/occlusion giữa `L3 Car` và `L9 Bike` ở `adasind_258420.jpg`.

Các nhận xét trên chỉ mô tả **WHAT** và rule liên quan. Chưa kết luận nguyên nhân (`WHY`) trong vòng `r2_qa`; các ca chưa rõ sẽ được đối chiếu ở P4.

**QA đã chốt:** slice `B4-dense`, mã khóa `A5E8-147C`, 5 nhận xét, trong đó các ca trên cần đối chiếu tiếp ở P4.
## Ảnh bằng chứng

Ảnh chỉ vẽ nhãn L của A (đỏ là đối tượng đang xét), chưa có reference hay model:
- `submission/screenshots/b_qa_310008_L2_L5.png`: L2 và L5 Pedestrian.
- `submission/screenshots/b_qa_270517_L5_edge.png`: L5 Bike sát mép phải.
- `submission/screenshots/b_qa_258420_L3_L9.png`: L3 Car và L9 Bike.
