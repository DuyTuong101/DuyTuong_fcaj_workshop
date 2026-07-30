---
title: "Week 5 Worklog"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Transition from classical ML to Deep Learning (DL) frameworks like PyTorch/TensorFlow.
* Explore SageMaker's custom containers for DL models.
* Implement and train a DeepAR model (or an LSTM-based architecture) for time-series forecasting using GPU instances.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                             | Start Date | Completion Date | Reference Material                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 30  | - Research time-series forecasting architectures (DeepAR, LSTM). <br> - Prepare the dataset in the specific time-series data format required by the model.        | 29/06/2026 | 30/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/deepar.html> |
| 31  | - Set up a GPU-based notebook instance (ml.g4dn.xlarge) and configure the PyTorch/TensorFlow environment. <br> - Write data loading scripts.                      | 01/07/2026 | 01/07/2026      |
| 32  | - Configure the SageMaker framework container for DeepAR. <br> - Submit a distributed training job on GPU instances. <br> - Monitor GPU utilization via CloudWatch. | 02/07/2026 | 03/07/2026      |
| 33  | - Evaluate the DL model performance on test data. <br> - Visualize the forecast predictions against actuals. <br> - Share results with the team.                   | 04/07/2026 | 05/07/2026      |

### Week 5 Achievements:

* Successfully transitioned the project workflow to Deep Learning and mastered the ecosystem of SageMaker DL containers.
* Implemented a complex time-series model (DeepAR) and adjusted hyperparameters to suit the specific data patterns.
* Gained practical experience in provisioning and utilizing GPU compute resources (ml.g4dn.xlarge) efficiently.
* Achieved strong forecasting performance, and the visual validation charts demonstrated a high degree of accuracy against historical data.
