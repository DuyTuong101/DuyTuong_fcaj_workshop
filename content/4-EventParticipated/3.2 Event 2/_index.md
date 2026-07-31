
---

#### Sự kiện 2: Tech Tools & Containerization Workshop

**File: `content/events/4.2-Event2/_index.md` (Tiếng Anh)**
```markdown
---
title: "Event 2: Tech Tools & Containerization Workshop"
date: 2026-06-13
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Reflection on Tech Tools & Containerization Workshop: Matching Tools to Roles

### Purpose of the Event

- Understanding the appropriate toolchains for different roles in a modern software development team (Developers, Ops Engineers, Data/ML Engineers).
- Providing a hands-on technical deep dive into Containerization concepts, specifically Docker.
- Explaining the basics of container networking and orchestration to prepare participants for deploying scalable applications.

### Speakers and Content

- **Speaker:** A Senior Solutions Architect at AWS with extensive experience in containerized applications and microservices architectures.
- **Content Overview:**
    - Analyzing the tool selection process for different teams.
    - Demystifying Docker: Images, Containers, and Registries.
    - Container networking concepts: Port mapping, bridge networks, and how containers talk to each other.
    - Introduction to Orchestration: Why Kubernetes and why it is used for production systems.

---

## Key Highlights

### 1. The "Right Tool" Philosophy

The speaker started with a compelling argument: "Tool selection should be based on the role's problem domain, not just industry hype."

He presented a matrix (which I noted down):

| Role | Primary Goal | Recommended Tools |
| :--- | :--- | :--- |
| **Developer** | Build features fast | `Node.js/Python`, `npm/pip`, `Git`, `VS Code` |
| **Ops Engineer** | Keep the system stable | `Terraform`, `Ansible`, `CloudWatch`, `Prometheus` |
| **Data/ML Engineer** | Analyze data, Train models | `Python`, `Pandas`, `SageMaker`, `Jupyter` |

He advised against forcing one tool onto everyone. "As an ML Engineer," he said, "you need tools that help you manipulate data and track experiments, not tools meant for standard web development."

### 2. The "Layered" Understanding of Docker

The workshop demystified Docker by breaking it down into 3 conceptual layers:

1.  **The Image:** A read-only template with instructions for creating a container. It's like a "class" in programming, or an ISO file.
2.  **The Container:** A runnable instance of an image. It is the "object" or the "virtual machine" that actually runs your code. It is ephemeral.
3.  **The Registry:** A place to store and share images (like Docker Hub or Amazon ECR).

The speaker demonstrated how to write a `Dockerfile` to define an environment for a simple Python app:

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]