---
title: "Proposal"
date: 2026-06-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Trong phần này, bạn sẽ tìm thấy đề xuất chi tiết cho dự án thực tập cốt lõi 4 tuần: **Hệ thống Dự báo & Cảnh báo Chất lượng Không khí Cục bộ (Local AQI Forecasting & Alert System)**. Dự án được thiết kế như một pipeline Machine Learning end-to-end trên nền tảng AWS, mô phỏng môi trường IoT thực tế để dự đoán Chỉ số Chất lượng Không khí (AQI) và gửi cảnh báo sớm đến người dùng.

# Hệ thống Dự báo & Cảnh báo Ô nhiễm Không khí Cục bộ
## Giải pháp AWS thống nhất để giám sát chất lượng không khí thời gian thực và cảnh báo dự đoán

### 1. Tóm tắt điều hành
**Hệ thống Dự báo & Cảnh báo AQI Cục bộ** là một dự án cộng tác kéo dài 4 tuần do nhóm 5 thực tập sinh tại First Cloud AI Journey (AWS) thực hiện. Hệ thống được thiết kế để mô phỏng một mạng lưới giám sát môi trường sử dụng nguồn dữ liệu công khai (OpenAQ). Hệ thống tận dụng AWS IoT Core để thu thập dữ liệu, Kinesis Data Firehose cho luồng streaming và Amazon S3 làm kho dữ liệu tập trung (Data Lake). Cốt lõi của hệ thống là **pipeline Machine Learning** sử dụng Amazon SageMaker để xử lý dữ liệu thô và huấn luyện mô hình dự báo chuỗi thời gian **DeepAR**. Mô hình đã huấn luyện được triển khai dưới dạng SageMaker Endpoint, cho phép backend FastAPI lấy dự báo và kích hoạt cảnh báo thời gian thực qua Amazon SNS khi chỉ số AQI vượt ngưỡng. Giải pháp này chứng minh một quy trình MLOps end-to-end hoàn chỉnh và có khả năng mở rộng trong một khung thời gian ngắn.

### 2. Tuyên bố vấn đề
#### Vấn đề hiện tại là gì?
Ô nhiễm không khí là một vấn đề đô thị nghiêm trọng. Tuy nhiên, người dân địa phương thường dựa vào các báo cáo của bên thứ ba chậm trễ, thiếu dữ liệu chi tiết và khả năng dự đoán theo thời gian thực. Cần có một hệ thống cục bộ không chỉ giám sát dữ liệu AQI lịch sử mà còn có thể thông minh dự đoán các đợt tăng đột biến ô nhiễm trong tương lai và chủ động cảnh báo cho người dân.

#### Giải pháp là gì?
Giải pháp của chúng tôi xây dựng một mạng lưới cảm biến mô phỏng (sử dụng dữ liệu lịch sử OpenAQ) để truyền dữ liệu vào AWS. Pipeline bao gồm:
1.  **Thu thập dữ liệu:** Các thiết bị IoT mô phỏng gửi tin nhắn MQTT đến AWS IoT Core và Kinesis Data Firehose, đưa dữ liệu thô vào S3 Data Lake.
2.  **Xử lý dữ liệu & ML:** Các SageMaker Processing Jobs làm sạch và kỹ thuật đặc trưng dữ liệu, sau đó được sử dụng để huấn luyện mô hình **DeepAR** cho dự báo chuỗi thời gian.
3.  **Triển khai & Cảnh báo:** Mô hình tối ưu được triển khai dưới dạng SageMaker Endpoint thời gian thực. Backend FastAPI truy vấn endpoint này để lấy dự báo AQI (24-48 giờ tới). Khi dự báo vượt quá ngưỡng an toàn, hệ thống sẽ gửi cảnh báo SMS/Email đến người dùng đã đăng ký qua Amazon SNS.

#### Lợi ích và ROI
- **Tự động hóa thời gian thực:** Thay thế các báo cáo thủ công bằng dự báo tự động, cập nhật liên tục.
- **An toàn cộng đồng:** Cho phép các biện pháp chủ động thông qua cảnh báo sớm trước khi nồng độ ô nhiễm tăng đột biến.
- **Khả năng mở rộng:** Kiến trúc serverless có thể dễ dàng mở rộng để đáp ứng hàng trăm trạm (cảm biến) mô phỏng mới mà không cần cung cấp hạ tầng nặng.
- **Hiệu quả chi phí:** Tận dụng serverless (Auto Scaling Spot Instances cho huấn luyện, S3 lifecycle) đảm bảo chi phí thấp cho mô hình thử nghiệm, chứng minh tính khả thi của giải pháp.

