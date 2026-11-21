# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): Nguyễn Duy Khánh (`nguyenduykhanh`)        Người kiểm (học viên): Lê Ngọc Bích Loan (`lengocbichloan`)  
Ngày: 21/11/2026                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `nguyenduykhanh-front.zip`, `nguyenduykhanh-fisheye.zip`, manifest `nguyenduykhanh-manifest.json` **revision 2**. sha256 hai file khớp manifest (front `ffdde396…a6f235`, fisheye `da2f713e…2cbec56`); `contains_images = false`.  
Số ảnh kiểm: 6 (+ 0 ảnh thêm)   Số nhãn đã kiểm (shape + tag): **80** (56 shape + 24 tag)  

Ảnh mẫu: A03, A04, A07 (front) · B02 (fisheye_front), B03 (fisheye_rear), B06 (fisheye_left).  
Cách kiểm: đọc `annotations.xml` trong hai zip, vẽ nhãn chồng lên ảnh gốc trong `data/` và phóng to từng vùng; chạy `road_check lint` trên cả hai zip (20 ảnh, 8 cảnh báo: QC1-05 ở A01, A04, A09, A11; QC1-02 ở A05, A09). Cảnh báo lint ngoài mẫu không tính vào phiếu.

| Ảnh | Shape | Tag | Tổng |
| --- | ---: | ---: | ---: |
| A03 | 4 | 4 | 8 |
| A04 | 10 | 4 | 14 |
| A07 | 5 | 4 | 9 |
| B02 | 15 | 4 | 19 |
| B03 | 13 | 4 | 17 |
| B06 | 9 | 4 | 13 |
| **Tổng** | **56** | **24** | **80** |

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 ảnh (24 tag) | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 4 ảnh (A03, A04, A07, B03) | 1 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 18 lane | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 18 lane | 0 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 18 lane | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 18 lane | 4 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 29 (drivable, crosswalk, road_curb) | 1 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 4 traffic_light | 1 | chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 11 đèn + biển | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 7 traffic_sign | 1 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 37 shape (B02, B03, B06) | 0 | chính |
| QC1-12 file | lỗi file | 2 zip + manifest | 0 | nhẹ / nặng |

## Danh sách lỗi (mỗi lỗi một dòng)
File CVAT for images 1.1 không ghi id cho shape, nên nhãn được định vị bằng tọa độ điểm đầu hoặc góc box.

| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-02 | B03 | lane L1 (thiếu) | nặng | Bỏ sót: vạch đứt trắng bên trái làn đang đi thấy rõ ở khoảng (495,351)–(574,301), ngay trước xe phía sau; lô chỉ có R1, R2. |
| 2 | QC1-06 | A04 | lane L1, điểm đầu (27,552) | chính | Sai thuộc tính: vạch tim đường là vàng đôi nhưng `color = white` (phải `yellow`). |
| 3 | QC1-06 | A04 | lane L1, điểm đầu (68,550) | chính | Sai thuộc tính: polyline thứ hai của cùng cặp vàng đôi cũng `color = white`. |
| 4 | QC1-06 | A04 | lane R1, điểm đầu (720,535) | chính | Sai thuộc tính: vạch giữa hai làn cùng chiều là vạch đứt nhưng `style = solid` (phải `dashed`). |
| 5 | QC1-06 | A03 | lane L1, điểm đầu (229,696) | chính | Sai thuộc tính: vạch bên trái là vạch vàng liền nhưng `color = white` (phải `yellow`). |
| 6 | QC1-07 | A07 | drivable direct, cạnh trái (0,523)–(428,359) | chính | Tràn vùng: polygon kéo tới chân dải phân cách bê tông, gồm cả lề nằm ngoài L1 (L1 của chính lô ở (25,555)–(385,373)). Guideline: vùng lái không gồm lề. |
| 7 | QC1-08 | B02 | traffic_light (thiếu), khoảng (1088,20)–(1190,195) | chính | Bỏ sót: cụm đèn xe nhìn từ phía sau ở góc trên phải, bên phải đèn đi bộ (1076,73)–(1142,167), cao hơn 8 px, chưa có box (`facing = back`). |
| 8 | QC1-10 | B02 | traffic_sign, box (68,66)–(139,234) | chính | Gán sai đối tượng: box ôm mặt sau một cụm đèn tín hiệu cạnh box đèn (105,111)–(162,217), không có mặt biển nào. Nên đổi thành `traffic_light` (`facing = back`) hoặc bỏ. |

Ảnh được ghi **không lỗi**: B06 (4 lane đều `position = other` đúng cho camera bên; hai polygon crosswalk là hai vạch ngựa vằn khác nhau, không chồng lên nhau; vùng lái không tính thân xe; polyline theo cung).

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A04 | Vạch đôi (L1 vàng đôi, R2 hai vạch trắng viền làn xe đạp) đang vẽ thành **hai polyline** cùng `position`, `kind = double`. Có chấp nhận không, hay phải gộp thành một polyline ở tâm cặp vạch? Lint báo QC1-05. | |
| A03 | Ảnh không có box đèn, biển nào, dù có đèn xanh (~(704,310)–(718,333)), biển trường học vàng (~(999,280)–(1040,327)) và biển công trường cam đều cao hơn 8 px. Bảng kế hoạch ảnh có yêu cầu đèn, biển cho A03 không? Nếu có thì là bỏ sót QC1-08 / QC1-10. | |
| A03 | R1 dừng ở (698,463), không tới sát xe. Đoạn gần xe vạch sơn rất mờ: có tính là thấy được (QC1-04) không? | |
| A07 | Hai đèn xanh nhỏ ở xa (~(661,229)–(669,240), ~(702,242)–(709,251)) cao khoảng 8–11 px, chưa có box. Có thuộc ngưỡng bắt buộc 8 px không? | |
| B02 | Box đèn (105,111)–(162,217) ghi `color = off`, `facing = front`. Mặt nhìn thấy trông giống mặt sau cụm đèn; cần lead xác nhận `facing`. | |

## Quyết định
Lỗi nặng: **1**   Lỗi chính: **7 / 80 nhãn = 8,75 %**   Lỗi nhẹ: **0**   Ca chờ lead: **5**  
Quyết định (Release / Hold / Rework): **Rework**  
Lý do (theo luật của phiếu): có lỗi nặng (QC1-02 thiếu L1 ở B03, vạch thấy được), và tỉ lệ lỗi chính 8,75 % > 5 %. Mỗi điều kiện đều đủ để Rework, không phụ thuộc các ca chờ lead.

Gợi ý sửa:
- Thêm L1 ở B03.
- Sửa `color` thành `yellow` cho L1 ở A04 và A03; sửa `style` thành `dashed` cho R1 ở A04.
- Thu cạnh trái drivable A07 về L1.
- B02: thêm box cụm đèn sau ở góc trên phải; đổi box (68,66)–(139,234) từ `traffic_sign` sang `traffic_light` hoặc bỏ.
- Export lại và tạo manifest `--revision 3`.

Ghi chú: hai lỗi trong bản nháp đầu đã được bỏ sau khi đối chiếu ảnh. (1) "A07 L1 vẽ theo mép bê tông" không đúng: polyline nằm trên vệt sơn mờ, tách khỏi chân dải phân cách. (2) "B06 hai crosswalk giao nhau" không đúng: đó là hai vạch ngựa vằn riêng.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](../docs/qc_severity_road_v1.0.md).
