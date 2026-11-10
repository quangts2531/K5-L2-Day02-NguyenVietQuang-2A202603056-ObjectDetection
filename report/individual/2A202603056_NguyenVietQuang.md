# Báo cáo cá nhân — Ngày 2 Object Detection

Xem [quy định nộp](../../SUBMISSION.md). Các mục chưa có kết quả được ghi rõ là chưa thực hiện hoặc chưa cung cấp; cần hoàn thiện trước khi nộp.

## 1. Thông tin học viên

- Họ tên: Nguyễn Viết Quang
- MSSV: 2A202603056
- Email: nguyenvietquang25032001@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/quangts2531/K5-L2-Day02-NguyenVietQuang-2A202603056-ObjectDetection
- Link Colab đã chạy: https://drive.google.com/file/d/1jErOqHqpvcHuKFGeF3VH64W2GcG5TtrN/view?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Áp dụng ID `car=0`, `truck=1`, `bus=2`, `van=3`. Ba tình huống cần chú ý là xe buýt bị che khuất ở `drive_009`, phân biệt van với car ở `drive_028`, và phân biệt truck với car ở `drive_042`. Quyết định cuối cùng cho các tình huống này cần được xác nhận khi rà ảnh gốc; nếu vẫn không phân biệt chắc lớp thì hỏi giảng viên.

## 3. Annotation và self-QC

- Số file nhãn train: 50.
- Kết quả checker và lỗi đã xử lý: checker báo đạt cấu trúc 50/50 file; chưa ghi nhận lỗi cấu trúc.
- 10 ảnh chọn để rà lại: `drive_009`, `drive_018`, `drive_023`, `drive_028`, `drive_030`, `drive_031`, `drive_037`, `drive_042`, `drive_045`, `drive_049`. Đối chiếu AI sơ bộ với gold ở IoU ≥ 0.50; học viên cần xác nhận bằng mắt trước khi coi là self-QC hoàn tất.
- Class lệch sơ bộ (label hiện tại → gold): `drive_009`: 5 `bus→van`, 1 `car→bus`, 1 `car→van`, 1 `truck→van`, 1 `van→truck`; `drive_018`: 1 `van→truck`; `drive_023`: 3 `bus→car`; `drive_030`: 1 `bus→van`, 2 `van→truck`, 1 `van→car`; `drive_031`: 1 `van→truck`; `drive_037`: 6 `bus→car`, 1 `truck→bus`; `drive_042`: 2 `bus→van`, 1 `truck→bus`. Không thấy class lệch ở các box ghép được của `drive_028`, `drive_045`, `drive_049`.
- Box không ghép được về vị trí với gold ở IoU ≥ 0.50 (cần mở ảnh để xác định nguyên nhân): `drive_009`: 13 student / 6 gold; `drive_018`: 8 / 0; `drive_023`: 0 / 1; `drive_028`: 12 / 0; `drive_030`: 27 / 3; `drive_031`: 20 / 0; `drive_037`: 2 / 0; `drive_042`: 4 / 0; `drive_045`: 22 / 3; `drive_049`: 22 / 0. Đây là box chưa ghép theo vị trí, không tự kết luận tất cả là nhãn thừa hay bỏ sót.
- Commit khóa nhãn ban đầu: chưa ghi nhận mã commit.

## 4. Gold QC và rework

| Chỉ số | Lần đánh giá hiện tại | Sau sửa nếu có |
|---|---:|---:|
| TP | 438 | Chưa chạy lại |
| FP | 569 | Chưa chạy lại |
| FN | 215 | Chưa chạy lại |
| Precision | 0.4350 | Chưa chạy lại |
| Recall | 0.6708 | Chưa chạy lại |
| F1 | 0.5277 | Chưa chạy lại |
| mean_iou_of_matches | 0.8493 | Chưa chạy lại |
| `passed` theo evaluator (ngưỡng mặc định F1 ≥ 0.80) | false | Chưa chạy lại |

Dẫn tới [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json) nếu file kết quả đã được lưu trong repo. Theo ngưỡng F1 > 0.50 mà học viên cho biết được áp dụng cho bài này, F1 = 0.5277 đạt; mean IoU = 0.8493 cũng đạt ngưỡng 0.70. Script hiện báo `passed: false` vì ngưỡng F1 mặc định là 0.80. Chưa có số liệu rework sau lần đánh giá này.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, Tesla T4; Ultralytics 8.4.208, Python 3.13.16, Torch 2.14.1+cu130.
- Các tham số thay đổi so với notebook nếu có và lý do: không thấy thay đổi; YOLO26n, 50 epochs, imgsz=640, batch=-1, seed=20260909.
- Đường dẫn best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4105 |
| Recall | 0.2303 |
| mAP50 | 0.2723 |
| mAP50–95 | 0.2017 |

Notebook cho biết đã tạo `outputs/day2_colab_outputs.zip` trong phiên Colab, gồm ảnh dự đoán và log train/eval. Hai folder `assets/eval_predictions/` và `assets/yolo26_runs/` hiện chưa có trong workspace cạnh báo cáo; cần tải/giải nén ZIP và thêm hai folder này để hoàn tất minh chứng nộp bài.

Trả lời bốn câu hỏi cuối notebook:

1. `bus` có recall thấp nhất (0.000). Notebook ghi nhận dự đoán bus ở `drive_052` và `drive_053`, nhưng không khớp được instance bus trong ground truth; cần xem ảnh prediction để mô tả lỗi cụ thể.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm độ khít của box và vị trí các cạnh ở các ngưỡng IoU chặt hơn.
3. Recall theo lớp là `car` 0.638, `truck` 0.200, `bus` 0.000 và `van` 0.0833. Điều này cho thấy các lớp ít mẫu nhận diện kém hơn trong lần chạy này; cần đối chiếu ảnh dự đoán với guideline trước khi kết luận có lỗi phân lớp cụ thể.
4. Không dùng 10 ảnh eval để train để giữ split đánh giá; trong notebook chúng được dùng làm validation để đánh giá/chọn checkpoint tốt nhất, nên không phải test độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Gold QC | F1 = 0.5277; vượt ngưỡng > 0.50 học viên cho biết áp dụng; script mặc định báo `passed=false` với ngưỡng 0.80 | Ghi kết quả vào báo cáo | Chưa có lần rework/evaluator sau sửa |
| YOLO eval | Recall thấp nhất ở bus; mAP50 = 0.2723 | Đã train/evaluate trên 10 ảnh eval | Cần thêm prediction và log vào `report/individual/assets/` |

## 7. Tự đánh giá theo rubric

Đã có 50 file nhãn, checker đạt cấu trúc, một lần gold QC và kết quả train/eval YOLO. Theo ngưỡng > 0.50 học viên cho biết áp dụng, annotation đạt; JSON vẫn ghi `passed=false` do ngưỡng mặc định trong script là 0.80. Còn thiếu danh sách ảnh self-QC bằng mắt, mã commit khóa nhãn ban đầu và hai folder minh chứng model trong assets.

## 8. Công cụ AI và tài liệu tham khảo

Đã dùng ChatGPT để hỗ trợ đọc kết quả evaluator, đối chiếu sơ bộ 10 ảnh với gold và cập nhật báo cáo; học viên sẽ tự xem ảnh/nhãn để xác nhận class lệch, box chưa ghép và quyết định sửa. Nguồn ngoài repo: chưa ghi nhận.
