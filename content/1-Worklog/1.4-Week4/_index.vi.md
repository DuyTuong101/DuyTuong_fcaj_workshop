---
title: "Worklog Tuần 4"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Giới thiệu về khái niệm Tối ưu siêu tham số (HPO) với SageMaker.
* Thiết lập và chạy HPO job bằng chiến lược Bayesian và Random Search.
* Phân tích kết quả tối ưu và rút ra bộ siêu tham số tối ưu nhất.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 23  | - Nghiên cứu kiến trúc HPO của SageMaker. <br> - Xác định không gian tìm kiếm siêu tham số (phạm vi cho learning rate, max_depth, v.v.).              | 22/06/2026   | 23/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html> |
| 24  | - Cấu hình HPO job sử dụng HyperparameterTuner. <br> - Gửi tuning job để khởi chạy nhiều training job đồng thời.                                       | 24/06/2026   | 24/06/2026      |
| 25  | - Giám sát tiến trình HPO. <br> - Phân tích chỉ số mô hình và phân phối siêu tham số. <br> - Truy xuất artifact mô hình tốt nhất.                     | 25/06/2026   | 26/06/2026      |
| 26  | - Đào tạo lại mô hình tốt nhất với tham số tối ưu trên toàn bộ tập dữ liệu. <br> - Xác thực cải thiện hiệu suất so với baseline trước đó.               | 27/06/2026   | 28/06/2026      |

### Kết quả đạt được tuần 4:

* Có hiểu biết sâu sắc về tính năng điều chỉnh mô hình tự động và các kỹ thuật tối ưu nâng cao.
* Xây dựng và thực thi thành công HPO job trên SageMaker, sử dụng tối ưu hóa Bayesian để tìm kiếm không gian tham số hiệu quả.
* Phân tích log HPO và xác định các siêu tham số tác động mạnh nhất đến hiệu suất.
* Đạt được cải thiện đáng kể về chỉ số chính (ví dụ: giảm RMSE) so với mô hình baseline.
