---
title: "Worklog Tuần 7"
date: 2026-07-13
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Xây dựng quy trình ML tự động với SageMaker Pipelines.
* Tích hợp AWS Step Functions để điều phối luồng công việc phức tạp.
* Hiểu vòng đời ML thông qua Model Registry để quản lý phiên bản.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 38  | - Nghiên cứu các thành phần của SageMaker Pipelines (Steps, Parameters, Properties). <br> - Thiết kế pipeline DAG end-to-end.                          | 13/07/2026   | 14/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-pipeline.html> |
| 39  | - Viết code cho các bước Data Processing, Training và Model Evaluation. <br> - Tạo pipeline và thực thi test run.                                      | 15/07/2026   | 15/07/2026      |
| 40  | - Khám phá AWS Step Functions để quản lý luồng công việc nhiều bước. <br> - Thiết lập CloudWatch Alarm giám sát lỗi thực thi pipeline.                  | 16/07/2026   | 17/07/2026      |
| 41  | - Đăng ký mô hình đã đánh giá vào SageMaker Model Registry (Phiên bản 1.0). <br> - Tự động hóa quy trình phê duyệt/từ chối cho triển khai mô hình.     | 18/07/2026   | 19/07/2026      |

### Kết quả đạt được tuần 7:

* Thiết kế thành công SageMaker Pipeline có thể tái sử dụng, điều phối toàn bộ vòng đời ML từ xử lý dữ liệu đến đánh giá mô hình.
* Có kinh nghiệm thực hành với AWS Step Functions, cho phép thực thi luồng công việc điều kiện phức tạp.
* Triển khai giám sát và cảnh báo sử dụng CloudWatch, thúc đẩy phát hiện sự cố chủ động trong hệ thống CI/CD.
* Tận dụng SageMaker Model Registry để lưu trữ, lập phiên bản và quản lý các artifact mô hình một cách tập trung, đảm bảo khả năng truy xuất nguồn gốc và tái tạo.
