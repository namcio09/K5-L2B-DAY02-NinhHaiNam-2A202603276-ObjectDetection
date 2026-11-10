# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Ninh Hai Nam
- MSSV:2A202603276
- Email:26ai.namnh10@vinuni.edu.vn
- Lớp:2B-LAB-E402
- Link repo bài làm: https://github.com/namcio09/K5-L2B-DAY02-NinhHaiNam-2A202603276-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/github/namcio09/K5-L2B-DAY02-NinhHaiNam-2A202603276-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb#scrollTo=6jTOpLvEsQYV

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train:50
- Kết quả checker và lỗi đã xử lý: Đạt 50/50 file nhãn hợp lệ, không có lỗi
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:drive_010 -> drive_020
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Khi rà lại còn thiếu 1 vài đối tượng car nhỏ nhưng vẫn có thể nhận diện được. Đã bổ sung thêm bounding box và thực hiện export lại file train từ cvat
- Commit khóa nhãn ban đầu:b093396

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |454|458|
| FP |521 |390 |
| FN |199 |195 |
| Precision |0.4656 |0.5401 |
| Recall |0.6953 |0.7014 |
| F1 |0.5577 |0.6103 |
| mean_iou_of_matches |0.8529 |0.8523 |
| passed |false |false |
- Chú thích: Đã loại bỏ box thừa (chủ yếu là xe đỗ ven đường/bãi ngoài làn lưu thông mà gold không gán), không đổi class, không thêm box, không sửa gold. F1 tăng 0.5577 → 0.6103. Kết quả vẫn `false` vì gold lệch guideline (đã được Lab Coach xác nhận và cho phép bỏ qua bước đánh giá gold)

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Colab, GPU Tesla T4 (14.9 GB), Ultralytics 8.4.208, Torch 2.14.1+cu130, Python 3.13.16 
- Các tham số thay đổi so với notebook nếu có và lý do: Không thay đổi, epochs=50, imgsz=640, model yolo26n
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.3156 |
| Recall |0.3066 |
| mAP50 |0.2740 |
| mAP50–95 |0.2006 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
class bus có recall = 0 và mAP50 xấp xỉ bằng 0, thấp nhất trong bốn lớp, Bus chỉ có 2 instance trong 10 ảnh eval, nằm ở `drive_051` và `drive_058` — model không phát hiện được chiếc nào có lẽ do trong 50 ảnh train, bus xuất hiện rất ít
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
mAP50 chỉ cần IoU ≥ 0.50 nên một box đúng chỗ nhưng hơi lỏng vẫn được tính, mAP50–95 đòi hỏi IoU cao ở nhiều ngưỡng nên tụt mạnh. Vì vậy khi mAP50 cao mà mAP50–95 thấp, cần kiểm: box có bám sát thân xe không (có lấn mặt đường/trời), có gộp hai xe thành một, có cắt mất phần đuôi/đầu xe ở xe bị che hoặc cắt khung
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Nhầm lẫn `van`/`bus`/`car` là khả năng cao nhất
- Dữ kiện hỗ trợ:
	+ `van` chỉ đạt mAP50=0.115
	+ `bus`=0.008
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Nếu train trên eval thì model đã "thấy đáp án", metrics sẽ cao giả tạo và không còn đo được khả năng tổng quát. Trong notebook, 10 ảnh eval đóng vai trò validation dùng trong quá trình train

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|Bước đầu khi thực hiện gấn nhãn|khó khăn khi xác định các class|Hỏi đáp với lab coach|none|
|Khi nhận gold file | Thấy gold file đang label bị nhầm 1 vài đối tượng | đối chiếu với xác nhận của lab coach | none |

## 7. Tự đánh giá theo rubric
- `Gold-set annotation QC` không đạt ngưỡng F1≥0.80 vì gold lệch guideline.
- đã chạy check_yolo_labels.py đạt 50/50
- Có file `outputs/annotation_evaluation.json`
- Không dùng eval để train, không sửa gold
## 8. Công cụ AI và tài liệu tham khảo
- ChatGPT: thực hiện research các thuật ngữ khái niệm
- CommandCode: hỗ trợ quá trình thực hiện các bước theo hướng dẫn
