# Mini guideline — bài làm cá nhân

> Bản nháp quy tắc được chuẩn bị trước khi gán nhãn. Khi làm bài, kiểm tra từng ảnh ở độ phân giải đầy đủ và cập nhật các case mơ hồ theo quyết định thực tế; không dùng bảng này thay cho việc tự quan sát ảnh.

## Định nghĩa lớp

| Lớp | Định nghĩa | Điểm phân biệt |
| --- | --- | --- |
| `car` (ID 0) | Xe con, sedan, SUV, taxi, xe bán tải | Xe con và bán tải thuộc `car`, không gán `truck` chỉ vì xe có thùng nhỏ. |
| `truck` (ID 1) | Xe tải chở hàng, xe đầu kéo | Xe có cấu hình xe tải chở hàng/đầu kéo; không gồm bán tải hoặc cabin nhỏ thuộc `car`. |
| `bus` (ID 2) | Xe buýt, minibus chở khách | Xe có dạng xe chở khách nhiều chỗ; xe van/minivan vẫn là `van`. |
| `van` (ID 3) | Van/minivan chở hàng hoặc chở người | Dạng thân van/minivan; không gộp vào `bus` chỉ vì đang chở khách. |

Giữ nguyên ID lớp trên trong mọi file YOLO. Nhãn mô tả loại xe, không phải trạng thái di chuyển.

## Quy tắc chung

- **Box:** mỗi xe một box riêng, sát đường bao phần thân xe nhìn thấy; không lấy thêm mặt đường, trời hoặc xe bên cạnh.
- **Che khuất:** vẫn gán nếu phần còn nhìn thấy đủ nhận diện xe và lớp; chỉ khoanh phần nhìn thấy. Xe chồng lấp có box riêng.
- **Cắt bởi rìa ảnh:** gán nếu nhận diện được; box kéo đến đúng rìa ảnh, không đoán phần nằm ngoài khung.
- **Rất nhỏ hoặc mơ hồ:** không đoán khi không thể xác định chắc là xe thuộc schema hoặc không phân biệt được lớp. Ghi tên ảnh/vị trí và hỏi giảng viên.
- **Ngoài phạm vi:** không gán người đi bộ, xe đạp, xe máy, biển báo, đèn giao thông, phản chiếu hoặc hình xe trên quảng cáo. Xe đang đỗ vẫn gán theo loại xe.
- **Định dạng:** xuất đủ một file `.txt` cho mỗi ảnh train, cùng basename; ảnh không có xe thuộc schema có file rỗng. Mỗi dòng gồm `class_id x_center y_center width height`, tọa độ chuẩn hóa `[0,1]`.

## Case cần chú ý khi xem ảnh

Các ảnh dưới đây được chọn từ contact sheet train để nhắc kiểm tra tình huống; phân loại cuối cùng phải dựa trên ảnh gốc ở kích thước đầy đủ.

| Ảnh | Tình huống cần kiểm tra | Quyết định áp dụng | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_009.jpg` | Nhiều xe buýt lớn ở gần nhau và che khuất một phần | Mỗi xe buýt nhận diện được là một box `bus` riêng; box chỉ bao phần nhìn thấy. | Xe chở khách dạng bus thuộc `bus`; không gộp các xe thành một box. |
| `drive_028.jpg` | Dòng xe có xe thân hộp trông giống van/minivan ở cạnh các xe con | Chỉ gán `van` nếu ảnh đầy đủ cho thấy rõ cấu hình van/minivan; nếu không phân biệt chắc với `car`, ghi case và hỏi giảng viên, không đoán. | `van` dành cho van/minivan; xe con thông thường thuộc `car`. Hình thu nhỏ không đủ để kết luận thay người gán nhãn. |
| `drive_042.jpg` | Xe chở hàng có kích thước lớn giữa dòng giao thông | Nếu quan sát ảnh gốc xác nhận là xe tải chở hàng, gán `truck`; nếu chỉ là bán tải/cabin nhỏ thì gán `car`. | Phân biệt theo loại xe trong schema, không theo kích thước đơn thuần. |

Sau self-QC, cập nhật bảng bằng quyết định đã kiểm tra trên ảnh gốc; ghi rõ ảnh, vị trí/tình huống và lý do nếu cần hỏi giảng viên.
