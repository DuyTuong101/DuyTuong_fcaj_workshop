---
title: "Sự kiện 3: Giao lưu đấu kiến thức AWS"
date: 2026-06-20
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch sự kiện Giao lưu đấu kiến thức AWS giữa các nhóm

### Mục Đích Của Sự Kiện

- Tạo ra một môi trường hấp dẫn và cạnh tranh, nơi các thực tập sinh có thể kiểm tra kiến thức của mình về các dịch vụ cốt lõi của AWS.
- Khuyến khích làm việc nhóm nhịp độ nhanh, tư duy nhanh và giải quyết vấn đề hợp tác dưới áp lực.
- Củng cố kiến thức về các khái niệm AWS như EC2, S3, VPC, Bảo mật, Serverless và Quản lý chi phí.

### Danh Sách Người Tham Gia Và Hình Thức

- **Người tham gia:** 4 nhóm thực tập sinh từ chương trình FCAJ (bao gồm nhóm của tôi, Nhóm AQI).
- **Hình thức:** Một cuộc thi dạng đố vui với các câu hỏi nhịp độ nhanh.
    - **Vòng 1 (Cá nhân):** Mọi người tự trả lời. Những người có thành tích tốt nhất sẽ tiến vào vòng tiếp theo.
    - **Vòng 2 (Theo nhóm):** Các nhóm tập trung thảo luận để trả lời các câu hỏi tình huống phức tạp hơn.
    - **Vòng 3 (Chung kết):** Một cuộc "đấu súng" nơi các nhóm chọn một chủ đề và trả lời một câu hỏi độ khó cao để giành điểm.

---

## Nội Dung Nổi Bật

### 1. Áp lực của "Chế độ chiến đấu"

Phần căng thẳng nhất của sự kiện là tính chất nhịp độ nhanh. Ở Vòng 2, chúng tôi có chưa đầy 30 giây để thảo luận một câu hỏi và gửi một câu trả lời duy nhất với tư cách một nhóm.

Điều này buộc chúng tôi phải:
- **Giao tiếp hiệu quả:** Chúng tôi phải tin tưởng vào chuyên môn của nhau ngay lập tức (ví dụ: tôi là "người ML", một đồng đội khác là "người Mạng", một người khác là "người Bảo mật").
- **Quyết định nhanh chóng:** Chúng tôi không thể tranh cãi về các chi tiết nhỏ. Chúng tôi phải đạt được sự đồng thuận nhanh chóng.
- **Giữ bình tĩnh:** Nhóm ồn ào nhất không phải lúc nào cũng là nhóm tốt nhất. Nhóm giữ bình tĩnh và lắng nghe nhau thường có câu trả lời đúng.

### 2. Các câu hỏi "Đào sâu"

Các câu hỏi rất xuất sắc vì chúng yêu cầu nhiều hơn là chỉ ghi nhớ các định nghĩa. Chúng kiểm tra sự hiểu biết của chúng tôi về các sự *đánh đổi*.

**Ví dụ câu hỏi:**
> "Một công ty muốn chuyển đổi một ứng dụng nguyên khối sang AWS. Họ cần tính khả dụng cao và độ trễ thấp cho người dùng ở Đông Nam Á. Kiến trúc nào phù hợp nhất cho việc này và dịch vụ nào nên được sử dụng để xử lý lưu lượng truy cập?"

**Câu trả lời của nhóm tôi:**
> *Chúng tôi đề xuất một kiến trúc microservices chạy trên ECS/Fargate containers, được phân phối trên nhiều Availability Zones (AZ) trong khu vực `ap-southeast-1`. Để quản lý lưu lượng truy cập, chúng tôi khuyến nghị sử dụng Application Load Balancer (ALB).*

Phản hồi của mentor là tích cực. Ông nhấn mạnh lý luận của chúng tôi: "Việc chạy container trên Fargate giúp loại bỏ chi phí quản lý các EC2 instance, trong khi ALB xử lý việc định tuyến và tính khả dụng cao. Cách tiếp cận tốt."

### 3. Phát hiện các câu hỏi "Lắt léo"

Một số câu hỏi được thiết kế để gài bẫy những người tham gia không đọc kỹ từ ngữ.

**Ví dụ câu hỏi:**
> "Bạn có yêu cầu lưu trữ các file JSON được truy cập không thường xuyên nhưng phải có thể truy xuất ngay lập tức khi cần. Lớp lưu trữ S3 nào hiệu quả nhất về chi phí?"

*Mọi người đều nhanh chóng hét lên "S3 Standard!" hoặc "S3 Intelligent-Tiering!"*

Chúng tôi đã suýt bị lừa! Câu trả lời đúng là **S3 Standard-IA (Infrequent Access)**. Từ khóa là "truy cập không thường xuyên". S3 Intelligent-Tiering rất tốt cho các mẫu truy cập không xác định, nhưng nếu mẫu được xác định rõ ràng, Standard-IA là rẻ nhất.

---

## Những Gì Học Được

### Bài học kỹ thuật

