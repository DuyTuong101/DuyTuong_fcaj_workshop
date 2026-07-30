---
title: "Worklog Tuần 6"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Triển khai các mô hình ML đã huấn luyện lên SageMaker Endpoint để suy luận thời gian thực.
* Thực hiện và kiểm tra Batch Transform job cho suy luận ngoại tuyến.
* Cấu hình chính sách Auto Scaling và Model Variants (A/B testing) để sẵn sàng cho môi trường sản xuất.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 34  | - Nghiên cứu kiến trúc Inference của SageMaker. <br> - Triển khai mô hình tốt nhất lên Endpoint thời gian thực. <br> - Kiểm tra API endpoint với payload mẫu. | 06/07/2026   | 07/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.html> |
| 35  | - Cấu hình và chạy Batch Transform job để xử lý tập dữ liệu lớn hiệu quả. <br> - So sánh chi phí và hiệu năng giữa suy luận thời gian thực và Batch.      | 08/07/2026   | 08/07/2026      |
| 36  | - Cấu hình chính sách Auto Scaling cho endpoint dựa trên độ trễ yêu cầu. <br> - Khám phá Model Variants cho các bản triển khai A/B.                     | 09/07/2026   | 10/07/2026      |
| 37  | - Tạo và cấu hình Endpoint Config với chiến lược blue/green deployment. <br> - Chạy kiểm tra tải trên endpoint đã triển khai. <br> - Tài liệu hóa thiết lập triển khai. | 11/07/2026   | 12/07/2026      |

### Kết quả đạt được tuần 6:

* Thể hiện chuyên môn trong việc triển khai các mô hình ML lên môi trường sản xuất của AWS.
* Tạo và kiểm tra thành công SageMaker Endpoint hoạt động, đảm bảo độ trễ dưới 100ms mỗi lần suy luận.
* Triển khai các Batch Transform job để xử lý dữ liệu theo lô, giảm chi phí cho tác vụ ngoại tuyến.
* Cấu hình chính sách auto-scaling và hiểu các nguyên tắc cơ bản của thử nghiệm A/B với Variants của SageMaker cho các bản cập nhật mô hình đáng tin cậy.
