# Sensor context

Ghi theo quan sát trực tiếp trên ảnh ADASIND (ví dụ `adasind_019560.jpg`, `adasind_001320.jpg`); không có tài liệu rig/calibration đi kèm nên không khẳng định thông số nào ngoài những gì nhìn thấy.

- Rig: ảnh là **một camera fisheye** nhìn về phía trước, có vẻ gắn trên xe hai bánh (xe máy/scooter): ở đáy khung có tay lái, bàn tay, tay áo và chân người lái. Camera nhìn dọc theo đường, thấy làn xe phía trước, xe ba bánh, xe tải, xe máy, người đi bộ và cả gia súc bên lề. Khung ảnh dọc (khổ chân dung); không suy ra độ cao, góc nghiêng hay vị trí gắn chính xác.
- `ego_body`: nhìn thấy ở **góc dưới bên trái** (tay lái, tay và cánh tay, đùi/chân người lái, một phần thân xe) trong các frame có thân xe như `adasind_001320.jpg`. Ở `adasind_019560.jpg` chỉ thấy một phần nhỏ ở mép trái và đáy khung. Có frame không thấy thân xe ego (theo GUIDE: `adasind_006840.jpg`, `adasind_271039.jpg`), nên không vẽ `ego_body` cho hai frame đó.
- Vòng kính (lens circle): là một **hình tròn/elip lớn nằm gần giữa khung**, chiếm khoảng 80–90% chiều rộng và khoảng 70–75% chiều cao ảnh. Ngoài vòng tròn là vùng đen của khung/ống kính ở bốn góc và phía trên, phía dưới. Nội dung bị méo rõ ở rìa vòng kính (đường thẳng cong, vật bị nén), còn vùng giữa gần như tự nhiên. Vật sát rìa vòng dễ bị `truncated` do vòng kính cắt.
- Giới hạn: đây chỉ là **một camera**; không suy ra camera trái/phải/sau, không suy ra seam giữa các camera và không dùng bin `center/mid/edge` để đoán vật ở gần hay xa xe.