### 3. Kiến trúc giải pháp
Hệ thống sử dụng kiến trúc 5 module, được điều phối toàn diện trên AWS trong vòng 4 tuần.
1.  **Ingestion:** Dữ liệu OpenAQ được mô phỏng và gửi đến AWS IoT Core / EC2 MQTT Broker bởi M1.
2.  **Storage:** M2 thiết lập Kinesis Data Firehose để truyền dữ liệu vào phân vùng `raw/` của S3 bucket.
3.  **Processing & ML:** M3 (ML Engineer) sử dụng SageMaker Processing Jobs để làm sạch và kỹ thuật đặc trưng. Dữ liệu sạch được dùng để huấn luyện DeepAR qua SageMaker Estimators. Tiến hành tối ưu siêu tham số (HPO) để cải thiện RMSE. Mô hình cuối được triển khai thành SageMaker Endpoint.
4.  **Backend & Alert:** M4 phát triển Backend FastAPI trên EC2 để đăng ký gọi SageMaker Endpoint, và tích hợp logic kích hoạt thông báo SNS khi vượt ngưỡng.
5.  **DevOps & QA:** M5 quản lý IAM roles/VPC, thiết lập CloudWatch giám sát và kiểm thử tích hợp end-to-end.

![Kiến trúc hệ thống AQI địa phương](/images/2-Proposal/aqi_architecture.jpeg)
*(Lưu ý: Vui lòng thay thế bằng sơ đồ kiến trúc thực tế của nhóm bạn)*

### Các dịch vụ AWS sử dụng
- **AWS IoT Core / EC2 (MQTT Broker):** Quản lý điểm cuối thu nhận dữ liệu từ cảm biến môi trường mô phỏng.
- **Kinesis Data Firehose:** Đảm bảo phân phối dữ liệu stream một cách đáng tin cậy đến S3.
- **Amazon S3:** Đóng vai trò là Data Lake tập trung cho dữ liệu thô, dữ liệu đã xử lý và các artifact mô hình.
- **Amazon SageMaker:** Xử lý Data Processing Jobs, huấn luyện DeepAR, Tối ưu siêu tham số (HPO), và Triển khai mô hình lên Endpoint thời gian thực.
- **FastAPI (trên EC2):** Lưu trữ backend API logic để người dùng đăng ký và truy xuất dự báo AQI.
- **Amazon SNS:** Quản lý hệ thống cảnh báo SMS và Email cho các sự kiện AQI nguy hiểm.
- **Amazon CloudWatch & IAM:** Giám sát sức khỏe hệ thống và thực thi phân quyền truy cập bảo mật dựa trên vai trò tối thiểu.

### 4. Triển khai kỹ thuật
**Các giai đoạn triển khai (Thực tập 4 tuần)**
Dự án được chia thành các luồng song song nhưng có kết nối, với các buổi tổng kết hàng tuần:
- **Tuần 1:** Thiết lập IAM/VPC, xây dựng S3 Data Lake, mô phỏng pipeline thu thập dữ liệu với IoT Core và thực hiện EDA ban đầu trên SageMaker.
- **Tuần 2:** Chạy SageMaker Processing Jobs để làm sạch và định dạng dữ liệu chuỗi thời gian. Huấn luyện mô hình DeepAR baseline đầu tiên. Bắt đầu xây dựng khung FastAPI skeleton.
- **Tuần 3:** Tối ưu siêu tham số (HPO) cho DeepAR để đạt độ chính xác cao nhất. Triển khai mô hình tốt nhất lên SageMaker Endpoint và tích hợp đầy đủ với backend FastAPI để kích hoạt luồng end-to-end.
- **Tuần 4:** Hoàn thành kiểm thử end-to-end toàn diện. Hoàn thiện tài liệu kỹ thuật và chuẩn bị cho buổi thuyết trình/demo cuối kỳ.

