# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Ô tô con, xe du lịch dưới 9 chỗ ngồi, xe gầm cao cá nhân (SUV/Sedan). | Thân xe ngắn, không có thùng chở hàng rời phía sau, không chở khách công cộng. |
| `truck` | Xe tải chở hàng, xe tải nặng, container, xe bán tải (pickup). | Có cabin phía trước và thùng chở hàng phía sau hoặc thân xe chuyên dụng chở hàng nặng. |
| `bus` | Xe buýt chở khách công cộng cỡ lớn, xe khách tuyến cố định. | Kích thước rất dài, có nhiều khoang cửa sổ hành khách dọc thân xe. |
| `van` | Xe tải van, xe khách nhỏ dưới 16 chỗ ngồi, dáng hình hộp. | Thân xe dài dạng hộp liền khối, nhỏ hơn bus nhưng lớn hơn car, chuyên chở hàng nhẹ hoặc khách trung chuyển. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Khung bounding box phải bao trọn phần thân chính của xe (từ mép trước đến mép sau, từ bánh xe đến nóc xe).
- Vật thể bị che khuất: Vẫn gán nhãn nếu nhìn rõ trên 50% cấu trúc chính của phương tiện và phân biệt được loại xe.
- Vật thể bị cắt bởi rìa ảnh: Vẫn gán nhãn phần xe xuất hiện trong khung hình nếu nhận diện được đặc trưng lớp.
- Vật thể rất nhỏ/mơ hồ: Bỏ qua không gán nhãn nếu xe ở quá xa, mờ nhòe hoặc diện tích quá nhỏ không phân biệt được thuộc lớp nào.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_012.jpg` | Xe bán tải (Pickup truck) có cabin chở người và thùng chở hàng hở phía sau. | `truck` | Có cấu trúc khung gầm và mục đích chở hàng chuyên dụng phía sau, xếp vào nhóm xe tải nhẹ. |
| `drive_025.jpg` | Xe khách trung chuyển 16 chỗ dáng thân dài hình hộp. | `van` | Kích thước nằm giữa ô tô con và xe buýt, dạng thân hộp phục vụ trung chuyển hành khách. |
| `drive_041.jpg` | Ô tô con bị che khuất khoảng 40% phần đầu xe bởi thân cây ven đường. | `car` | Tỷ lệ che khuất dưới 50%, vẫn nhận diện rõ phần thân và đuôi xe thuộc dòng ô tô con. |