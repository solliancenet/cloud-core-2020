# Machine Learning team challenge

**Contents** 

<!-- TOC -->

- [Machine Learning team challenge](#machine-learning-team-challenge)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
  - [Exercise 1: Creating a forecast model using automated machine learning](#exercise-1-creating-a-forecast-model-using-automated-machine-learning)
    - [Task 1: Create an automated machine learning experiment](#task-1-create-an-automated-machine-learning-experiment)
    - [Task 2: Review the experiment run results](#task-2-review-the-experiment-run-results)
    - [Task 3: Deploy the Best Model](#task-3-deploy-the-best-model)
  - [Exercise 2: Creating a deep learning model (RNN) for time series data](#exercise-2-creating-a-deep-learning-model-rnn-for-time-series-data)
    - [Task 1: Create the model using a notebook](#task-1-create-the-model-using-a-notebook)
  - [Exercise 3: Creating, training and tracking a deep learning text classification model with Azure Databricks, MLflow and Azure Machine Learning](#exercise-3-creating-training-and-tracking-a-deep-learning-text-classification-model-with-azure-databricks-mlflow-and-azure-machine-learning)
    - [Task 1: Create, train and track the classification model using a notebook](#task-1-create-train-and-track-the-classification-model-using-a-notebook)
    - [Task 2: Review model performance metrics and training artifacts in Azure Machine Learning workspace](#task-2-review-model-performance-metrics-and-training-artifacts-in-azure-machine-learning-workspace)
  - [After the team challenge](#after-the-team-challenge)
    - [Task 1: Clean up lab resources](#task-1-clean-up-lab-resources)

<!-- /TOC -->

## Abstract and learning objectives

In this hands-on lab, you will use Azure Databricks in combination with Azure Machine Learning to build, train and deploy desired models. You will learn how to train a forecasting model against time-series data, without any code, by using automated machine learning, and how to score data in real-time using Spark Structure Streaming within Azure Databricks.  You will create a recurrent neural network (RNN) model using PyTorch in Azure Databricks that can be used to forecast against time-series data and train a Natural Language Processing (NLP) text classification model using Keras. You will also learn how to use MLflow for managing experiments run directly on the Azure Databricks cluster and how MLflow can seamlessly log metrics and training artifacts in your Azure Machine Learning workspace.

At the end of this lab, you will improve your ability to build solutions leveraging Azure Machine Learning and Azure Databricks.

## Overview

Trey Research Inc. delivers innovative solutions for manufacturers. They specialize in identifying and solving problems for manufacturers that can run the range from automating away mundane but time-intensive processes to delivering cutting edge approaches that provide new opportunities for their manufacturing clients.

Trey Research is looking to provide the next generation experience for connected car manufacturers by enabling them to utilize AI to decide when to pro-actively reach out to the customer through alerts delivered directly to the car's in-dash information and entertainment head unit. For their proof of concept (PoC), they would like to focus on two maintenance related scenarios.

In the first scenario, Trey Research recently instituted new regulations defining what parts are compliant or out of compliance. Rather than rely on their technicians to assess compliance, they would like to automatically assess the compliance based on component notes already entered by authorized technicians. Specifically, they are looking to leverage Deep Learning technologies with Natural Language Processing techniques to scan through vehicle specification documents to find compliance issues with new regulations. Then each car is evaluated for out compliance components.

In the second scenario, Trey Research would like to predict the likelihood of battery failure based on the telemetry stream of time series data that the car provides about how the battery performs when the car is started, how it is charging while running and how well it is holding its charge, among other factors. If they detect a battery failure is imminent within the next 30 days, they would like to send an alert.

Upon detection of an out of compliance component or a battery at risk of failure, they would like to be able to send an alert directly to the customer inviting them to schedule a service appointment to replace the part.

In building this PoC, Trey Research wants to understand how they might use machine learning or deep learning in both scenarios, and standardize the platform that would support the data processing, model management and inferencing aspects of each.

They are also interested to learn what new capabilities Azure provides that might help them to integrate with their existing investments in MLflow for managing machine learning experiments. Furthermore, they would also like to understand how Azure might help them to document and explain the models that are created to non-data scientists or might accelerate their time to creating production ready, performant models.

In this lab, you will use Azure Databricks in combination with Azure Machine Learning to build, train and deploy the desired models.

## Solution architecture

The following diagram summarizes the key components and processing steps in the lab.

![Vehicle battery telemetry is ingested by an IoT Hub or Event Hub. This data is stored in long term storage, Azure Storage. This data is used by Azure Databricks to train the model that is managed and registered via an Azure Machine Learning workspace. AutoML is also another option that can be used to register a machine learning model. These models are then used for stream data processing and batch data processing in Azure Databricks.](media/lab-architecture.png 'Solution Architecture')

In this lab, models are trained using both Azure Databricks (for deep learning with the PyTorch and Keras frameworks) and Azure Machine Learning compute (for automated machine learning using the user experience in the Azure Machine Learning studio). Models are registered with the Azure Machine Learning Workspace. The data used for model training is read from Azure Storage.

The scoring is performed using notebooks running within Azure Databricks notebooks, which show how to load and apply the respective models against the data provided.

## Requirements

1. Microsoft Azure subscription must be Pay-As-You-go or MSDN.

    - Trial subscriptions will not work. You will run into issues with Azure resource quota limits.

    - Subscriptions with access limited to a single resource group will not work. You will need the ability to deploy multiple resource groups.

## Before the hands-on lab

Refer to the Before the hands-on lab setup guide manual before continuing to the lab exercises.

## Exercise 1: Creating a forecast model using automated machine learning

Duration: 40 minutes

In this exercise, you will create a model that predicts battery failure from time-series data using the visual interface to automated machine learning in an Azure Machine Learning workspace.

### Task 1: Create an automated machine learning experiment

1. Navigate to your Azure Machine Learning workspace in the Azure Portal. Select **Try the new Azure Machine Learning studio, Launch now**.

    ![The Azure Machine Learning workspace is displayed. The Launch now button is selected on the Overview screen.](media/automl-open-studio.png 'Open Azure Machine Learning studio')

    > **Note**: Alternatively, you can sign-in directly to the [Azure Machine Learning studio portal](https://ml.azure.com).

2. Select **Automated ML icon** in the left navigation bar.

    ![The Automated ML menu item is highlighted in the left menu in the Azure Machine Learning studio.](media/automl-open.png 'Open Automated ML section')

3. Select **+ New automated ML run**.

    ![In the Automated machine learning section in Azure Machine Learning studio. The "New automated ML run" button is selected.](./media/automl-new-run.png 'Create new automated ML run')

4. Select **+ Create dataset, From web files** to start registering your training data.

    ![From the toolbar menu, the + Create a new dataset menu item is expanded with the From web files option selected.](media/automl-create-dataset-01.png 'Create dataset for automated ML run')

5. In the `Basic info` section provide the following information and then select **Next**:

   - **Web URL**: `https://databricksdemostore.blob.core.windows.net/data/connected-car/daily-battery-time-series-v2.csv`
  
   - **Name**: `daily-battery-time-series`

    ![The Create dataset from web files Basic info form is displayed populated with the values outlined above. The Next button at the bottom of the form is selected.](media/automl-create-dataset-02a.png 'Basic info')

6. On the Settings and preview form, set the **Column headers** drop down to **All files have same headers**, and then select **Next**.

    ![The Create dataset from web files Settings and preview form is displayed with the Column headers highlighted with the All files have the same headers value selected.](media/automl-create-dataset-02b.png 'Settings and preview panel')

7. Review the training data schema. Toggle the `Include` switch next to the column name to exclude the `Path`, `Column1`, `Number_Of_Trips`, `Lifetime_Cycles_Used` and `Battery_Rated_Cycles` columns. Select **Next**.

    ![The Create dataset and from web files Schema form is displayed in tabular format. The include toggle values for the columns listed above are switched to the Off position. The Next button is highlighted at the bottom of the form.](media/automl-create-dataset-03.png 'Select Features')

8. Review the dataset details in the `Confirm details` section and select **Create**.

    ![The Confirm details screen shows a summary of the dataset to be created. The Create button is highlighted.](media/automl-create-dataset-04.png 'Confirm and create the dataset')

9. Select the `daily-battery-time-series` dataset and then select **Next**.

    ![In the Create a new Automated ML run, the daily-battery-time-series dataset is selected from the dataset list. The Next button is highlighted.](media/automl-create-dataset-05.png 'Select newly created dataset')

10. Provide the experiment name: `Battery-Cycles` and select **Daily_Cycles_Used** as target column. Select **Create a new compute**.

    ![In the Configure run form is populated with the above values. The Create a new compute button is highlighted.](media/automl-create-experiment.png 'Create New Experiment details')

11. For the new compute, provide the following values and then select **Create**:

    - **Compute name**: `auto-ml-compute`
  
    - **Select Virtual Machine size**: `STANDARD_DS3_v2`
  
    - **Minimum number of nodes**: `1`
  
    - **Maximum number of nodes**: `1`

    ![The New Training Cluster form is populated with the above values. The Create button is selected at the bottom of the form.](media/automl-create-compute.png 'Create a New Compute')

    > **Note**: The creation of the new compute may take several minutes. Once the process is completed, select **Next** in the `Configure run` section.

12. Select the `Time series forecasting` task type and provide the following values and then select **View additional configuration settings**:

    - **Time column**: `Date`

    - **Group by column(s)**: `Battery_ID`

    - **Forecast horizon**: `30`

    ![The Select task type form is populated with the values outlined above. The View additional configuration settings link is highlighted.](media/automl-configure-task-01.png 'Configure time series forecasting task')

13. For the automated machine learning run additional configurations, provide the following values and then select **Save**:

    - **Primary metric**: `Normalized root mean squared error`

    - **Training job time (hours)** (in the `Exit criterion` section): enter `1` as this is the lowest value currently accepted.

    - **Metric score threshold**: enter `0.1355`. When this threshold value will be reached for an iteration metric the training job will terminate.

    ![The Additional configurations form is populated with the values defined above. The Save button is highlighted at the bottom of the form.](media/automl-configure-task-02.png 'Configure automated machine learning run additional configurations')

    > **Note**: We are setting a metric score threshold to limit the training time. In practice, for initial experiments, you will typically only set the training job time to allow AutoML to discover the best algorithm to use for your specific data.

14. Select **Finish** to start the new automated machine learning run.

    > **Note**: The experiment should run for up to 10 minutes. If the run time exceeds 15 minutes, cancel the run and start a new one (steps 3, 9, 10, 12, 13, and 14). Make sure you provide a higher value for `Metric score threshold` in step 13.

### Task 2: Review the experiment run results

1. Once the experiment completes, select `Details` to examine the details of the run containing information about the best model and the run summary.

   ![The Run Detail screen of Run 1 indicates it has completed. The Details tab is selected where the the best model, ProphetModel, is indicated along with the run summary.](media/automl-review-run-01.png 'Run details - best model and summary')

2. Select `Models` to see a table view of different iterations and the `Normalized root mean squared error` score for each iteration. Note that the normalized root mean square error measures the error between the predicted value and actual value. In this case, the model with the lowest normalized root mean square error is the best model. Note that Azure Machine Learning Python SDK updates over time and gives you the best performing model at the time you run the experiment. Thus, it is possible that the best model you observe can be different than the one shown below.

    ![The Run Detail screen of Run 1 is displayed with the Models tab selected. A table of algorithms is displayed with the values for Normalized root mean squared error highlighted.](media/automl-review-run-02.png 'Run Details - Models with their associated primary metric values')

3. Return to the details of your experiment run and select the best model **Algorithm name**.

    ![The Run Detail screen of Run 1 is displayed with the Details tab selected. The best model algorithm name is selected.](media/automl-review-run-03.png 'Run details - recommended model and summary')

4. From the `Model` tab, select **View all other metrics** to review the various `Run Metrics` to evaluate the model performance.

    ![The model details page displays run metrics associated with the Run.](media/automl-review-run-04.png 'Model details - Run Metrics')

5. Next, select **Metrics, predicted_true** to review the model performance curve: `Predicted vs True`.

    ![The model run page is shown with the Metrics tab selected. A chart is displayed showing the Predicted vs True curve.](media/automl-review-run-05.png 'Predicted vs True curve')

### Task 3: Deploy the Best Model

1. From the top toolbar select **Deploy**.

    ![From the toolbar the Deploy button is selected.](media/automl-deploy-best-model-01.png 'Deploy best model')

2. Provide the `Name`, `Description` and `Compute type`, and then select **Deploy**:

    - **Name**: **battery-cycles**

    - **Description**: **The best AutoML model to predict battery cycles.**

    - **Compute type**: Select `ACI`.

    ![The Deploy a model dialog is populated with the values listed above. The Deploy button is selected at the bottom of the form.](media/automl-deploy-best-model-02.png 'Deploy the best model')

3. The model deployment process will register the model, create the deployment image, and deploy it as a scoring webservice in an Azure Container Instance (ACI). To view the deployed model, from Azure Machine Learning studio select **Endpoints icon, Real-time endpoints**.

   ![In the left menu, the Endpoints icon is selected. On the Endpoints screen, the Rea-time endpoints tab is selected and a table is displayed showing the battery-cycles endpoint highlighted.](media/automl-deploy-best-model-03.png 'Deployed model endpoints')

   > **Note**: The `battery-cycles` endpoint will show up in a matter of seconds but the actual deployment takes several minutes. You can check the deployment state of the endpoint by selecting it and then selecting the `Details` tab. A successful de deployment will have a state of `Healthy`.

4. If you see your model deployed in the above list, you are now ready to continue on to the next exercise.

## Exercise 2: Creating a deep learning model (RNN) for time series data

Duration: 45 minutes

### Task 1: Create the model using a notebook

1. Browse to your Azure Databricks Workspace and open `AI with Databricks and AML \ 1.0 Deep Learning with Time Series`. This is the notebook you will step through executing in this lab.

2. Follow the instructions within the notebook to complete the lab.

## Exercise 3: Creating, training and tracking a deep learning text classification model with Azure Databricks, MLflow and Azure Machine Learning

Duration: 45 minutes

In this exercise, you create a model for classifying component text as compliant or non-compliant. You will train the model on the Azure Databricks cluster and use MLflow integration with Azure Machine Learning to track and log experiment metrics and artifacts in the Azure Machine Learning workspace.

### Task 1: Create, train and track the classification model using a notebook

1. Browse to your Azure Databricks Workspace and navigate to `AI with Databricks and AML \ 3.0 Deep Learning with Text`. This is the notebook you will step through executing in this lab.

2. Follow the instructions within the notebook to complete the lab.

### Task 2: Review model performance metrics and training artifacts in Azure Machine Learning workspace

1. Select the **Link to Azure Machine Learning studio** from the output of the last cell in the notebook to open the `Run Details` page in the Azure Machine Learning studio.

   ![A notebook cell output is displayed with the Link to Azure Machine Learning studio highlighted under the Details Page column.](media/mlflow_1.png 'Open Azure Machine Learning studio')

2. The **Run Details** page shows the three metrics that were logged via MLflow during the model training process: **learning rate (lr)**, **evaluation loss (eval_loss)**, and **evaluation accuracy (eval_accuracy)**.

   ![In the Run Details page, the Metrics section containing eval_accuracy, eval_loss, and lr is highlighted.](media/mlflow_2.png 'Model Training Metrics')

3. Next, select **Outputs + logs, training_results.png** to review the model training artifacts logged using MLflow. In this section, you can review the curves showing both accuracy and loss as the model training progress. You can also observe that MLflow logs the trained model and the training history with Azure Machine Learning workspace.

   ![On the Run Details page, the Output + Logs tab is selected, and the training_results.png item is selected in a list on the left. The image is displayed showing charts of Training and validation accuracy, and Training and validation loss.](media/mlflow_3.png 'Model Training Artifacts')

## After the team challenge

Duration: 5 minutes

To avoid unexpected charges, it is recommended that you clean up all of your lab resources when you complete the lab.

### Task 1: Clean up lab resources

1. Navigate to the Azure Portal and locate the `MCW-AI-Lab` Resource Group you created for this lab.

2. Select **Delete resource group** from the command bar.

    ![The Delete resource group button.](media/cleanup-delete-resource-group.png 'Delete resource group button')

3. In the confirmation dialog that appears, enter the name of the resource group and select **Delete**.

4. Wait for the confirmation that the Resource Group has been successfully deleted. If you don't wait, and the delete fails for some reason, you may be left with resources running that were not expected. You can monitor using the Notifications dialog, which is accessible from the Alarm icon.

    ![The Notifications dialog box has a message stating that the resource group is being deleted.](media/cleanup-delete-resource-group-notification-01.png 'Notifications dialog box')

5. When the Notification indicates success, the cleanup is complete.

    ![The Notifications dialog box has a message stating that the resource group has been deleted.](media/cleanup-delete-resource-group-notification-02.png 'Notifications dialog box')

You should follow all steps provided _after_ attending the team challenge.
