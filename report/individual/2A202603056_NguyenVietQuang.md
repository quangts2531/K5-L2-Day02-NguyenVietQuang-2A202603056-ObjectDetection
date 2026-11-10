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
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: chưa ghi nhận; cần tự rà và điền danh sách.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: chưa ghi nhận kết quả rà bằng mắt.
- Commit khóa nhãn ban đầu: chưa ghi nhận mã commit.

## 4. Gold QC và rework

| Chỉ số | Lần đánh giá hiện tại (trước rework) | Sau sửa nếu có |
|---|---:|---:|
| TP | 438 | Chưa chạy lại |
| FP | 569 | Chưa chạy lại |
| FN | 215 | Chưa chạy lại |
| Precision | 0.4350 | Chưa chạy lại |
| Recall | 0.6708 | Chưa chạy lại |
| F1 | 0.5277 | Chưa chạy lại (ngưỡng ≥ 0.80) |
| mean_iou_of_matches | 0.8493 | Chưa chạy lại (ngưỡng ≥ 0.70) |
| passed | false | Chưa chạy lại |

Dẫn tới [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json) nếu file kết quả đã được lưu trong repo. Lần hiện tại chưa đạt do F1 dưới ngưỡng; mean IoU của các box ghép đạt ngưỡng. Chưa có rework được ghi nhận. Ưu tiên rà từng box trên `drive_018`, `drive_023`, `drive_028`, `drive_030`, `drive_031`, `drive_037`, `drive_045` và `drive_049`, rồi ghi quyết định sửa/giữ và lý do. Không thể suy ra nguyên nhân cụ thể chỉ từ FP/FN tổng hợp.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: chưa ghi nhận.
- Các tham số thay đổi so với notebook nếu có và lý do: chưa ghi nhận.
- Đường dẫn best checkpoint trong phiên chạy: chưa ghi nhận.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | Chưa ghi nhận |
| Recall | Chưa ghi nhận |
| mAP50 | Chưa ghi nhận |
| mAP50–95 | Chưa ghi nhận |

Chưa ghi nhận hai folder `assets/eval_predictions/` và `assets/yolo26_runs/` hoặc kết quả model. Sau khi train/evaluate, đặt cả hai folder cạnh báo cáo, dẫn link tương đối, điền số đo thực tế và phân tích ít nhất một ảnh cùng vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Chưa trả lời; cần dựa trên recall theo lớp và ảnh dự đoán từ lần chạy thực tế.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm độ khít và tọa độ các cạnh box ở nhiều ngưỡng IoU.
3. Chưa kết luận; cần đối chiếu lỗi phân lớp trong ảnh dự đoán với guideline và dữ kiện cụ thể.
4. Không dùng 10 ảnh eval để train để giữ split đánh giá; trong notebook chúng được dùng làm validation để đánh giá/chọn checkpoint tốt nhất, nên không phải test độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Gold QC | F1 = 0.5277, dưới ngưỡng yêu cầu | Đã ghi các ảnh ưu tiên rà trong guideline | Chưa rework/chạy lại evaluator |

## 7. Tự đánh giá theo rubric

Đã có 50 file nhãn, checker đạt cấu trúc và có một lần gold QC. Gold QC chưa đạt F1 yêu cầu; cần rework và đánh giá lại. Chưa ghi nhận self-QC bằng mắt, commit khóa nhãn ban đầu hoặc kết quả train/eval YOLO.

## 8. Công cụ AI và tài liệu tham khảo

Đã dùng ChatGPT để hỗ trợ đọc kết quả evaluator, cập nhật guideline và soạn báo cáo; cần tự đối chiếu các nhãn với ảnh/gold và chịu trách nhiệm về quyết định sửa. Nguồn ngoài repo: chưa ghi nhận.
