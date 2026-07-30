---
title: "Week 4 Worklog"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Introduction to the concept of Hyperparameter Tuning (HPO) using SageMaker.
* Setting up and running HPO jobs with Bayesian and Random Search strategies.
* Analyzing tuning results and extracting the optimal hyperparameters.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                             | Start Date | Completion Date | Reference Material                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 23  | - Research SageMaker Hyperparameter Tuning (HPO) architecture. <br> - Define the hyperparameter search space (ranges for learning rate, max_depth, etc.).        | 22/06/2026 | 23/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html> |
| 24  | - Configure an HPO job using the HyperparameterTuner object. <br> - Submit the tuning job to launch multiple training jobs simultaneously.                        | 24/06/2026 | 24/06/2026      |
| 25  | - Monitor the HPO job progress. <br> - Analyze the resulting model metrics and hyperparameter distribution. <br> - Retrieve the best model artifact.             | 25/06/2026 | 26/06/2026      |
| 26  | - Retrain the best model with optimal parameters on the full dataset. <br> - Validate the performance gain against the previous baseline.                         | 27/06/2026 | 28/06/2026      |

### Week 4 Achievements:

* Gained a deep understanding of automatic model tuning and advanced optimization techniques.
* Successfully constructed and executed a SageMaker HPO job, utilizing Bayesian optimization to efficiently search the parameter space.
* Analyzed the HPO logs and identified the most impactful hyperparameters contributing to performance.
* Achieved a measurable improvement in the key metrics (e.g., lowered RMSE) compared to the baseline model.
