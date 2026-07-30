---
title: "Worklog Tuần 8"
date: 2026-07-20
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Giới thiệu Amazon SageMaker Feature Store để quản lý đặc trưng tập trung.
* Sử dụng SageMaker Data Wrangler để phân tích và biến đổi dữ liệu trực quan.
* Kỹ thuật đặc trưng nâng cao và cộng tác xuyên nhóm trên các đặc trưng chung.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 42  | - Tìm hiểu các khái niệm SageMaker Feature Store (Offline và Online stores). <br> - Thiết kế cấu trúc Feature Group cho dữ liệu dự án.                  | 20/07/2026   | 21/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store.html> |
| 43  | - Đưa các đặc trưng đã kỹ thuật vào Feature Store. <br> - Truy vấn Offline Feature Store bằng Athena để kiểm tra tính nhất quán dữ liệu.              | 22/07/2026   | 22/07/2026      |
| 44  | - Học cách sử dụng SageMaker Data Wrangler. <br> - Tạo luồng dữ liệu để làm sạch và biến đổi trực quan một tập dữ liệu thô lớn mà không cần viết code. | 23/07/2026   | 24/07/2026      |
| 45  | - Xuất luồng Data Wrangler thành bước SageMaker Pipeline. <br> - Cộng tác với các nhóm chức năng chéo để chuẩn hóa các định nghĩa đặc trưng chung.      | 25/07/2026   | 26/07/2026      |

### Kết quả đạt được tuần 8:

* Hiểu được lợi ích và chi tiết triển khai của SageMaker Feature Store cho việc quản lý phiên bản và chia sẻ dữ liệu.
* Tạo và đưa dữ liệu thành công vào Online và Offline Feature Group để phục vụ suy luận thời gian thực và theo lô.
* Có được kinh nghiệm đáng kể trong việc chuẩn bị dữ liệu ML ít code sử dụng SageMaker Data Wrangler.
* Hài hòa các định nghĩa đặc trưng với nhóm, thiết lập một "nguồn sự thật" thống nhất cho các đầu vào mô hình ML.
