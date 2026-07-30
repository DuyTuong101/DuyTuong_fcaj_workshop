---
title: "Week 7 Worklog"
date: 2026-07-13
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Build automated ML workflows using SageMaker Pipelines.
* Integrate AWS Step Functions for complex orchestration.
* Understand the lifecycle of ML models using the Model Registry for versioning.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                             | Start Date | Completion Date | Reference Material                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------- |
| 38  | - Study the components of SageMaker Pipelines (Steps, Parameters, Properties). <br> - Design an end-to-end Directed Acyclic Graph (DAG) pipeline.                   | 13/07/2026 | 14/07/2026 | <https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-pipeline.html> |
| 39  | - Implement code for Data Processing, Training, and Model Evaluation steps. <br> - Create the pipeline and execute a test run.                                  | 15/07/2026 | 15/07/2026 |      |
| 40  | - Explore AWS Step Functions to manage multi-step workflows. <br> - Set up CloudWatch Alarms to monitor pipeline execution failures.                            | 16/07/2026 | 17/07/2026 |      |
| 41  | - Register the evaluated model into SageMaker Model Registry (Version 1.0). <br> - Automate the approval/rejection workflow for model deployment.               | 18/07/2026 | 19/07/2026 |      |

### Week 7 Achievements:

* Successfully designed a reusable SageMaker Pipeline that orchestrates the entire ML lifecycle from data processing to model evaluation.
* Gained hands-on experience with AWS Step Functions, enabling complex conditional workflow execution.
* Implemented monitoring and alerting using CloudWatch, promoting proactive issue detection in the CI/CD system.
* Leveraged the SageMaker Model Registry to store, version, and manage the model artifacts centrally, ensuring lineage and reproducibility.
