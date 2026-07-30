---
title: "Proposal"
date: 2026-06-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

In this section, you will find the comprehensive proposal for the 4-week capstone project: **Local AQI Forecasting & Alert System**. The project is designed as an end-to-end Machine Learning pipeline on the AWS platform, simulating a real-world IoT environment to predict Air Quality Index (AQI) and send early warnings to users.

# Local AQI Forecasting & Alert System
## A Unified AWS Solution for Real-Time Air Quality Monitoring and Predictive Alerting

### 1. Executive Summary
The **Local AQI Forecasting & Alert System** is a collaborative 4-week project conducted by a team of 5 interns at the First Cloud AI Journey (AWS). The system is designed to simulate an environmental monitoring network using public data sources (OpenAQ). It leverages AWS IoT Core for data ingestion, Kinesis Data Firehose for streaming, and Amazon S3 as a central Data Lake. At the core of the system is a **Machine Learning pipeline** using Amazon SageMaker to process raw data and train a **DeepAR** time-series forecasting model. The trained model is deployed as a SageMaker Endpoint, enabling a FastAPI backend to fetch forecasts and trigger real-time alerts via Amazon SNS when AQI thresholds are breached. This solution demonstrates a complete serverless-capable, end-to-end MLOps workflow within a condensed timeframe.

### 2. Problem Statement
#### What’s the Problem?
Air pollution is a critical urban issue. However, local communities often rely on delayed, third-party reports which lack actionable, granular, and real-time forecasting. There is a pressing need for a localized system that not only monitors historical AQI data but also intelligently predicts future pollution spikes and proactively alerts residents.

#### The Solution
Our solution builds a simulated sensor network (using OpenAQ historical data) that stream data into AWS. The pipeline consists of:
1.  **Data Ingestion:** Simulated IoT devices publish MQTT messages to AWS IoT Core and Kinesis Data Firehose, landing raw data into an S3 Data Lake.
2.  **Data Processing & ML:** SageMaker Processing Jobs clean and feature-engineer the data, which is then used to train a **DeepAR** model for time-series forecasting.
3.  **Deployment & Alerting:** The optimized model is deployed as a real-time SageMaker Endpoint. A FastAPI backend queries this endpoint to get AQI predictions (24-48 hours ahead). When the forecast exceeds safety thresholds, the system sends SMS/Email alerts to subscribed users via Amazon SNS.

#### Benefits and Return on Investment
- **Real-time Automation:** Replaces static manual reports with dynamic, automated forecasts.
- **Public Safety:** Enables proactive safety measures through early warnings before pollution spikes occur.
- **Scalability:** The serverless-centric architecture can easily scale to accommodate hundreds of new simulated stations (sensors) without provisioning heavy infrastructure.
- **Cost Efficiency:** Leveraging serverless services (Auto Scaling Spot Instances for training, S3 lifecycle policies) ensures costs remain low for a prototype, proving the viability of the solution.

### 3. Solution Architecture
The system employs a 5-module architecture, fully orchestrated on AWS over a 4-week period.
1.  **Ingestion:** OpenAQ data is simulated and published to AWS IoT Core / MQTT EC2 Broker by M1.
2.  **Storage:** M2 sets up Kinesis Data Firehose to stream data into the `raw/` zone of the S3 bucket.
3.  **Processing & ML:** M3 (ML Engineer) utilizes SageMaker Processing Jobs for data cleaning and feature engineering. The clean data is used to train a DeepAR model via SageMaker Estimators. Hyperparameter tuning is applied to optimize RMSE. The final model is deployed as a SageMaker Endpoint.
4.  **Backend & Alert:** M4 develops a FastAPI Backend on EC2 to subscribe to the SageMaker Endpoint, and integrates logic to trigger Amazon SNS notifications upon threshold breaches.
5.  **DevOps & QA:** M5 manages the IAM roles/VPC security, sets up CloudWatch monitoring, and conducts end-to-end integration testing.

![Local AQI System Architecture](/images/2-Proposal/aqi_architecture.jpeg)
*(Note: Please replace with your actual architecture diagram image)*

