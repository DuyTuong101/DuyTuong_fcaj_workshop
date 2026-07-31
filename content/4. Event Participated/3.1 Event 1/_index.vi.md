
**File: `content/events/4.1-Event1/_index.vi.md` (Tiếng Việt)**
```markdown
---
title: "Sự kiện 1: Chia sẻ lộ trình DevOps"
date: 2026-06-06
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch sự kiện Chia sẻ lộ trình DevOps: Từ IT Helpdesk đến DevOps tại các tập đoàn lớn

### Mục Đích Của Sự Kiện

- Chia sẻ hành trình nghề nghiệp chuyển đổi từ vị trí IT Helpdesk truyền thống lên vị trí Kỹ sư DevOps cấp cao tại các doanh nghiệp lớn.
- Giải thích các kỹ năng thiết yếu, sự thay đổi tư duy và các công cụ kỹ thuật cần thiết để thành công trong lĩnh vực DevOps.
- Cung cấp cho sinh viên và thực tập sinh cái nhìn tổng quan về bối cảnh vận hành IT hiện đại và tầm quan trọng của tự động hóa.

### Danh Sách Diễn Giả Và Nội Dung Trình Bày

- **Diễn giả:** Một chuyên gia IT dày dặn kinh nghiệm với hơn 10 năm làm việc, hiện đang lãnh đạo một nhóm DevOps tại một tổ chức tài chính lớn.
- **Nội dung tổng quan:**
    - Khởi đầu sự nghiệp: Sửa chữa phần cứng và hỗ trợ người dùng cuối tại helpdesk.
    - Lộ trình tự học: Chuyển từ các script thủ công sang Infrastructure as Code (IaC).
    - Tìm hiểu sâu về các công cụ cốt lõi: Docker, Kubernetes, CI/CD pipelines (Jenkins/GitLab CI) và các nhà cung cấp Cloud (AWS, GCP).
    - Tầm quan trọng của văn hóa "Ops": Sự hợp tác giữa đội phát triển và đội vận hành.

---

## Nội Dung Nổi Bật

### 1. Sự thay đổi tư duy: Từ sửa chữa sang phòng ngừa

Diễn giả nhấn mạnh rằng thử thách lớn nhất trong quá trình chuyển đổi không phải là học các công cụ mới, mà là thay đổi tư duy nền tảng.

- **Tư duy Helpdesk:** Phản ứng thụ động. "Chờ đợi sự cố xảy ra, rồi sửa nó."
- **Tư duy DevOps:** Chủ động. "Xây dựng hệ thống có thể thông báo cho chúng ta trước khi hỏng hóc, và lý tưởng nhất là có thể tự phục hồi."

Ông nhấn mạnh rằng các công ty đang tìm kiếm những kỹ sư có thể nhìn vào một hệ thống và tự hỏi: "Tôi có thể làm gì *hôm nay* để ngăn chặn một sự cố gián đoạn vào *ngày mai*?". Khái niệm về độ tin cậy chủ động này đã gây ấn tượng sâu sắc với tôi.

### 2. Lộ trình kỹ thuật: Một cách tiếp cận có cấu trúc

Diễn giả đã phác thảo một lộ trình tiến bộ rõ ràng để làm chủ các kỹ năng DevOps:

```text
Cấp độ 1: Vận hành thủ công
→ SSH vào server, chạy lệnh, kiểm tra log thủ công.

Cấp độ 2: Scripting & Tự động hóa
→ Viết script Bash, Python để xử lý các tác vụ lặp đi lặp lại.

Cấp độ 3: Quản lý cấu hình
→ Sử dụng Ansible, Terraform để định nghĩa hạ tầng dưới dạng code.

Cấp độ 4: Container hóa & Điều phối
→ Sử dụng Docker để đóng gói, Kubernetes (K8s) để mở rộng và quản lý container.

Cấp độ 5: CI/CD & Quan sát toàn diện
→ Tự động hóa toàn bộ pipeline (Build -> Test -> Deploy) và triển khai giám sát mạnh mẽ (Prometheus, Grafana, CloudWatch).