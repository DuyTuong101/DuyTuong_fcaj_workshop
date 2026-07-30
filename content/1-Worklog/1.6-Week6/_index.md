---
title: "Week 6 Worklog"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Deploy trained ML models to SageMaker Endpoints for real-time inference.
* Implement and test Batch Transform jobs for offline inference.
* Configure auto-scaling policies and Model Variants (A/B testing) for production readiness.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                             | Start Date | Completion Date | Reference Material                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 34  | - Study the SageMaker Inference architecture. <br> - Deploy the best performing model to a real-time SageMaker Endpoint. <br> - Test endpoint API with sample payloads. | 06/07/2026 | 07/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.html> |
| 35  | - Configure and launch a Batch Transform job to process large datasets efficiently. <br> - Compare cost and performance between Real-time and Batch inference.     | 08/07/2026 | 08/07/2026      |
| 36  | - Configure Auto Scaling policies for the endpoint based on request latency. <br> - Explore Model Variants for Canary deployments and A/B testing.               | 09/07/2026 | 10/07/2026      |
| 37  | - Create and configure an Endpoint Config with blue/green deployment strategy. <br> - Run load tests on the deployed endpoint. <br> - Document deployment setup.  | 11/07/2026 | 12/07/2026      |

### Week 6 Achievements:

* Demonstrated expertise in deploying ML models to AWS production environments.
* Successfully created and tested a live SageMaker Endpoint, ensuring less than 100ms latency per inference.
* Implemented Batch Transform jobs to handle batch data, reducing cost for offline workloads.
* Configured auto-scaling policies and understood the fundamentals of A/B testing with SageMaker Variants for reliable model updates.
