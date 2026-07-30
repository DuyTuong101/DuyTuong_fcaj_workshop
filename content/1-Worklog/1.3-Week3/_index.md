---
title: "Week 3 Worklog"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Understand the process of training models using SageMaker's built-in algorithms.
* Configure, execute, and monitor training jobs with XGBoost and Linear Learner.
* Validate model performance using regression/classification metrics on the validation set.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                             | Start Date | Completion Date | Reference Material                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 16  | - Research and identify the appropriate built-in algorithm for the business problem. <br> - Format the S3 data into the required Protobuf/CSV format.             | 15/06/2026 | 16/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html> |
| 17  | - Configure SageMaker Estimator for XGBoost. <br> - Launch the training job on ml.m5.xlarge instances. <br> - Monitor job logs in CloudWatch.                    | 17/06/2026 | 17/06/2026      |
| 18  | - Train a second baseline model using Linear Learner. <br> - Compare both model results and analyze feature importance.                                          | 18/06/2026 | 19/06/2026      |
| 19  | - Evaluate models against business KPIs. <br> - Select the best performing baseline model. <br> - Document the training configuration and results.               | 20/06/2026 | 21/06/2026      |

### Week 3 Achievements:

* Mastered the configuration syntax of SageMaker Estimators and the data packaging requirements.
* Successfully executed XGBoost and Linear Learner training jobs on AWS managed compute instances.
* Implemented performance evaluation scripts to calculate key metrics (RMSE, R2, Accuracy) for regression and classification tasks.
* Selected an optimal baseline model and prepared a technical summary for the team review.
