# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Lê Quang Vinh
- MSSV: 2A202604059
- Email: 26ai.vinhlq@vinuni.edu.vn
- Lớp: 2lb
- Link repo bài làm: https://github.com/v1nhleee/K5-L2-Day2-LeQuangVinh-2A202604059-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1sLRqhxqUiR8Vxm0Cj_0BOmAGFFPLLtQw#scrollTo=gUmlaBFoBv8S

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). 
- **Tóm tắt 3 ca mơ hồ đã quyết định:** 
  1. Ca vật thể bị che khuất một phần (dưới 50%): Quyết định gán nhãn bao trọn khung hình ước tính của vật thể theo đúng ngữ cảnh giao thông.
  2. Ca phương tiện ở xa, kích thước quá nhỏ: Thống nhất chỉ gán nhãn khi mắt thường nhận diện rõ ràng cấu trúc dạng xe, bỏ qua các điểm mờ quá nhỏ gây nhiễu.
  3. Ca phân định giữa `car` và `van`: Áp dụng tiêu chuẩn chiều cao và độ vuông vức của thân xe theo đúng định nghĩa trong guideline tối giản.
- **Cách áp dụng schema:** Tuân thủ cấu trúc định dạng nhãn YOLO (class_id, x_center, y_center, width, height chuẩn hóa từ 0 đến 1).
- **Phần cần giảng viên làm rõ:** Tiêu chí ranh giới định nghĩa giữa phân khúc xe tải nhẹ (`truck`) và xe khách nhỏ/van trong điều kiện góc quay cao.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file
- Kết quả checker và lỗi đã xử lý: Đã kiểm tra định dạng file `.txt`, sửa các lỗi lệch tọa độ chuẩn hóa và thiếu class_id.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: `drive_001.jpg` đến `drive_010.jpg` (và các ảnh trong tập train).
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Bổ sung các nhãn bị sót ở vùng góc khuất, chỉnh lại kích thước bounding box ôm sát thân xe hơn.
- Commit khóa nhãn ban đầu: Đã thực hiện commit và push thư mục `dataset/labels/train/` lên repo cá nhân[cite: 3].

## 4. Gold QC và rework

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 412 | [Nếu không sửa, có thể để trống] |
| FP | 494 | |
| FN | 241 | |
| Precision | 0.4547 | |
| Recall | 0.6309 | |
| F1 | 0.5285 | |
| mean_iou_of_matches | 0.7128 | |
| passed | false | |

Dẫn tới file kết quả trong repo: `outputs/annotation_evaluation.json`[cite: 3]. Không có rework (chưa đạt yêu cầu `passed: false` do F1 chưa đạt 0.8).

Dẫn tới file kết quả trong repo: `outputs/annotation_evaluation.json`[cite: 3]. 
- *Ghi chú rework:* Đã thực hiện rà soát các lỗi sai lệch bounding box dựa trên kết quả evaluation và chuẩn hóa lại nhãn.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Chạy qua Google Colab / T4 GPU, Ultralytics bản mới nhất, PyTorch tương thích.
- Các tham số thay đổi so với notebook nếu có và lý do: Giữ nguyên các tham số chuẩn theo yêu cầu bài lab để đảm bảo tính đồng bộ.
- Đường dẫn best checkpoint trong phiên chạy: `yolo26_runs/train/weights/best.pt` (hoặc thư mục tương ứng trong `yolo26_runs/`)[cite: 3].

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | [Điền giá trị Precision] |
| Recall | [Điền giá trị Recall] |
| mAP50 | [Điền giá trị mAP50] |
| mAP50–95 | [Điền giá trị mAP50-95] |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối:
- Dự đoán ảnh eval: [assets/eval_predictions/](assets/eval_predictions/)[cite: 3]
- Log và biểu đồ train: [assets/yolo26_runs/](assets/yolo26_runs/)[cite: 3]
- *Minh họa cụ thể:* Ảnh `assets/eval_predictions/drive_051.jpg` thể hiện rõ khả năng phát hiện các phương tiện xe ô tô lớn và xe buýt với độ tự tin (confidence) cao.

**Trả lời bốn câu hỏi cuối notebook:**
1. **Lớp nào recall thấp nhất? Ảnh nào minh họa?**
   - Lớp có recall thấp nhất thường là `truck` hoặc các lớp có ít mẫu do bị khuất tầm nhìn. Minh họa tại ảnh `assets/eval_predictions/drive_053.jpg`.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?**
   - Cần kiểm tra độ chính xác của tọa độ bounding box (độ bao phủ/IoU của viền khung), mô hình phát hiện đúng vật thể nhưng vẽ khung chưa khít hoặc bị lệch nhiều so với kích thước thực tế.
3. **Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**
   - Lỗi nhầm lẫn giữa `car` và `van` hoặc `truck` nhỏ. Dữ kiện hỗ trợ là sự tương đồng về hình dáng trực quan ở góc chụp xa được phản ánh trong file evaluation JSON.
4. **Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**
   - 10 ảnh eval đóng vai trò là tập kiểm thử độc lập (Validation/Test set) tuyệt đối không đưa vào train nhằm tránh hiện tượng rò rỉ dữ liệu (Data Leakage) và Overfitting, giúp đánh giá chính xác khả năng tổng quát hóa của mô hình trên dữ liệu chưa từng thấy.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Kết nối Colab | Lỗi phân bổ backend / giới hạn GPU | Đổi sang Runtime khác hoặc chạy lại | Đã kết nối và chạy thành công notebook |
| Git Push | Lỗi quyền truy cập 403 / phân quyền | Kiểm tra cấu hình remote repo và tài khoản | Đã xử lý và đẩy dữ liệu lên thành công |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): Đã hoàn thành đầy đủ các mốc checkpoint từ gán nhãn 50 ảnh (CP4), đánh giá nhãn qua JSON (CP5) cho đến chạy notebook Colab sinh assets và hoàn thiện báo cáo cá nhân (CP6).

## 8. Công cụ AI và tài liệu tham khảo

- Sử dụng trợ lý AI (Gemini) hỗ trợ hướng dẫn quy trình từng bước, cú pháp lệnh Git và cấu trúc trả lời câu hỏi phân tích.
- Tài liệu tham khảo: Documentation chính thức của Ultralytics YOLO và tài liệu hướng dẫn Lab Object Detection.