# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Văn Đại
- MSSV: 2A202604070
- Email: vdai20204@gmail.com
- Lớp: K5-L2B
- Link repo bài làm: https://github.com/Pennywise111124/K5-L2B-DAY02-NguyenVanDai-2A202604070-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/github/Pennywise111124/K5-L2B-DAY02-NguyenVanDai-2A202604070-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb (kết quả các lệnh chạy ở file day2_annotation_to_yolo.ipynb trên GitHub)

## 2. Schema và mini guideline

Chi tiết trong [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

Schema 4 lớp theo data.yaml: 0 car, 1 truck, 2 bus, 3 van. Quy tắc áp dụng chính: mỗi xe một box ôm sát phần nhìn thấy; xe bị che vẫn gán nếu nhận diện được lớp; xe bị cắt rìa thì box chạm rìa ảnh; không gán mô tô/xe máy, người, phản chiếu, hình xe trên bảng quảng cáo; ca van mơ hồ với bus hoặc car thì ghi lại và hỏi giảng viên, không tự đoán.

Ba ca mơ hồ đã quyết định:

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | Xe bị xe khác che một phần | Gán box quanh phần nhìn thấy của xe bị che, đúng lớp (đã hỏi lab coach) | GUIDE: bị che một phần vẫn gán nếu nhận diện được lớp. |
| `drive_004` | Quá nhiều xe tụ tập và không thấy rõ điểm ảnh | Không box cho lớp đó vì chỉ box những vật thể có thể nhìn thấy và xác định được lớp (đã hỏi lab coach) | GUIDE: không tự đoán nếu không xác định chắc lớp. |
| `drive_016` | Xe van/minibus mơ hồ giữa `van` và `car` | Chưa tự chọn, sau khi hỏi lab coach thì chốt theo câu trả lời: `van` | GUIDE: van/minivan mơ hồ với `bus` hoặc `car` thì ghi case ID và hỏi. |

## 3. Annotation và self-QC

- Số file nhãn train: 50 (dataset/labels/train/).
- Kết quả checker và lỗi đã xử lý: python tools/check_yolo_labels.py báo "ĐẠT cấu trúc: 50/50 file nhãn hợp lệ". Không có lỗi định dạng cần xử lý.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001.jpg, drive_003.jpg, drive_009.jpg, drive_015.jpg, drive_018.jpg, drive_025.jpg, drive_034.jpg, drive_041.jpg, drive_046.jpg, drive_048.jpg
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Với những vật thể nhìn thấy rõ mà chưa box sẽ box lại, có những vật ở quá xa sau khi hỏi lab coach thì sẽ xóa đi vì thấy không cần thiết, có vài trường hợp `car` và `van` khá giống nhau đã hỏi lab coach và đưa ra quyết định.
- Commit khóa nhãn ban đầu: 14c0dad ("Initial train labels and mini guideline").

## 4. Gold QC và rework

Chi tiết trong [annotation_evaluation.json](../../outputs/annotation_evaluation.json)

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 458 | không rework |
| FP | 442 | không rework |
| FN | 195 | không rework |
| Precision | 0.5089 | không rework |
| Recall | 0.7014 | không rework |
| F1 | 0.5898 | không rework |
| mean_iou_of_matches | 0.8516 | không rework |
| passed | 0.80 ≥ F1 ≥ 0.50, mean IoU ≥ 0.70 | không rework |

Nhận xét kết quả: mean IoU đạt (0.85), nghĩa là box vẽ sát; F1 thấp hơn ngưỡng 0.80 của script.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, GPU Tesla T4; Ultralytics 8.4.208, Python 3.13.16, torch 2.14.1+cu130.
- Các tham số thay đổi so với notebook nếu có và lý do: không thay đổi.
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt (không nộp file .pt).

Chi tiết trong [assets/eval_predictions/](assets/eval_predictions/), [assets/yolo26_runs/](assets/yolo26_runs/)

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4105 |
| Recall | 0.2303 |
| mAP50 | 0.2723 |
| mAP50–95 | 0.2017 |

Theo lớp (log vehicle_eval): car mAP50 0.739 (94 box), truck 0.239 (5 box), bus 0.008 (2 box), van 0.103 (12 box).

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
- Theo log đánh giá trên 10 ảnh eval, lớp `bus` có recall thấp nhất (0, trên 2 vật thể). Ảnh minh họa: `drive_052.jpg`. Trong [`val_batch0_labels.jpg`](assets/yolo26_runs/vehicle_eval/val_batch0_labels.jpg), xe buýt lớn ở giữa đường mang nhãn thật `van`, trong khi ở [`val_batch0_pred.jpg`](assets/yolo26_runs/vehicle_eval/val_batch0_pred.jpg) và ảnh [`drive_052.jpg`](assets/eval_predictions/visuals/drive_052.jpg) model dự đoán `bus` (độ tin cậy 0.51). Dự đoán trùng vị trí nhưng khác tên lớp nên bị tính là sai, góp phần làm recall của `van` thấp. Chiếc xe này nhìn bằng mắt là xe buýt, nên tên lớp trong nhãn eval có thể không khớp với cách hiểu `bus`/`van`.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
- Trường hợp này cho thấy model tìm đúng vật thể nhưng box lệch hoặc lỏng khi IoU yêu cầu cao hơn, nên cần kiểm độ sát của các cạnh box trong nhãn train (tightness).
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Lỗi liên quan nhiều nhất là nhầm lẫn giữa `bus`, `van`, `truck`. Dữ kiện: (a) theo log đánh giá, `bus` mAP50 chỉ 0.008 (2 vật thể) và `van` 0.103 (12 vật thể), thấp hơn nhiều so với `car` 0.739; (b) ở `drive_052`, xe buýt lớn mang nhãn thật `van` còn model dự đoán `bus` (0.51), cho thấy tên lớp trong nhãn eval có thể không khớp cách hiểu `bus`/`van`; (c) kết quả chấm gold có 442 FP và 195 FN nên phần phân lớp của nhãn train chưa nhất quán với nhãn tham chiếu.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
- Nếu train trên ảnh eval thì model đã nhìn thấy đáp án, mAP trên eval không còn phản ánh khả năng tổng quát hóa (rò rỉ dữ liệu). Trong notebook, 10 ảnh eval được dùng làm validation trong lúc train, để theo dõi mAP từng epoch và chọn best.pt, nên đây không phải test set độc lập; số liệu trên eval vẫn có thể hơi lạc quan.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Mới bắt đầu làm quen với cvat | Phải dùng thao tác thủ công ở các taskbar làm chậm tiến độ | Học cách sử dụng phím tắt | Đã thành thục hơn trong việc dán nhãn |
| Kết quả eval theo lớp | `bus` recall 0, `van` thấp; `drive_052` nhãn `van` trên xe buýt | So sánh `val_batch0_labels.jpg` với `val_batch0_pred.jpg` | Chưa xác định nguyên nhân; không sửa nhãn eval/gold |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

- **Định dạng YOLO**: đạt. 50 file nhãn, `check_yolo_labels.py` báo 50/50.
- **Đầy đủ vật thể**: một phần. Recall với gold là 0.70 (195 FN), nên còn sót hoặc sai lớp ở nhiều xe.
- **Độ bám sát box**: đạt. Mean IoU 0.85, trên mức 0.80 của rubric.
- **Phân lớp nhất quán**: chưa đạt. Nhiều box sai lớp so với gold, tập trung ở `bus`/`van`/`truck`; chưa xác định nguyên nhân.
- **Mini guideline**: đã định nghĩa lớp, quy tắc box và 3 ca mơ hồ có lý do.
- **Gold-set QC**: chưa đạt ngưỡng của rubric. F1 0.5898 < 0.80.
- **Pipeline YOLO**: đã chạy Colab, không dùng eval để train, có metrics, ảnh eval và phân tích lỗi.
- **Điều kiện bắt buộc**: đủ 50 file, qua checker, có gold evaluation, giữ đúng split.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- **Claude (Anthropic)**: soạn khung `GUIDELINE_MINI.md` rồi mình chỉnh theo ảnh thật và câu trả lời của lab coach.
