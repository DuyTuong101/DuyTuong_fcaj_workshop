
**File: `content/events/4.2-Event2/_index.vi.md` (Tiếng Việt)**
```markdown
---
title: "Sự kiện 2: Hội thảo về Công cụ kỹ thuật & Container hóa"
date: 2026-06-13
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch hội thảo về Công cụ kỹ thuật & Container hóa: Lựa chọn công cụ phù hợp với từng vai trò

### Mục Đích Của Sự Kiện

- Hiểu rõ các chuỗi công cụ phù hợp với các vai trò khác nhau trong một nhóm phát triển phần mềm hiện đại (Nhà phát triển, Kỹ sư vận hành, Kỹ sư Dữ liệu/ML).
- Cung cấp kiến thức thực hành sâu về các khái niệm Container hóa, cụ thể là Docker.
- Giải thích các khái niệm cơ bản về mạng container và điều phối để chuẩn bị cho người tham dự triển khai các ứng dụng có khả năng mở rộng.

### Danh Sách Diễn Giả Và Nội Dung Trình Bày

- **Diễn giả:** Một Kiến trúc sư Giải pháp Cấp cao tại AWS với kinh nghiệm sâu rộng về các ứng dụng container hóa và kiến trúc microservices.
- **Nội dung tổng quan:**
    - Phân tích quy trình lựa chọn công cụ cho các nhóm khác nhau.
    - Làm sáng tỏ Docker: Images, Containers và Registries.
    - Các khái niệm mạng container: Port mapping, bridge networks và cách các container giao tiếp với nhau.
    - Giới thiệu về Điều phối: Tại sao Kubernetes và tại sao nó được sử dụng cho các hệ thống sản xuất.

---

## Nội Dung Nổi Bật

### 1. Triết lý "Công cụ phù hợp"

Diễn giả bắt đầu với một lập luận thuyết phục: "Việc lựa chọn công cụ nên dựa trên lĩnh vực vấn đề của vai trò, không chỉ dựa trên sự cường điệu của ngành."

Ông đã trình bày một ma trận (mà tôi đã ghi chú lại):

| Vai trò | Mục tiêu chính | Công cụ được đề xuất |
| :--- | :--- | :--- |
| **Nhà phát triển** | Xây dựng tính năng nhanh | `Node.js/Python`, `npm/pip`, `Git`, `VS Code` |
| **Kỹ sư vận hành** | Giữ hệ thống ổn định | `Terraform`, `Ansible`, `CloudWatch`, `Prometheus` |
| **Kỹ sư Dữ liệu/ML** | Phân tích dữ liệu, Huấn luyện mô hình | `Python`, `Pandas`, `SageMaker`, `Jupyter` |

Ông khuyên không nên ép buộc một công cụ duy nhất cho tất cả mọi người. "Với tư cách là một Kỹ sư ML," ông nói, "bạn cần các công cụ giúp bạn thao tác dữ liệu và theo dõi thử nghiệm, không phải các công cụ dành cho phát triển web tiêu chuẩn."

### 2. Sự hiểu biết "Phân lớp" về Docker

Hội thảo đã làm sáng tỏ Docker bằng cách chia nó thành 3 lớp khái niệm:

1.  **Image:** Một mẫu chỉ đọc chứa các hướng dẫn để tạo một container. Nó giống như một "lớp" trong lập trình, hoặc một file ISO.
2.  **Container:** Một thể hiện có thể chạy của một image. Nó là "đối tượng" hoặc "máy ảo" thực sự chạy code của bạn. Nó có tính tạm thời.
3.  **Registry:** Một nơi lưu trữ và chia sẻ image (như Docker Hub hoặc Amazon ECR).

Diễn giả đã trình diễn cách viết một `Dockerfile` để định nghĩa môi trường cho một ứng dụng Python đơn giản:

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]