# Monitor SageMaker Models using Watson Open Scale on Cloud Pak for Data hosted on AWS 

In this Code Pattern, we will demonstrate how to build predictive models in SageMaker and port the notebooks into IBM Cloud Pak for Data. We will run the SageMaker notebooks in Watson Studio canvas and generate predictions. 

After completing this Code Pattern, you will understand how to :

* Pre-process & analysis of data
* Merge data to create consolidated files
* Generate use-cases from the consolidated files for building predictive models
* Build multiple predictive models using SageMaker and generate predictions
* Download the models from SageMaker and Import them into Watson Studio
* Run the notebooks in Watson Studio and generate predictions
* Write the predicted results to S3 bucket
* Monitor SageMaker models in Watson Open Scale

## Architecture Diagram

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/arch-flow.png)

### Flow

1. Upload raw data from source to S3 bucket
2. Pre-process & analyze the data 
3. Merge the data files to create consolidated date
4. Ingest the data from S3 bucket into SageMaker
5. Build multiple predictive models in SageMaker
6. Download the SageMaker models and import them into Watson Studio
7. Run the models in Watson Studio 
8. Write the results back to S3 bucket
9. Generate visualizations using embedded dashboard

## Pre-requisites

* Install Cloud Pak for Data on RedHat OpenShift cluster on AWS
* Create an account with [AWS](https://aws.amazon.com/resources/create-account/)
* Create an account with [SageMaker](https://aws.amazon.com/pm/sagemaker/)
* Create a Notebook instance [SageMaker Notebook](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html)

## Video

[![](http://img.youtube.com/vi/1PEh5TLr7m0/0.jpg)](https://youtu.be/1PEh5TLr7m0)

## Included components

* [IBM Cloud Pak for Data](https://www.ibm.com/in-en/products/cloud-pak-for-data): Predict outcomes faster using a platform built with data fabric architecture. Collect, organize and analyze data, no matter where it resides.

* [Amazon SageMaker](https://aws.amazon.com/sagemaker/): Build, train, and deploy machine learning (ML) models for any use case with fully managed infrastructure, tools, and workflows. 

* [Amazon S3](https://aws.amazon.com/s3/): Amazon Simple Storage Service (Amazon S3) is an object storage service offering industry-leading scalability, data availability, security, and performance.

* [IBM Watson Studio](https://www.ibm.com/cloud/watson-studio): Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.

* [IBM Watson Open Scale](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=services-watson-openscale): Use IBM Watson OpenScale to analyze your AI with trust and transparency and understand how your AI models make decisions. Detect and mitigate bias and drift.


## Featured technologies

* [Artificial Intelligence](https://developer.ibm.com/technologies/artificial-intelligence/): Any system which can mimic cognitive functions that humans associate with the human mind, such as learning and problem solving.
* [Data Science](https://developer.ibm.com/code/technologies/data-science/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.
* [Analytics](https://developer.ibm.com/code/technologies/analytics/): Analytics delivers the value of data for the enterprise.
* [Python](https://www.python.org/): Python is a programming language that lets you work more quickly and integrate your systems more effectively.

## Steps

1. [Upload the raw data into S3 Buckets](#1-upload-the-raw-data-into-s3-buckets)
2. [Build multiple notebooks in SageMaker](#2-build-multiple-notebooks-in-sagemaker)
3. [Create projects in Watson Studio](#3-create-projects-in-watson-studio)
4. [Download the SageMaker notebooks and import them into IBM Watson Studio](#4-download-the-sagemaker-notebooks-and-import-them-into-ibm-watson-studio)
5. [Upload the SageMaker notebooks into Watson Studio project](#5-upload-the-sagemaker-notebooks-into-watson-studio-project)
6. [Run the notebooks in Watson Studio to generate predictions and endpoints](#6-run-the-notebooks-in-watson-studio-to-generate-predictions-and-endpoints)
7. [Setup Watson Open Scale for monitoring SageMaker endpoints](#7-setup-watson-open-scale-for-monitoring-sagemaker-endpoints)
8. [Monitor SageMaker endpoints using Watson Open Scale](#8-monitor-sagemaker-endpoints-using-watson-open-scale)


## 1. Upload the raw data into S3 Buckets

Create an S3 bucket in AWS by refering to the AWS documentation.

* [Click here to create a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)
* Click on Create Bucket and enter a name for the bucket. (for example, 'Datasets')
* Enable the option `Block all public access` and then click on `Create bucket` button.

## 2. Build multiple notebooks in SageMaker

Login to the SageMaker console and click on Notebook instances under Notebook in the navigation pane. 

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/sm-login.png)

Click on create notebook instance, select the Notebook instance type & IAM role as shown below. It will take a couple of minutes for the instance to be created. Be patient!

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/crt-nb-sm.png)

After the instance is created, click on open Jupyter. You can also open JupyterLab if needed.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/opn-nb-sm.png)

Click on `Upload` on the top right side to upload notebooks. Navigate to the [notebooks](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/notebooks) section and download all the notebooks into your local file system. You can upload the notebooks in one go and run them in SageMaker. 

When a notebook is executed, what is actually happening is that each code cell in
the notebook is executed, in order, from top to bottom.

Each code cell is selectable and is preceded by a tag in the left margin. The tag
format is `In [x]:`. Depending on the state of the notebook, the `x` can be:

* A blank, this indicates that the cell has never been executed.
* A number, this number represents the relative order this code step was executed.
* A `*`, this indicates that the cell is currently executing.

There are several ways to execute the code cells in your notebook:

* One cell at a time.
  * Select the cell, and then press the `Play` button in the toolbar.
* Batch mode, in sequential order.
  * From the `Cell` menu bar, there are several options available. For example, you
    can `Run All` cells in your notebook, or you can `Run All Below`, that will
    start executing from the first cell under the currently selected cell, and then
    continue executing all cells that follow.
    
If you want to download the notebooks, select the notebook - click on File - Download as - Notebook(.ipynb) into your local file system.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/dw-nb-sm.png)
  
## 3. Create projects in Watson Studio

Sign in to IBM Cloud Pak for Data console

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/cpd-login.png)

Click on New Project and select an empty project per below.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/crt-prj.png)

Define the project by giving a Name and hit 'Create'.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/def-prj.png)

## 4. Download the SageMaker notebooks and import them into IBM Watson Studio

`Data Pre-processing` 

Navigate to the [notebooks](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/notebooks) section and download the notebook 'Data-pre-processing.ipynb' into your local file system. This notebook built in SageMaker takes care of all the pre-processing activities like data analysis, merging data, missing values, feature engineering, usecases formation & more. The updated csv files are uploaded onto S3 bucket programmatically. Upload the notebook into Cloud Pak for Data environment using Watson Studio in the next step.

`Time-Series Forecasting Models`

Navigate to the [notebooks](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/notebooks) section and download the notebooks 'WS-Flanders-Predict.ipynb' & 'WS-Belgium-Predict.ipynb' into your local file system. These notebooks built in SageMaker using Deep Neural networks takes care of building time-series forecasting models at the Region & Country levels. Upload the notebooks into Cloud Pak for Data environment using Watson Studio in the next step.

`Risk Index Prediction`

Navigate to the [notebooks](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/notebooks) section and download the notebook 'Risk_Index_Prediction.ipynb' into your local file system. This notebook built using SageMaker Linear Learner (in-built module) takes care of building multi-class classification ML model for prediction risk index per region. Upload the notebook into Cloud Pak for Data environment using Watson Studio in the next step.

## 5. Upload the SageMaker notebooks into Watson Studio project

Log in to the project in Cloud Pak for Data and click on `Add to project`. 

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/add-to-prj.png)

Click on `Notebook`

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/sel-nb.png)

Select `From file` option - choose the runtime as 8vCPU & 32GB RAM - click on `Upload` and select the notebook from your local file system to complete uploading the notebook.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/from-file.png)

`Note: If you do not see the runtime as 8vCPU & 32GB RAM in the drop down, then you will have to add the runtime under Environments tab in the project.` 

Click on `Environments`. 

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/env.png)

Create new runtime environment.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/new-env.png)

**`Repeat the above steps for uploading all the notebooks in this repository.`**

## 6. Run the notebooks in Watson Studio to generate predictions and endpoints

`Deploy SageMaker model from Watson Studio & create endpoints`

Navigate to the [sagemaker-model-deploy](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/sagemaker-model-deploy/RI-SageMaker-Deploy-Wstudio.ipynb) section and download the notebook 'RI-SageMaker-Deploy-Wstudio.ipynb' into your local file system. This notebook built in Watson Studio uses SageMaker inbuilt modules (Linear Learner) to create a multiclass classifier model for prediction risk index per region. The Watson Studio notebook deploys the model in SageMaker platform and creates two endpoints with different methodologies for real-time scoring. Upload the notebooks into Cloud Pak for Data environment using Watson Studio as shown in previous step.

## 7. Setup Watson Open Scale for monitoring SageMaker endpoints

`Setup the monitoring of SageMaker endpoint on Watson Open Scale`

Navigate to the [open-scale-model-monitor](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/open-scale-model-monitor/SageMaker-Monitor-OpenScale.ipynb) section and download the notebook 'SageMaker-Monitor-OpenScale.ipynb' into your local file system. This notebook built in Watson Studio uses Watson Open Scale for setup & monitor SageMaker endpoints. The metrics which will be evaluated are Fairness, Quality & Drift detection of SageMaker endpoints as per the thresholds set by the user. This will help to identify whether the SageMaker model requires retraining & eliminate bias in the scoring of the data to generate predictions. 

`Upload Drift detection model file into Watson Open Scale canvas`

Navigate to the [](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/drift-model/Risk-Index-Drift_detection_model.tar.gz) section and download the file 'Risk-Index-Drift_detection_model.tar.gz' into your local file system. The model has been built and trained on the Risk Index data to learn and highlight when there's a change in model performance. This needs to be uploaded into Watson Open Scale canvas as shown in the previous steps. 

## 8. Monitor SageMaker endpoints using Watson Open Scale

`Monitor SageMaker endpoints using Watson Open Scale for Fairness, Quality & Drift detection`

This step requires IBM Cloud API Key. You can generate one [here](https://www.ibm.com/docs/en/app-connect/containers_cd?topic=servers-creating-cloud-api-key)

This step requires Cloud Object Storage credentials. You can generate one [here](https://www.ibm.com/docs/en/app-connect/containers_cd?topic=type-cloud-object-storage-s3-account-details)

Navigate to the [link](https://aiopenscale.cloud.ibm.com/aiopenscale/insights/357fc90a-5d8f-4a51-8fb5-734aa84b2b86/) to view the different metrics like Fairness, Quality, Drift etc on the SageMaker endpoint. The Fairness & Quality monitoring has been setup using the Watson Studio notebook from step 7. 

Click on Linear Learner SageMaker model in Open Scale canvas which was imported into Watson Open Scale programmatically using the notebooks from steps 6 & 7.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/LL-wos.png)

Click on `Actions` and choose `Configure monitors`.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/conf-mon.png)

click on `Drift` under Evaluations tab and upload the Drift model tar.gz file (which was downloaded in previous step) by clicking on the pencil icon next to Drift model - click next and upload the file. You can also set `Drift thresholds` & `Sample size` as per your requirement.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/upload-dd-model.png)

You should see a message per below that configuration is saved. 

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/dd-model-saved.png)

You can view the Watson Open Scale monitor which has metrics like `Fairness, Quality & Drift` set up for monitoring the model endpoints.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/wos.png)

We are all set to monitor SageMaker endpoints on Watson Open Scale for Fairness, Quality & Drift metrics. Explainability is not set up because the feature works for binary classification where as Risk Index prediction is multi-class classification use-case.

## Summary

In this code pattern, we have learnt how to extract data from different sources, pre-process the data, generate two use-cases, build models in SageMaker notebooks using SageMaker in-built modules & open sourced modules. We have also learnt how to port SageMaker notebooks into Watson Studio to generate SageMaker endpoints. We have configured Watson Open Scale to setup & monitor SageMaker endpoints for Fairness, Quality & Drift metrics. This is a good example of migration story from SageMaker to Cloud Pak for data and integration of different services using IBM & AWS to build end to end solution. 

## License

This code pattern is licensed under the Apache License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1](https://developercertificate.org/) and the [Apache License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache License FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)


