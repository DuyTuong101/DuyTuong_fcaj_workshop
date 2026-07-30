---
title: "Worklog Tuần 2"
date: 2026-06-08
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Thiết lập và tùy chỉnh môi trường Amazon SageMaker Studio và JupyterLab.
* Phát triển kỹ năng nền tảng về Phân tích dữ liệu khám phá (EDA) và hồ sơ dữ liệu.
* Thực thi logic làm sạch và tiền xử lý dữ liệu cần thiết cho Machine Learning.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 9   | - Cấu hình SageMaker Domain và hồ sơ người dùng. <br> - Kiểm tra không gian JupyterLab và tạo notebook dự án. <br> - Kết nối SageMaker với S3 bucket.   | 08/06/2026   | 08/06/2026      | <https://docs.aws.amazon.com/sagemaker/>  |
| 10  | - Tải và kiểm tra tập dữ liệu thô từ S3 với Pandas. <br> - Thực hiện EDA sơ bộ (thống kê mô tả, phân tích dữ liệu khuyết, biểu đồ phân phối).        | 09/06/2026   | 10/06/2026      |
| 11  | - Triển khai các kỹ thuật làm sạch dữ liệu (điền giá trị, xử lý ngoại lai, loại bỏ trùng lặp). <br> - Kỹ thuật đặc trưng cơ bản bằng biến đổi ngày tháng và phân loại. | 11/06/2026   | 12/06/2026      |
| 12  | - Tạo tập Train và Validation. <br> - Tuần tự hóa và tải dữ liệu đã xử lý lên S3. <br> - Tài liệu hóa logic pipeline dữ liệu.                          | 13/06/2026   | 14/06/2026      |

### Kết quả đạt được tuần 2:

* Khởi tạo thành công môi trường SageMaker Studio và liên kết với các nguồn dữ liệu S3 an toàn.
* Đã thực hiện quy trình EDA kỹ lưỡng với Pandas và Matplotlib, rút ra những hiểu biết chính về phân phối dữ liệu.
* Triển khai script Python tự động cho tiền xử lý dữ liệu, xử lý hiệu quả các giá trị null và ngoại lai.
* Đã xuất bản tập dữ liệu đã qua kỹ thuật đặc trưng lên S3, tạo nền tảng vững chắc cho việc huấn luyện mô hình ở Tuần 3.
