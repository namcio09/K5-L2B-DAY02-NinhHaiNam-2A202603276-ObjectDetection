# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Các loại xe con bao gồm: sedan, hatchback, SUV, taxi, xe bán tải | kích thước nhỏ hoặc trung bình, thân xe kín có 4 cửa, xe bán tải có thùng hở nhỏ đằng sau |
| `truck` |Xe tải chở hang, xe đầu kéo, xe ben | có thùng chở hàng lớn và gầm xe cao, cabin xe tách rời thùng |
| `bus` |Xe bus, minibus chở khách | xe dài nhiều cửa sổ lớn, thân xe cao, có biển tuyến xe |
| `van` |Xe van, minivan chở hàng hoặc chở người | thân xe kín, cửa trượt ở đằng sau, cao hơn car nhưng ngắn hơn truck |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: box ôm sát cả phần xe nhìn thấy được, không lấy thêm các thành phần bên ngoài khác
- Vật thể bị che khuất: nếu nhận diện được vật thể, vẽ box quanh phần nhìn thấy, nếu bị che nhiều không chắc thuộc lớp nào thì bỏ qua
- Vật thể bị cắt bởi rìa ảnh: vẽ box chạm đúng rìa, không đoán phần ngoài khung
- Vật thể rất nhỏ/mơ hồ: Không tự đoán lớp, bỏ qua nếu không xác định được 

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` |Xe van đằng sau cột điện và car bị che hầu hết chỉ lộ phần thân xe | vẽ box cắt phần thân trên xe car che phía trước | vì vẫn có thể nhìn thấy phần thân cao của xe van nên không bỏ qua được và cần cắt xe car để bảo đảm box bao đủ xe van |
| `drive_005` |Xe đen nhỏ ở phía sau xe van bên trái phía trên của ảnh, ảnh quá mờ không thấy rõ | không gán label cho xe đó | Vì ảnh mơ hồ không xác định được |
| `drive_037` |Có 1 loại xe giống xe tải nhưng không phải | Không đánh label cho phương tiện đó | vì trong guidline không đề cập tới |