**Yêu cầu kỹ thuật (Với vai trò ML Engineer - M3)**
- **Deep Learning Framework:** Kiến thức chuyên sâu về PyTorch/TensorFlow và container DeepAR của SageMaker.
- **ML Modeling:** Kinh nghiệm cấu hình SageMaker Estimators, xử lý dữ liệu chuỗi thời gian và thực hiện Tối ưu siêu tham số (HPO) để giảm thiểu RMSE/MAE.
- **Triển khai:** Có khả năng triển khai mô hình đã huấn luyện thành SageMaker Endpoint cho suy luận thời gian thực.
- **Giao tiếp liên nhóm:** Phối hợp thường xuyên với M2 (để nhận dữ liệu sạch) và M4 (để bàn giao endpoint cho API sử dụng).

### 5. Mốc thời gian & Cột mốc
- **Cuối Tuần 1:** Chứng minh luồng thu thập dữ liệu hoạt động: Cảm biến mô phỏng -> S3 bucket thô.
- **Cuối Tuần 2:** Bàn giao tập dữ liệu đã làm sạch và mô hình DeepAR baseline hoạt động tạo ra dự báo sơ bộ. Hàm đăng ký backend đã kiểm tra được với SNS.
- **Cuối Tuần 3:** Trình diễn thành công luồng end-to-end: Thu thập dữ liệu thời gian thực -> SageMaker Endpoint dự báo -> Backend phát hiện vượt ngưỡng -> SNS gửi cảnh báo.
- **Cuối Tuần 4:** Thuyết trình dự án cuối kỳ trước hội đồng và nộp tài liệu kỹ thuật đầy đủ.

### 6. Dự toán ngân sách
Dự án sử dụng kiến trúc thân thiện với Free Tier (tầng miễn phí) khi có thể. Chi phí ước tính chính cho thời gian thực tập 4 tuần:
- **SageMaker Training & Endpoint:** Các instance `ml.m5.xlarge` và `ml.g4dn.xlarge` (GPU cho DeepAR) (khoảng $0.50 - $1.50/giờ, chỉ tính phí trong thời gian training/deploy).
- **Amazon S3:** Chi phí lưu trữ tối thiểu (< $0.10 cho 4 tuần thử nghiệm).
- **Kinesis Data Firehose:** $0.03 mỗi GB dữ liệu đưa vào (rất thấp cho mô phỏng).
- **EC2 & NAT Gateway:** Khoảng $0.20/ngày để lưu trữ backend.
- **Tổng chi phí ước tính:** Dưới **$20** cho toàn bộ vòng đời phát triển và kiểm thử, được M5 quản lý và tối ưu hóa chặt chẽ.

### 7. Đánh giá rủi ro
#### Ma trận rủi ro
- **Mô hình quá khớp / Độ chính xác kém:** Ảnh hưởng cao, Xác suất trung bình.
- **Chi phí tăng đột biến (SageMaker Endpoint):** Ảnh hưởng trung bình, Xác suất thấp.
- **Mất kết nối mạng khi thu thập dữ liệu:** Ảnh hưởng trung bình, Xác suất thấp.

#### Chiến lược giảm thiểu
- **Mô hình:** Triển khai dừng sớm (early-stopping) và các chiến lược HPO hiệu quả.
- **Chi phí:** Thiết lập cảnh báo ngân sách CloudWatch nghiêm ngặt (ngưỡng $5) để dừng training job ngay lập tức.
- **Mạng:** Triển khai cơ chế thử lại mạnh mẽ trong script thu thập dữ liệu.

#### Kế hoạch dự phòng
- Nếu DeepAR không hội tụ, chuyển sang dùng XGBoost hoặc Linear Learner làm baseline thống kê.
- Sử dụng chế độ huấn luyện local-mode của SageMaker để kiểm tra script trước khi chạy trên cloud tốn chi phí.

### 8. Kết quả mong đợi
#### Kết quả kỹ thuật
- Một pipeline dự báo AQI thời gian thực, hoàn toàn tự động.
- Một SageMaker Endpoint đã triển khai có thể truy cập qua FastAPI để dự đoán 24-48 giờ tới.
- Một hệ thống cảnh báo có khả năng mở rộng, gửi thông báo đến nhiều người đăng ký qua Amazon SNS.

#### Giá trị lâu dài
- Đóng vai trò là mô hình thử nghiệm cấp sản xuất cho các hệ thống giám sát môi trường thành phố thông minh.
- Cung cấp nền tảng pipeline dữ liệu vững chắc cho các nghiên cứu AI/ML trong tương lai về xác định nguồn gây ô nhiễm.
