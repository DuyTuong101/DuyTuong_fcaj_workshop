
---

#### Sự kiện 3: AWS Knowledge Battle

**File: `content/events/4.3-Event3/_index.md` (Tiếng Anh)**
```markdown
---
title: "Event 3: AWS Knowledge Battle"
date: 2026-06-20
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Reflection on AWS Knowledge Battle: Inter-group Competition

### Purpose of the Event

- To create an engaging and competitive environment where interns can test their knowledge of AWS core services.
- To encourage fast-paced teamwork, quick thinking, and collaborative problem-solving under pressure.
- To reinforce learning of AWS concepts such as EC2, S3, VPC, Security, Serverless, and Cost Management.

### Participants and Format

- **Participants:** 4 intern teams from the FCAJ program (including my team, Team AQI).
- **Format:** A quiz-based battle with rapid-fire questions.
    - **Round 1 (Individual):** Everyone answers on their own. Top performers advance.
    - **Round 2 (Team-based):** Teams huddle together to answer more complex, scenario-based questions.
    - **Round 3 (Finale):** A "shootout" where teams pick a category and answer a high-difficulty question to win points.

---

## Key Highlights

### 1. The Pressure of "Battle Mode"

The most intense part of the event was the rapid-fire nature. In Round 2, we had less than 30 seconds to discuss a question and submit a single answer as a team.

This forced us to:
- **Communicate effectively:** We had to trust each other's expertise instantly (e.g., I was the "ML guy", another teammate was the "Networking guy", another was the "Security guy").
- **Decide quickly:** We couldn't argue over minor details. We had to reach a consensus fast.
- **Stay calm:** The loudest team wasn't always the best. The team that stayed calm and listened to each other typically got the correct answer.

### 2. The "Deep Dive" Questions

The questions were excellent because they required more than just memorizing definitions. They tested our understanding of *trade-offs*.

**Example Question:**
> "A company wants to migrate a monolithic application to AWS. They need high availability and low latency for their users in Southeast Asia. Which architecture is most suitable for this, and which service should be used to handle the traffic?"

**My Team's Answer:**
> *We suggested a microservices architecture running on ECS/Fargate containers, distributed across multiple Availability Zones (AZs) in the `ap-southeast-1` region. For traffic management, we recommended using an Application Load Balancer (ALB).*

The mentor's feedback was positive. He highlighted our reasoning: "Running containers on Fargate removes the overhead of managing EC2 instances, while the ALB handles the routing and high availability. Good approach."

### 3. Spotting the "Tricky" Questions

Some questions were designed to trap participants who didn't read the wording carefully.

**Example Question:**
> "You have a requirement to store JSON files that are accessed infrequently but must be instantly retrievable when needed. What is the most cost-effective S3 storage class?"

*Everyone quickly shouted "S3 Standard!" or "S3 Intelligent-Tiering!"*

We were almost fooled! The correct answer was **S3 Standard-IA (Infrequent Access)**. The keyword was "accessed infrequently". S3 Intelligent-Tiering is great for unknown access patterns, but if the pattern is clearly defined, Standard-IA is the cheapest.

---

## Lessons Learned

### Technical Lessons

- **Network Basics are Fundamental:** Many questions were about VPCs, Subnets, Security Groups, and NACLs. I realized that while I focus on ML, I must understand the underlying networking infrastructure. A model is useless if its endpoint is unreachable.
- **Serverless vs. Managed vs. Unmanaged:** The battle reinforced the difference between services that are fully managed (like Lambda), partially managed (like ECS/Fargate), and unmanaged (like EC2). Knowing the pros and cons is critical for choosing the right tool.
- **AWS Global Infrastructure:** Understanding which services are global (IAM, S3) and which are regional (EC2, VPC) was a recurring theme in the questions. This is crucial for designing resilient architectures.

### Teamwork Lessons

- **Trust your Team's Strengths:** We won several rounds because we trusted each other. Instead of fighting over the answer, we quickly assigned roles. I listened to my teammate who was an expert in RDS, and he trusted me on Lambda. It worked.
- **Silence can be Golden:** The most successful teams were not the ones that shouted the loudest, but the ones that listened to the question carefully, discussed quietly, and submitted a unified answer.

---

## Application to Current Work

### Apply to Current ML Internship Project

- **Resource Naming & Tagging:** I learned that proper tagging is crucial for tracking costs and resources. I will ensure that all resources in my project are tagged correctly to make management easier.
- **VPC and Subnets:** I now understand why my SageMaker Notebook needs to be in a specific subnet to access S3 and other services. I will pay closer attention to the networking configuration when setting up my next training job or endpoint.
- **Cost Optimization:** The event touched on cost. I will prioritize using Spot Instances for non-critical training tasks to reduce costs, and I will use AWS Budgets to monitor spending closely.

### Apply to Future Development Work

- **High Availability & Fault Tolerance:** Designing for the "Multi-AZ" (Availability Zone) pattern is not just a buzzword; it's a requirement for enterprise systems.
- **Understand the "Why" of Services:** I will make an effort to understand the *trade-offs* of AWS services, not just the features. This will lead to better architectural decisions in the future.
- **Practice "Being Thorough":** Always reading the question carefully is a life lesson that applies to architecture as well.

---

## Event Experience & Personal Reflection

**More Than Just a Quiz**

The AWS Knowledge Battle was far more than just a fun quiz. It was a perfect summary of the internship's first few weeks. It forced us to recall everything we had learned about EC2, S3, Security Groups, IAM, and Serverless architectures, and synthesize it under pressure.

**Learning Through Competition**

I am not typically a competitive person, but the energy in the room was infectious. It pushed me to think faster and harder than I would in a normal study session. I realized that creating a competitive element is a highly effective way to consolidate knowledge.

**A Realization about my "ML Silo"**

As an ML Engineer intern, I tend to view the world through the lens of SageMaker, Jupyter Notebooks, and Python scripts. This event forced me to step out of that silo and look at the foundational infrastructure. I realized that my ability to work with the Networking and DevOps team in the future will depend on my understanding of VPCs, Load Balancers, and Security Groups. A good ML Engineer doesn't just know the model; they know the infrastructure.

---


The competition was incredibly fast-paced and engaging. As I was deeply involved in the team discussions and answering questions, I didn't take photos during the event. However, the excitement and knowledge I gained from the competition are memories that will stick with me for a long time.