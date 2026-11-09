# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Hoàn Thiện 
- MSSV: 2A202603123
- Email: nht270605@gmail.com
- Lớp: 2A
- Link repo bài làm: https://github.com/leejihoon-quant/K5-L2-Day02-Nguy-n-Ho-n-Thi-n-2A202603123-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1UWQ56MvdaaFyM3aNgGit0MWdXoREek87



Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 2. Schema và mini guideline

Xem [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

- **Thứ tự ID lớp:** nhãn dùng thứ tự của bộ gold (`car=0, van=1, truck=2, bus=3`), đã được lab coach xác nhận là lệch so với bảng schema trong `README.md` (`car=0, truck=1, bus=2, van=3`). `data.yaml` đã cập nhật theo thứ tự này.
- **Bốn ca mơ hồ đã quyết định:**
  - `drive_001`: khối trắng hình hộp bị xe SUV và cột đèn che gần hết, chỉ thấy phần thùng, gán `van` quanh phần nhìn thấy.
  - `drive_042`: xe tải chở hàng chất rất cao, gán `truck` và bao cả phần hàng vào box.
  - `drive_006`: xe nhỏ thân hộp có giá nóc, gán `van`.
  - `drive_024`: xe ba bánh chở khách, không gán vì ngoài schema.
- **Phần cần giảng viên làm rõ:** `drive_001` (gán `van`, `truck` hay không gán), và quy tắc về xe nhỏ ở xa (gold dường như gán ít xe nhỏ hơn bộ nhãn của mình; xem mục 4).

## 3. Annotation và self-QC

- Số file nhãn train: 50 (`dataset/labels/train/drive_001.txt` đến `drive_050.txt`), tổng 1165 box sau rework: `car` 1004, `van` 40, `truck` 25, `bus` 96.
- Kết quả checker: `python tools/check_yolo_labels.py` chạy trên bản clone của repo (commit `1cb208e`) cho kết quả **"ĐẠT cấu trúc: 50/50 file nhãn hợp lệ"**.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: [ `drive_001`, `drive_006`, `drive_009`, `drive_012`, `drive_024`, `drive_030`, `drive_039`, `drive_042`, `drive_044`, `drive_049`  `drive_014` `drive_027`]
- Lỗi phát hiện khi rà và cách sửa:
  - Nhãn nháp được tạo bằng model YOLO11m đã huấn luyện sẵn trên COCO (1217 box: 1052 `car`, 61 `truck`, 104 `bus`, 0 `van`), rồi mình rà và sửa trong CVAT.
  - Model không có lớp `van`, nên nhiều xe van bị gán `car`, `truck` hoặc `bus`; đã đổi lại sang `van`.
  - Một số xe con bị gán `truck`, xe tải bị gán `bus`, xe buýt bị gán `truck`; đã sửa lớp.
  - Box thừa trên mái che bến xe buýt, lan can, ống cống, thùng rác và xe ba bánh; đã xóa.
  - Một xe bị sót ở `drive_042` (xe buýt lớn ở góc dưới trái); đã thêm box.
  - Sau khi chấm gold, phát hiện thêm 27 xe bị gán hai box với hai lớp khác nhau; đã xem từng cặp và giữ box đúng lớp (xem mục 4).
- Commit khóa nhãn ban đầu: `deac124` ("Add files via upload", bộ nhãn 50 file). Lưu ý: commit này đặt nhầm 50 file vào thư mục lồng `dataset/labels/train/train/`; đã sửa ở commit `b3f58b9` (xóa thư mục lồng) và commit `db087ed` (upload lại đúng chỗ, kèm rework).

## 4. Gold QC và rework

| Chỉ số | Trước sửa (nhãn khóa ban đầu) | Sau sửa |
|---|---:|---:|
| TP | 479 | 573 |
| FP | 713 | 592 |
| FN | 174 | 80 |
| Precision | 0,4018 | 0,4918 |
| Recall | 0,7335 | 0,8775 |
| F1 | 0,5192 | 0,6304 |
| mean_iou_of_matches | 0,8447 | 0,8490 |
| passed | false | false |

Kết quả sau sửa nằm trong `outputs/annotation_evaluation.json`. `passed` vẫn là `false` so với ngưỡng gốc F1 ≥ 0,80, nhưng lab coach đã xác nhận F1 > 0,5 là chấp nhận được cho bài này. 

### Rework đã thực hiện

1. **Đổi thứ tự ID lớp theo gold.**
   - *Lỗi quan sát:* F1 thấp dù mean IoU cao (0,845). Khi ghép box theo vị trí (IoU ≥ 0,5, bỏ qua lớp), xe buýt của mình (ID 2) khớp gold ID 3 (52/53 box), xe tải (ID 1) khớp gold ID 2 (18/19), xe van (ID 3) khớp gold ID 1 (24/28), `car` khớp ID 0 (475/495).
   - *Kiểm chứng độc lập:* nhãn `eval` có sẵn trong repo cũng theo thứ tự này (ví dụ `drive_052` và `drive_058`: xe buýt mang ID 3; `drive_051`: xe tải thùng mang ID 2).
   - *Quyết định:* đổi số lớp ở đầu mỗi dòng nhãn và `data.yaml` theo gold, sau khi lab coach xác nhận có lệch ID. Không đổi vị trí hay số lượng box. Riêng bước này: F1 0,519 → 0,623 (TP 575, FP 617, FN 78).
2. **Xóa 27 box trùng khác lớp.** Cùng một xe có hai box (ví dụ `drive_006` có cả `car` và `van`); xem từng cặp trên ảnh, giữ box đúng lớp. F1 0,623 → 0,630.
3. **Quyết định không áp dụng:** thử bỏ các xe nhỏ hơn 32×32 px cho F1 0,571 (hoặc 0,705 nếu kết hợp đổi ID), nhưng recall giảm vì mất khoảng 430 xe thật. Mình giữ nguyên các xe nhỏ để bộ nhãn đầy đủ.

### Nguyên nhân F1 chưa cao (không suy chỉ từ FP/FN)

- 570 box của mình không có xe tương ứng trong gold; diện tích trung vị khoảng 28×28 px, tức là chủ yếu xe nhỏ ở xa. Mình giả thuyết gold gán ít xe nhỏ hơn bộ nhãn nháp từ model, nhưng **chưa có xác nhận** quy tắc này từ giảng viên.
- 39 box đúng vị trí nhưng sai lớp (ví dụ 19 xe van mình gán `car`).
- 48 xe gold bị sót hoàn toàn.

## 5. Kết quả YOLO

- Runtime: Google Colab, GPU Tesla T4; Python 3.13.16; Ultralytics 8.4.208; torch 2.14.1+cu130.
- Tham số thay đổi so với notebook: không đổi (`yolo26n.pt`, 50 epoch, `imgsz=640`, batch tự động = 44, `seed=20260909`). Chỉ điền `REPO_URL` trong phiên Colab. `data.yaml` trong repo đã đổi thứ tự lớp theo gold.
- Best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt` (không nộp file `.pt`).

| Chỉ số trên 10 ảnh eval (113 xe) | Giá trị |
|---|---:|
| Precision | 0,697 |
| Recall | 0,371 |
| mAP50 | 0,531 |
| mAP50–95 | 0,350 |

| Lớp | Số xe eval | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|---:|
| `car` | 94 | 0,569 | 0,840 | 0,772 | 0,513 |
| `van` | 5 | 0,360 | 0,144 | 0,190 | 0,108 |
| `truck` | 2 | 1,000 | 0,000 | 0,495 | 0,396 |
| `bus` | 12 | 0,859 | 0,500 | 0,667 | 0,385 |

Tài liệu đính kèm (đặt cạnh báo cáo):
- Ảnh dự đoán 10 ảnh eval: [assets/eval_predictions/visuals/](assets/eval_predictions/visuals/)
- Ảnh cụ thể: [assets/eval_predictions/visuals/drive_051.jpg](assets/eval_predictions/visuals/drive_051.jpg)
- Confusion matrix: [assets/yolo26_runs/vehicle_eval/confusion_matrix.png](assets/yolo26_runs/vehicle_eval/confusion_matrix.png)
- Log train theo epoch: [assets/yolo26_runs/vehicle_annotation/results.csv](assets/yolo26_runs/vehicle_annotation/results.csv)

Lưu ý khi đọc kết quả: 10 ảnh eval chỉ có 2 xe `truck` và 5 xe `van`, nên các chỉ số theo lớp này rất nhiễu. Eval cũng được dùng làm validation khi train và chọn `best.pt`, nên không phải test set độc lập.

### Trả lời bốn câu hỏi cuối notebook

1. **Lớp nào recall thấp nhất? Ảnh minh họa?** `truck` có recall 0,00 (chỉ 2 xe eval, ở `drive_051` và `drive_058`), tiếp theo là `van` 0,144 (5 xe). Theo confusion matrix, cả 5 xe `van` bị dự đoán thành `car` và 1/2 xe `truck` bị dự đoán thành `car`, 1 xe bị sót. Ví dụ `drive_051`: xe tải thùng trắng ở mép phải trên (gold có nhãn `truck`) không được dự đoán là `truck`, và model dự đoán nhầm một `bus` (độ tin cậy 0,47) ở quầy hàng phía trên bên trái. [ĐIỀN: mở ảnh `drive_051.jpg` và xác nhận lại mô tả này bằng mắt]
2. **mAP50 cao nhưng mAP50–95 thấp thì box gặp vấn đề gì?** Ở đây mAP50 = 0,531 nhưng mAP50–95 = 0,350, nghĩa là model tìm đúng xe nhưng box chưa đủ sát ở các ngưỡng IoU cao. Cần kiểm tra độ sát của box: xe mờ do chuyển động (khó xác định mép), xe nhỏ ở xa (lệch vài pixel làm IoU giảm nhiều), cách xử lý xe bị che, và sự khác nhau giữa cách gán nhãn của mình với gold (ví dụ ở `drive_051` model dự đoán 18 `car` trong khi gold chỉ có 8 `car`, vì mình gán nhiều xe nhỏ ở xa hơn gold; điều này kéo precision `car` xuống 0,569).
3. **Lỗi `truck`/`bus`/`van`/`car` nào liên quan guideline?** Có. `van` bị nhầm với `car` vì xe van nhỏ khó phân biệt (guideline mô tả `van` là thân hộp, cao hơn `car`, nhưng với xe nhỏ ở xa tiêu chí này khó áp dụng), và `truck` dễ nhầm với `bus`/`car` ở xe thùng. Dữ kiện hỗ trợ: recall `van` 0,144, `truck` 0,00; confusion matrix; và dữ liệu train mất cân bằng nặng (`car` 1004, `bus` 96, `van` 40, `truck` 25 box). Cải thiện: thêm ví dụ `van` và `truck` cho guideline, bổ sung ảnh có nhiều xe hiếm, làm rõ ngưỡng kích thước.
4. **Vì sao không dùng 10 ảnh eval để train?** Eval dùng để đo khả năng tổng quát hóa trên ảnh model chưa từng thấy. Nếu train trên eval thì mAP bị thổi phồng và không còn phản ánh chất lượng thật. Trong notebook này eval còn đóng vai trò validation để chọn `best.pt`, nên giữ nguyên split và không dùng eval để train.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Upload nhãn lên GitHub | 50 file nằm sai chỗ trong `dataset/labels/train/train/`, script checker không thấy | Xóa thư mục lồng bằng "Delete directory" trên nhánh `main`, upload lại đúng chỗ | Đã sửa (commit `b3f58b9`, `db087ed`). Nhánh thật là `main`; `deac124` chỉ là mã rút gọn của commit nên trang xem chỉ đọc |
| Tạo nhãn nháp | Cài PyTorch bằng pip thất bại 3 lần do mạng đứt (tải 12–60 MB trên 127 MB) | Tải file `.whl` chính thức từ `download.pytorch.org`, kiểm tra SHA256, cài từ file | Đã cài được và chạy YOLO11m |
| Chấm gold | F1 0,519 dù mean IoU 0,845 | Phân tích ma trận nhầm lớp, so với nhãn `eval`, hỏi lab coach | Phát hiện lệch ID, đã xác nhận và rework (mục 4) |
| Chấm gold | 27 xe bị gán hai box khác lớp | Xem từng cặp trên ảnh, xóa box sai lớp | Đã xử lý; F1 tăng nhẹ lên 0,630 |
| Train trên Colab | Colab báo sự cố cấp runtime; tải file zip từ bảng Files báo "Failed to fetch" hai lần | Thử lại bằng trình duyệt của mình | Tải được sau đó |
| Chấm gold | 570 box xe nhỏ ở xa không có trong gold | Thử bỏ xe nhỏ hơn 32×32 px | Không áp dụng vì mất xe thật; còn chờ giảng viên làm rõ quy tắc xe nhỏ |

## 7. Tự đánh giá theo rubric

- **Hoàn thành và định dạng YOLO:** có 50 file nhãn, checker đạt 50/50.
- **Đầy đủ vật thể / bám sát box:** đã rà và sửa nhãn nháp, nhưng còn nhiều xe nhỏ ở xa mà mình gán còn gold không gán. Độ sát box tốt theo mean IoU 0,849, nhưng mình chưa tự kiểm tra từng box.
- **Phân lớp nhất quán:** `van`/`car` và `truck`/`bus` vẫn là điểm yếu (xem mục 5).
- **Mini guideline và ca mơ hồ:** có định nghĩa lớp, quy tắc và bốn ca có lý do.
- **Gold QC:** F1 = 0,630 và mean IoU = 0,849. Mean IoU đạt ngưỡng 0,70; F1 chưa đạt 0,80 nhưng đạt ngưỡng 0,5 theo xác nhận của lab coach, kèm vòng rework được giải thích ở mục 4.
- **Pipeline YOLO và phân tích:** Colab đã chạy, không dùng eval để train, có metrics và ảnh dự đoán. Các chỉ số theo lớp nhiễu vì eval chỉ có 2 xe `truck` và 5 xe `van`.
- **Chưa kiểm được:** 

## 8. Công cụ AI và tài liệu tham khảo

- **Claude (Claude Code):**  hướng dẫn set up, chạy model YOLO11m đã huấn luyện sẵn trên COCO để tạo nhãn nháp rồi đóng gói để nhập vào CVAT; phân tích kết quả chấm gold (phát hiện lệch ID, box trùng khác lớp) .
- **Phần mình tự kiểm tra:** rà soát lại từng ảnh sau khi chạy model YOLO11m, kiểm tra, đối chiếu phân tích của claude với dữ liệu thực tế, từ đó đưa ra giả thuyết ID của gold đang bị lệch và rework
- **Công cụ khác:** CVAT chạy local (Docker) để gán nhãn và xuất YOLO 1.1; thư viện Ultralytics (YOLO11m cho nhãn nháp, YOLO26n cho bài train); Google Colab; GitHub.
- **Nguồn ngoài repo:** tài liệu của Ultralytics và CVAT; trang tải PyTorch `download.pytorch.org`. Bộ ảnh theo README thuộc tập con UA-DETRAC (CC-BY-4.0).
- Không đính kèm gold train và không ghi mật khẩu/token trong báo cáo.