- **Kiến thức cơ bản về Mạng là nền tảng:** Nhiều câu hỏi liên quan đến VPC, Subnets, Security Groups và NACLs. Tôi nhận ra rằng trong khi tôi tập trung vào ML, tôi phải hiểu hạ tầng mạng cơ bản. Một mô hình là vô dụng nếu endpoint của nó không thể truy cập được.
- **Serverless so với Managed vs. Unmanaged:** Cuộc thi đã củng cố sự khác biệt giữa các dịch vụ được quản lý hoàn toàn (như Lambda), được quản lý một phần (như ECS/Fargate) và không được quản lý (như EC2). Biết được ưu và nhược điểm là rất quan trọng để chọn đúng công cụ.
- **Hạ tầng toàn cầu của AWS:** Hiểu dịch vụ nào là toàn cầu (IAM, S3) và dịch vụ nào là theo khu vực (EC2, VPC) là một chủ đề lặp lại trong các câu hỏi. Điều này rất quan trọng để thiết kế các kiến trúc có khả năng phục hồi.

### Bài học về làm việc nhóm

- **Tin tưởng vào điểm mạnh của nhóm:** Chúng tôi đã thắng một số vòng vì chúng tôi tin tưởng lẫn nhau. Thay vì tranh cãi về câu trả lời, chúng tôi nhanh chóng phân công vai trò. Tôi đã lắng nghe đồng đội của mình, người là chuyên gia về RDS, và anh ấy tin tưởng tôi về Lambda. Nó đã hiệu quả.
- **Sự im lặng có thể là vàng:** Các nhóm thành công nhất không phải là những nhóm hét to nhất, mà là những nhóm lắng nghe câu hỏi một cách cẩn thận, thảo luận nhẹ nhàng và gửi một câu trả lời thống nhất.

---

## Ứng Dụng Vào Công Việc

### Áp dụng vào dự án thực tập ML hiện tại

- **Đặt tên và Gắn thẻ tài nguyên:** Tôi đã học được rằng việc gắn thẻ đúng cách là rất quan trọng để theo dõi chi phí và tài nguyên. Tôi sẽ đảm bảo rằng tất cả các tài nguyên trong dự án của tôi được gắn thẻ chính xác để giúp việc quản lý dễ dàng hơn.
- **VPC và Subnets:** Bây giờ tôi hiểu tại sao SageMaker Notebook của tôi cần phải nằm trong một subnet cụ thể để truy cập S3 và các dịch vụ khác. Tôi sẽ chú ý hơn đến cấu hình mạng khi thiết lập training job hoặc endpoint tiếp theo của mình.
- **Tối ưu hóa chi phí:** Sự kiện đã đề cập đến chi phí. Tôi sẽ ưu tiên sử dụng Spot Instances cho các tác vụ training không quan trọng để giảm chi phí và sẽ sử dụng AWS Budgets để theo dõi chi tiêu chặt chẽ.

### Áp dụng cho công việc phát triển trong tương lai

- **Tính khả dụng cao & Khả năng chịu lỗi:** Việc thiết kế theo mẫu "Multi-AZ" không chỉ là một từ thông dụng; nó là một yêu cầu đối với các hệ thống doanh nghiệp.
- **Hiểu "Tại sao" của các dịch vụ:** Tôi sẽ nỗ lực để hiểu các sự *đánh đổi* của các dịch vụ AWS, không chỉ các tính năng. Điều này sẽ dẫn đến các quyết định kiến trúc tốt hơn trong tương lai.
- **Thực hành "Sự kỹ lưỡng":** Luôn luôn đọc kỹ câu hỏi là một bài học cuộc sống áp dụng cho cả kiến trúc.

---

## Trải Nghiệm Trong Sự Kiện

**Hơn cả một cuộc đố vui**

AWS Knowledge Battle không chỉ là một cuộc đố vui thú vị. Nó là một bản tóm tắt hoàn hảo của một vài tuần đầu tiên của kỳ thực tập. Nó buộc chúng tôi phải nhớ lại tất cả những gì chúng tôi đã học về EC2, S3, Security Groups, IAM và các kiến trúc Serverless, và tổng hợp chúng dưới áp lực.

**Học thông qua cạnh tranh**

Tôi không phải là người có tính cạnh tranh điển hình, nhưng năng lượng trong phòng thực sự lan tỏa. Nó thúc đẩy tôi suy nghĩ nhanh hơn và mạnh mẽ hơn so với khi học bình thường. Tôi nhận ra rằng việc tạo ra một yếu tố cạnh tranh là một cách rất hiệu quả để củng cố kiến thức.

**Nhận ra sự "Cô lập ML" của tôi**

Với tư cách là một thực tập sinh Kỹ sư ML, tôi có xu hướng nhìn thế giới qua lăng kính của SageMaker, Jupyter Notebooks và các script Python. Sự kiện này đã buộc tôi phải bước ra khỏi sự cô lập đó và nhìn vào hạ tầng nền tảng. Tôi nhận ra rằng khả năng làm việc với nhóm Mạng và DevOps trong tương lai của tôi sẽ phụ thuộc vào sự hiểu biết của tôi về VPC, Load Balancer và Security Groups. Một Kỹ sư ML giỏi không chỉ biết mô hình; họ còn biết hạ tầng.

---


Cuộc thi diễn ra vô cùng nhanh và hấp dẫn. Vì tôi tham gia sâu vào các cuộc thảo luận nhóm và trả lời câu hỏi, tôi đã không chụp ảnh trong sự kiện. Tuy nhiên, sự phấn khích và kiến thức mà tôi thu được từ cuộc thi là những kỷ niệm sẽ đọng lại với tôi trong một thời gian dài.