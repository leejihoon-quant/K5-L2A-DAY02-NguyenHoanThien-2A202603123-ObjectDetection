# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người cỡ nhỏ: sedan, SUV, taxi, xe bán tải/cabin nhỏ. | Thân thấp, có khoang hành khách và cốp riêng. Không phải xe buýt, xe tải hay minivan. |
| `truck` | Xe tải chở hàng và xe đầu kéo. | Có cabin tách khỏi thùng hàng, hoặc là đầu kéo kéo rơ-moóc. Bán tải cabin nhỏ không thuộc lớp này mà thuộc `car`. |
| `bus` | Xe buýt, minibus chở khách. | Thân dài, cao, nhiều cửa sổ dọc thân. Lớn hơn `van` rõ rệt. |
| `van` | Xe van hoặc minivan chở hàng hoặc chở người. | Thân dạng hộp, cao hơn `car`, nhỏ hơn `bus`, không có cabin tách như `truck`. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: box ôm sát phần thân xe nhìn thấy được; cạnh box chạm mép thân xe, không lấy thêm mặt đường, bầu trời hay xe bên cạnh. Mỗi xe một box riêng, kể cả cùng lớp.
- Vật thể bị che khuất: vẫn vẽ box quanh phần nhìn thấy nếu còn đủ để nhận ra lớp. Hai xe chồng lên nhau thì vẽ hai box riêng trên phần nhìn thấy của từng xe.
- Vật thể bị cắt bởi rìa ảnh: kéo box đến đúng rìa ảnh, không đoán phần nằm ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: nếu không xác định chắc lớp thì không tự đoán; ghi tên ảnh vào bảng ca mơ hồ và hỏi giảng viên.
- Không gán (ngoài phạm vi bốn lớp): mô tô/xe máy, người đi bộ, xe đạp, biển báo, đèn giao thông, phản chiếu trong kính/gương, hình xe trên bảng quảng cáo.
- Xe đang đỗ vẫn gán theo loại xe, vì lớp mô tả loại vật thể chứ không mô tả trạng thái.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | Phía sau xe SUV trắng đỗ bên đường có một khối trắng hình hộp chữ nhật, bị xe SUV và cột đèn che gần hết, chỉ thấy phần thùng, không thấy cabin hay bánh xe. | Vẽ một box `truck` quanh phần nhìn thấy của khối trắng, riêng biệt với box của xe SUV; đã ghi lại để hỏi giảng viên xác nhận. | Quy tắc "vật thể bị che vẫn gán box quanh phần nhìn thấy nếu nhận diện được lớp"; thùng hộp lớn phù hợp với xe tải chở hàng (`truck`). Nếu giảng viên cho là không đủ để xác định lớp thì bỏ box này. |
| `drive_042` | Xe tải chở hàng chất rất cao (bao tải, hàng hóa) nhô lên trên thùng xe, che một phần xe buýt hai tầng phía sau. | Gán `truck`, và box bao cả phần hàng chất trên thùng. Xe buýt hai tầng phía sau vẫn có box `bus` riêng trên phần nhìn thấy. | Lớp mô tả loại xe, không mô tả trạng thái: xe tải chở hàng vẫn là `truck`. Hàng là một phần nhìn thấy của xe, nên bao vào box để box gắn với đúng một vật thể. Áp dụng thống nhất cho mọi xe chở hàng chất cao. |
| `drive_006` | Xe nhỏ màu xanh nhạt, thân hộp, nóc cao có giá nóc, nhìn giống xe minivan nhỏ; dễ nhầm với `car`. | Gán `van`. | Schema xếp xe van/minivan chở hàng hoặc chở người vào `van`, thân hộp và cao hơn xe con thông thường (`car`). |
| `drive_024` | Xe ba bánh chở khách (tuk-tuk) ở giữa dưới ảnh, trông giống một phương tiện có mui. | Không gán box. | Không thuộc bốn lớp `car`, `truck`, `bus`, `van` (cùng nhóm với xe máy, xe ba bánh), nên không gán theo quy tắc "không gán vật thể ngoài schema". |
