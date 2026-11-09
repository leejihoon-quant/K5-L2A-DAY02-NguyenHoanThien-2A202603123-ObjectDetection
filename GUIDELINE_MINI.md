# Mini guideline — bài làm cá nhân

## Thứ tự ID lớp

Nhãn của bài này dùng thứ tự ID **của bộ gold** (đã được lab coach xác nhận là lệch so với bảng schema trong `README.md`):

| ID | Lớp |
| ---: | --- |
| 0 | `car` |
| 1 | `van` |
| 2 | `truck` |
| 3 | `bus` |

`data.yaml` đã được cập nhật theo thứ tự này. Bộ nhãn khóa ban đầu (commit đầu tiên) dùng thứ tự theo `README.md` (`car=0, truck=1, bus=2, van=3`); sau khi chấm gold, mình chỉ đổi số lớp ở đầu mỗi dòng cho khớp gold, không đổi vị trí hay số lượng box (xem báo cáo, mục 4).

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người cỡ nhỏ: sedan, hatchback, SUV, taxi, xe bán tải cabin nhỏ. | Thân thấp, có khoang hành khách và cốp riêng. Không phải xe buýt, xe tải hay minivan. |
| `van` | Xe van hoặc minivan chở hàng hoặc chở người. | Thân dạng hộp, nóc cao, cao hơn xe con nhưng nhỏ hơn xe buýt, không có cabin tách như xe tải. Ví dụ: van trắng nhiều cửa sổ bên hông, xe nhỏ thân hộp có giá nóc. |
| `truck` | Xe tải chở hàng và xe đầu kéo. | Có cabin tách khỏi thùng hàng, hoặc là đầu kéo kéo rơ-moóc, hoặc xe chuyên dụng như xe ben, xe bồn, xe cứu hộ. Bán tải cabin nhỏ thuộc `car`. |
| `bus` | Xe buýt, minibus chở khách. | Thân dài, cao, nhiều cửa sổ dọc thân; gồm xe buýt thường, xe buýt hai tầng, xe buýt nối khớp và minibus. |

## Quy tắc chung

- **Mỗi xe đúng một box.** Mỗi xe thuộc bốn lớp có một box riêng, kể cả cùng lớp. Nếu một xe có hai box khác lớp trùng nhau thì chỉ giữ box đúng lớp (đã rà và xóa các box trùng như vậy).
- **Box bám sát vật thể:** box ôm sát phần thân xe nhìn thấy được, không lấy thêm mặt đường, bầu trời hay xe bên cạnh. Với xe bị mờ do chuyển động, đặt cạnh box tại chỗ thân xe nhìn rõ, không lấy vệt mờ.
- **Vật thể bị che khuất:** vẫn vẽ box quanh phần nhìn thấy nếu còn nhận ra lớp. Hai xe chồng nhau thì vẽ hai box riêng trên phần nhìn thấy của từng xe.
- **Vật thể bị cắt bởi rìa ảnh:** kéo box đến đúng rìa ảnh, không đoán phần nằm ngoài ảnh.
- **Xe chở hàng chất cao:** gán lớp theo loại xe và bao cả phần hàng nằm trên thùng vào box.
- **Vật thể rất nhỏ/mơ hồ:** gán nếu còn nhận ra là xe và xác định được lớp; nếu không xác định chắc được lớp thì không đoán, ghi lại ảnh và hỏi giảng viên. Không đặt ngưỡng kích thước cố định, nên bộ nhãn có nhiều xe nhỏ ở xa (nhiều hơn bộ gold, và đây là một lý do precision chưa cao khi chấm gold).
- **Không gán:** mô tô/xe máy, xe ba bánh, xe đạp, người đi bộ, biển báo, đèn giao thông, phản chiếu trong kính/gương, hình xe trên bảng quảng cáo.
- **Xe đang đỗ** vẫn gán theo loại xe, vì lớp mô tả loại vật thể chứ không mô tả trạng thái.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | Phía sau xe SUV trắng đỗ bên đường có một khối trắng hình hộp chữ nhật, bị xe SUV và cột đèn che gần hết, chỉ thấy phần thùng, không thấy cabin hay bánh xe. | Vẽ một box `van` quanh phần nhìn thấy của khối trắng, tách biệt với box của xe SUV (`car`); ghi lại để hỏi giảng viên xác nhận. | Quy tắc "vật thể bị che vẫn gán box quanh phần nhìn thấy nếu nhận ra lớp"; khối hộp trắng chỉ thấy thùng, không thấy cabin tách riêng nên chọn `van` thay vì `truck`. Nếu giảng viên cho là không đủ để xác định lớp thì bỏ box này. |
| `drive_042` | Xe tải chở hàng chất rất cao (bao tải, hàng hóa) nhô lên trên thùng xe, che một phần xe buýt hai tầng phía sau. | Gán `truck`, và box bao cả phần hàng chất trên thùng. Xe buýt hai tầng phía sau vẫn có box `bus` riêng trên phần nhìn thấy. | Lớp mô tả loại xe, không mô tả trạng thái: xe tải chở hàng vẫn là `truck`. Hàng là một phần nhìn thấy của xe, nên bao vào box để box gắn với đúng một vật thể. Áp dụng thống nhất cho mọi xe chở hàng chất cao. |
| `drive_006` | Xe nhỏ màu xanh nhạt, thân hộp, nóc cao có giá nóc, nhìn giống xe minivan nhỏ; dễ nhầm với `car`. Ban đầu cùng một xe có hai box (`car` và `van`). | Gán `van` và chỉ giữ một box. | Schema xếp xe van/minivan chở hàng hoặc chở người vào `van`; thân hộp và cao hơn xe con thông thường (`car`). Quy tắc "mỗi xe một box". |
| `drive_024` | Xe ba bánh chở khách (tuk-tuk) ở giữa dưới ảnh, trông giống một phương tiện có mui. | Không gán box. | Không thuộc bốn lớp `car`, `van`, `truck`, `bus` (cùng nhóm với xe máy, xe ba bánh), nên không gán theo quy tắc "không gán vật thể ngoài schema". |

## Ghi chú quy trình

Nhãn nháp được tạo bằng model YOLO11m rồi rà soát và sửa thủ công trong CVAT (đổi lớp cho xe van/xe tải/xe buýt bị gán sai, xóa box thừa và box trùng, thêm xe bị sót). Chi tiết công cụ và phần tự kiểm tra nằm trong báo cáo cá nhân, mục 3 và mục 8.
