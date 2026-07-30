---
title: "Worklog Tuần 1"
date: 2026-06-05
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Hòa nhập và tham gia vào đội phát triển First Cloud AI Journey.
* Hiểu rõ mô hình trách nhiệm chung của AWS và các nền tảng bảo mật cơ bản.
* Thiết lập môi trường phát triển ban đầu bao gồm IAM Roles và AWS CLI.
* Xây dựng kiến trúc lưu trữ dữ liệu trên Amazon S3 cho các tác vụ Machine Learning.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 6   | - Buổi họp giới thiệu và tóm tắt dự án. <br> - Gặp gỡ mentor và các thực tập sinh. <br> - Thiết lập kênh liên lạc nội bộ (Slack/Teams).                | 05/06/2026   | 05/06/2026      |
| 7   | - Tìm hiểu AWS IAM (Users, Groups, Policies). <br> - Áp dụng best practices để bảo mật Access Keys. <br> - Bật MFA cho Root User.                    | 06/06/2026   | 06/06/2026      | <https://docs.aws.amazon.com/IAM/>        |
| 8   | - Cài đặt và cấu hình AWS CLI trên môi trường local. <br> - Tạo S3 bucket chuyên biệt và thiết lập cấu trúc thư mục chuẩn (raw, processed, models). <br> - Kiểm tra kết nối S3. | 07/06/2026   | 07/06/2026      | <https://aws.amazon.com/cli/>             |

### Kết quả đạt được tuần 1:

* Hoàn tất quy trình nhập môn và làm quen với văn hóa làm việc của nhóm.
* Nắm vững các khái niệm IAM của AWS, đảm bảo khung truy cập bảo mật và có đặc quyền tối thiểu cho dự án.
* Cấu hình thành công AWS CLI và xác minh kết nối thông qua lệnh `aws sts get-caller-identity`.
* Thiết kế và triển khai cấu trúc S3 Data Lake có khả năng mở rộng để tổ chức dữ liệu thô, đã xử lý và các sản phẩm mô hình cho dự án ML.