### AWS Services Used
- **AWS IoT Core / EC2 (MQTT Broker):** Manages the ingestion endpoint for simulated environmental sensors.
- **Kinesis Data Firehose:** Enables reliable streaming data delivery to S3.
- **Amazon S3:** Serves as the centralized Data Lake for raw data, processed data, and model artifacts.
- **Amazon SageMaker:** Handles Data Processing Jobs, DeepAR model training, Hyperparameter Tuning (HPO), and Model Deployment to a real-time Endpoint.
- **FastAPI (on EC2):** Hosts the backend API logic for user subscription and AQI forecast retrieval.
- **Amazon SNS:** Manages the SMS and Email alerting system for high-risk AQI events.
- **Amazon CloudWatch & IAM:** Monitors system health and enforces secure, least-privilege role-based access.

### 4. Technical Implementation
**Implementation Phases (4-Week Internship)**
The project is split into parallel but interconnected streams, with weekly sync-ups:
- **Week 1:** Setup IAM/VPC, establish S3 Data Lake, simulate data ingestion pipeline with IoT Core, and perform initial EDA on SageMaker notebooks.
- **Week 2:** Execute SageMaker Processing Jobs to clean and format time-series data. Train the initial DeepAR baseline model. Begin developing the FastAPI skeleton.
- **Week 3:** Tune hyperparameters (HPO) for DeepAR to maximize accuracy. Deploy the best model to a SageMaker Endpoint and fully integrate it with the FastAPI backend to enable the end-to-end flow.
- **Week 4:** Complete comprehensive end-to-end testing. Finalize technical documentation and prepare the final presentation/demo.

**Technical Requirements (As an ML Engineer - M3)**
- **Deep Learning Framework:** In-depth knowledge of PyTorch/TensorFlow and the specific SageMaker DeepAR container.
- **ML Modeling:** Experience configuring SageMaker Estimators, handling time-series datasets, and performing Hyperparameter Optimization (HPO) to minimize RMSE/MAE.
- **Deployment:** Ability to deploy a trained model as a SageMaker Endpoint for real-time inference.
- **Inter-team Communication:** Regular coordination with M2 (to receive clean data) and M4 (to deliver the endpoint for API consumption).

### 5. Timeline & Milestones
- **End of Week 1:** Demonstrate a working data ingestion flow: Simulated sensors -> S3 raw bucket.
- **End of Week 2:** Deliver the clean dataset and a working baseline DeepAR model generating preliminary forecasts. Backend subscribe function tested with SNS.
- **End of Week 3:** Successfully demonstrate the end-to-end flow: Real-time data ingestion -> SageMaker Endpoint Forecast -> Backend detects threshold -> SNS sends alert.
- **End of Week 4:** Final project presentation to the committee and submission of full technical documentation.

### 6. Budget Estimation
The project utilizes a Free Tier-friendly architecture where possible. The primary estimated costs are for the duration of the 4-week internship:
- **SageMaker Training & Endpoint:** `ml.m5.xlarge` and `ml.g4dn.xlarge` (GPU for DeepAR) instances (approx. $0.50 - $1.50/hour, billed only for training/deployment time).
- **Amazon S3:** Minimal storage costs (< $0.10 for the 4-week trial).
- **Kinesis Data Firehose:** $0.03 per GB ingested (very low for simulation).
- **EC2 & NAT Gateway:** ~$0.20/day for backend hosting.
- **Total Estimated Cost:** Under **$20** for the entire development and testing lifecycle, which is strictly controlled and optimized by the DevOps (M5).

### 7. Risk Assessment
#### Risk Matrix
- **Model Overfitting / Poor Accuracy:** High Impact, Medium Probability.
- **Sudden Cost Spikes (SageMaker Endpoint):** Medium Impact, Low Probability.
- **Network Timeouts in Data Ingestion:** Medium Impact, Low Probability.

#### Mitigation Strategies
- **Model:** Implement early-stopping and efficient HPO strategies.
- **Cost:** Set up strict CloudWatch Budget Alerts ($5 threshold) to halt training jobs immediately.
- **Network:** Implement robust retry mechanisms in the ingestion script.

#### Contingency Plans
- If DeepAR fails to converge, fall back to XGBoost or Linear Learner as a statistical baseline.
- Use local-mode SageMaker training to test scripts before committing to cloud training costs.

### 8. Expected Outcomes
#### Technical Outcomes
- A fully automated, real-time AQI forecasting pipeline.
- A deployed SageMaker Endpoint accessible via FastAPI for 24-48 hour predictions.
- A scalable alert system capable of sending notifications to multiple subscribers via Amazon SNS.

#### Long-term Value
- Serves as a production-level prototype for smart city environmental monitoring.
- Provides a robust data pipeline foundation for future AI/ML research into pollution source identification.
