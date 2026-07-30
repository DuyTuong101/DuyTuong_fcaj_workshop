---
title: "Worklog Tuần 3"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Hiểu quy trình đào tạo mô hình bằng thuật toán tích hợp sẵn của SageMaker.
* Cấu hình, thực thi và giám sát training job với XGBoost và Linear Learner.
* Xác thực hiệu suất mô hình bằng các chỉ số hồi quy/phân loại trên tập validation.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 16  | - Nghiên cứu và xác định thuật toán tích hợp phù hợp cho bài toán kinh doanh. <br> - Định dạng dữ liệu S3 thành định dạng Protobuf/CSV yêu cầu.          | 15/06/2026   | 16/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html> |
| 17  | - Cấu hình SageMaker Estimator cho XGBoost. <br> - Khởi chạy training job trên instance ml.m5.xlarge. <br> - Giám sát log job trên CloudWatch.        | 17/06/2026   | 17/06/2026      |
| 18  | - Huấn luyện mô hình baseline thứ hai bằng Linear Learner. <br> - So sánh kết quả và phân tích độ quan trọng của đặc trưng.                             | 18/06/2026   | 19/06/2026      |
| 19  | - Đánh giá mô hình theo KPI kinh doanh. <br> - Chọn mô hình baseline tốt nhất. <br> - Tài liệu hóa cấu hình đào tạo và kết quả.                       | 20/06/2026   | 21/06/2026      |

### Kết quả đạt được tuần 3:

* Nắm vững cú pháp cấu hình của SageMaker Estimator và các yêu cầu đóng gói dữ liệu.
* Thực thi thành công các training job XGBoost và Linear Learner trên các instance tính toán được quản lý bởi AWS.
* Triển khai các script đánh giá hiệu suất để tính toán các chỉ số chính (RMSE, R2, Accuracy) cho tác vụ hồi quy và phân loại.
* Chọn được mô hình baseline tối ưu và chuẩn bị tóm tắt kỹ thuật cho buổi review với nhóm.
