---
title: "Worklog Tuần 5"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Chuyển đổi từ ML cổ điển sang Deep Learning sử dụng PyTorch/TensorFlow.
* Khám phá custom containers của SageMaker cho DL.
* Triển khai và huấn luyện mô hình DeepAR (hoặc LSTM) dự báo chuỗi thời gian sử dụng GPU.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 30  | - Nghiên cứu kiến trúc dự báo chuỗi thời gian (DeepAR, LSTM). <br> - Chuẩn bị dữ liệu theo định dạng chuỗi thời gian yêu cầu.                           | 29/06/2026   | 30/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/deepar.html> |
| 31  | - Thiết lập notebook GPU (ml.g4dn.xlarge) và môi trường PyTorch/TensorFlow. <br> - Viết script tải dữ liệu.                                            | 01/07/2026   | 01/07/2026      |
| 32  | - Cấu hình container SageMaker cho DeepAR. <br> - Gửi training job phân tán trên GPU. <br> - Giám sát sử dụng GPU qua CloudWatch.                        | 02/07/2026   | 03/07/2026      |
| 33  | - Đánh giá hiệu suất DL trên tập test. <br> - Trực quan hóa dự báo so với thực tế. <br> - Chia sẻ kết quả với nhóm.                                       | 04/07/2026   | 05/07/2026      |

### Kết quả đạt được tuần 5:

* Chuyển đổi thành công quy trình dự án sang Deep Learning và nắm vững hệ sinh thái container DL của SageMaker.
* Triển khai mô hình chuỗi thời gian phức tạp (DeepAR) và điều chỉnh siêu tham số phù hợp với các mẫu dữ liệu cụ thể.
* Có được kinh nghiệm thực tế trong việc cung cấp và sử dụng tài nguyên GPU tính toán (ml.g4dn.xlarge) hiệu quả.
* Đạt được hiệu suất dự báo tốt, các biểu đồ xác nhận trực quan cho thấy độ chính xác cao so với dữ liệu lịch sử.
