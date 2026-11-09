# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người cỡ nhỏ đến trung bình: sedan, SUV, taxi, xe bán tải (cabin nhỏ). | Xe bán tải/cabin nhỏ vẫn là `car`. Không gồm xe buýt, xe tải, mô tô. |
| `truck` | Xe tải chở hàng, có thùng hàng cỡ lớn, hoặc xe đầu kéo. | Khác bán tải: truck là xe tải chở hàng thực thụ, thùng lớn. |
| `bus` | Xe buýt, minibus chở khách, thân dài, mui cao, nhiều cửa sổ dọc thân. | Khác `van`: kích thước lớn và nhiều cửa sổ hơn hẳn. |
| `van` | Xe van, minivan chở hàng hoặc chở người, thân hộp liền khối. | Khác `car`: mui cao, thân hộp. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: ôm sát phần thân xe nhìn thấy được, không lấy thêm mặt đường, trời hay xe bên cạnh. Mỗi xe một box riêng, kể cả cùng lớp.
- Vật thể bị che khuất: vẽ box quanh phần nhìn thấy được nếu nhận diện được lớp. Hai xe chồng nhau thì vẽ hai box riêng trên phần nhìn thấy của từng xe.
- Vật thể bị cắt bởi rìa ảnh: box chạm đúng rìa ảnh, không đoán phần nằm ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: nếu chỉ còn mảnh rất nhỏ và không xác định chắc lớp thì không tự đoán; ghi case ID vào bảng bên dưới và hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | Xe bị xe khác che một phần | Box cả phần phía sau xe che, dán đúng lớp (đã hỏi lab coach) | GUIDE: bị che một phần vẫn gán nếu nhận diện được lớp. |
| `drive_004` | Quá nhiều xe tụ tập và không thấy rõ điểm ảnh | Không box cho lớp đó vì chỉ box những vật thể có thể nhìn thấy và xác định được lớp (đã hỏi lab coach) | GUIDE: không tự đoán nếu không xác định chắc lớp. |
| `drive_016` | Xe van/minibus mơ hồ giữa `van` và `car` | Chưa tự chọn, sau khi hỏi lab coach thì chốt theo câu trả lời: `van` | GUIDE: van/minivan mơ hồ với `bus` hoặc `car` thì ghi case ID và hỏi. |

