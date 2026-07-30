---
title: "Worklog Tuần 9"
date: 2026-07-27
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Tích hợp tất cả các thành phần cá nhân thành một hệ thống ML end-to-end thống nhất.
* Tối ưu hóa chi phí đào tạo thông qua việc sử dụng SageMaker Spot Instances.
* Code review chuyên sâu, kiểm toán bảo mật và đánh giá hiệu suất.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 46  | - Hợp nhất các SageMaker Pipeline và chiến lược Feature Store cá nhân thành một kiến trúc thống nhất. <br> - Code review sâu rộng với mentor.          | 27/07/2026   | 28/07/2026      |
| 47  | - Triển khai Spot Instances để giảm chi phí AWS lên tới 70%. <br> - Viết chiến lược checkpointing để xử lý gián đoạn spot một cách an toàn.            | 29/07/2026   | 29/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/model-spot-training.html> |
| 48  | - Thực hiện kiểm toán bảo mật trên IAM roles và chính sách S3. <br> - Thiết lập bộ benchmarking hiệu suất cho toàn bộ pipeline.                        | 30/07/2026   | 31/07/2026      |
| 49  | - Chuẩn bị và thực hiện demo end-to-end cho mentor. <br> - Thu thập phản hồi và tinh chỉnh logic cuối cùng của hệ thống.                              | 01/08/2026   | 02/08/2026      |

### Kết quả đạt được tuần 9:

* Hoàn thành tích hợp toàn bộ hệ thống, chứng minh một pipeline ML end-to-end ổn định và chịu lỗi.
* Giảm hiệu quả chi phí đào tạo bằng cách cấu hình Spot Instances và triển khai cơ chế checkpoint.
* Đảm bảo dự án đáp ứng các tiêu chuẩn bảo mật doanh nghiệp thông qua các đánh giá chính sách IAM và S3 nghiêm ngặt.
* Đạt được cột mốc quan trọng khi trình diễn thành công hệ thống toàn diện cho ban quản lý, nhận được phản hồi vận hành có giá trị.
