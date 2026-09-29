# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

> Bản nháp P0 của người C (ý đầu tiên). Sau P4 sẽ bổ sung lý do dựa trên lỗi đã thấy trên slice ADASIND.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Đông xe hai bánh và ba bánh, rider so với người dắt xe, vật bị che hoặc bị vòng kính cắt, ngược sáng | Nhiều vật chồng nhau; ranh giới `Bike`/`Pedestrian` và `truncated`/`occluded` dễ nhập nhằng; vật nhỏ ở xa gần ngưỡng 40 px | Nhãn trên ảnh fisheye gốc (chưa undistort/BEV); giữ `camera_id`, timestamp, phiên bản calibration và rule version | Hai người soát độc lập trên ảnh gốc, đối chiếu theo rule; bất đồng được người thứ ba phân xử trước khi khoá |
| rear | Xe áp sát phía sau, thân xe ego che một phần, ánh sáng yếu hoặc chói đèn | Vật bị cắt bởi mép ảnh và thân xe; độ tương phản thấp làm khó nhìn ranh giới vật | Nhãn trên ảnh gốc camera sau; giữ calibration và vị trí gắn riêng của camera sau, không dùng chung với camera trước | Soát độc lập, kiểm riêng vùng `ego_body`/`ignore_region`; chỉ gọi gold khi người soát xác nhận trên ảnh gốc |
| left | Vật sát thân xe, méo mạnh ở rìa, vùng chồng với camera trước hoặc sau | Fisheye nén vật ở rìa nên box dễ lệch; vật ở seam có thể bị đếm hai lần hoặc bỏ sót | Nhãn trên ảnh gốc camera trái; giữ calibration, timestamp đồng bộ với camera kế cận để tra seam | Soát độc lập; với ca seam, đối chiếu ảnh hai camera cùng timestamp trước khi kết luận |
| right | Vật sát thân xe, xe hai bánh chạy sát, người đi bộ ở lề, vùng chồng phải | Giống camera trái nhưng phân bố vật khác (bên lề đường); dễ áp nhầm phân bố lỗi của camera trái | Nhãn trên ảnh gốc camera phải; calibration riêng, không sao chép từ camera trái dù rig đối xứng | Soát độc lập riêng cho camera phải; so tỉ lệ lỗi với camera trái, không gộp chung |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): khi thay camera/ống kính hoặc vị trí gắn, khi calibration đổi (nhãn trên ảnh gốc vẫn đúng nhưng ánh xạ sang BEV/seam thì không), khi rule gán nhãn tăng phiên bản (ví dụ đổi định nghĩa `Bike`, ngưỡng 40 px), hoặc khi phân bố dữ liệu thực địa lệch rõ khỏi lúc chọn 200 frame (mùa mới, thành phố mới, ban đêm).
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: một xe máy ở góc trước-trái xuất hiện ở cả camera trước (box ở `edge`) và camera trái (box ở `mid`). Cần người soát quyết định trước khi ghép: (1) hai box có phải cùng một vật không, dựa trên timestamp đồng bộ và calibration; (2) policy đầu ra đích: giữ cả hai, hợp nhất, hay chọn một theo camera có box đầy đủ hơn. Không tự xoá box hay coi là `DUPLICATE` nếu chưa có policy này.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera: hai người đồng ý với nhau chỉ cho biết họ áp cùng cách đọc, không chứng minh cách đọc đó đúng; và số liệu trên ADASIND (một camera, một slice ba frame) không phủ được hình học, ánh sáng, che khuất và seam riêng của bốn camera. Cần review và đo riêng từng camera, kể cả hard slice, cộng phần seam/cross-camera mà ảnh một camera không có.
