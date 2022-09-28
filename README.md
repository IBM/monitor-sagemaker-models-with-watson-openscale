# Monitor SageMaker models using Watson OpenScale on Cloud Pak for Data hosted on AWS 

This code pattern demonstrates how to build predictive models in Amazon SageMaker and port the notebooks into IBM Cloud Pak for Data. It also shows you how to run the SageMaker notebooks in Watson Studio canvas and generate predictions.

After completing this code pattern, you will understand how to :

* Pre-process and analyze data
* Merge data to create consolidated files
* Generate use cases from the consolidated files for building predictive models
* Build multiple predictive models using SageMaker and generate predictions
* Download the models from SageMaker and import them into Watson Studio
* Run the notebooks in Watson Studio and generate predictions
* Write the predicted results to the S3 bucket
* Monitor SageMaker models in Watson OpenScale

## Architecture Diagram

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/arch-flow.png)

### Flow

1. Upload raw data from the source to the S3 bucket.
2. Pre-process and analyze the data.
3. Merge the data files to create consolidated data.
4. Import the data from the S3 bucket into SageMaker.
5. Build multiple predictive models in SageMaker.
6. Download the SageMaker models and import them into Watson Studio.
7. Run the models in Watson Studio.
8. Write the results back to the S3 bucket.

## Prerequisites

* Install Cloud Pak for Data on RedHat OpenShift cluster on AWS
* Create an account with [AWS](https://aws.amazon.com/resources/create-account/)
* Create an account with [SageMaker](https://aws.amazon.com/pm/sagemaker/)
* Create a notebook instance using [SageMaker Notebook](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html)
* Create an account with [IBM Cloud](https://cloud.ibm.com/login)
* Create a Watson OpenScale instance using [IBM Cloud](https://cloud.ibm.com/login) and select internal database using manual setup option
* Clone this [repository](https://github.com/IBM/monitor-sagemaker-models-with-watson-openscale) or `Download the repo as Zip` into your local system to access all the files. 

## Video

[![](http://img.youtube.com/vi/1PEh5TLr7m0/0.jpg)](https://youtu.be/1PEh5TLr7m0)

## Included components

* [IBM Cloud Pak for Data](https://www.ibm.com/in-en/products/cloud-pak-for-data): Predict outcomes faster using a platform built with data fabric architecture. Collect, organize, and analyze data no matter where it resides.

* [Amazon SageMaker](https://aws.amazon.com/sagemaker/): Build, train, and deploy machine learning (ML) models for any use case with fully managed infrastructure, tools, and workflows. 

* [Amazon S3](https://aws.amazon.com/s3/): Amazon Simple Storage Service (Amazon S3) is an object storage service offering industry-leading scalability, data availability, security, and performance.

* [IBM Watson Studio](https://www.ibm.com/cloud/watson-studio): Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.

* [IBM Watson OpenScale](https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=services-watson-openscale): Analyze your AI with trust and transparency and understand how your AI models make decisions. Detect and mitigate bias and drift.


## Featured technologies

* [Artificial intelligence](https://developer.ibm.com/technologies/artificial-intelligence/): Any system that can mimic cognitive functions that humans associate with the human mind, such as learning and problem solving.
* [Data science](https://developer.ibm.com/code/technologies/data-science/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.
* [Analytics](https://developer.ibm.com/code/technologies/analytics/): Analytics delivers the value of data for the enterprise.
* [Python](https://www.python.org/): A programming language that lets you work more quickly and integrate your systems more effectively.

## Steps

1. [Upload the raw data into S3 Buckets](#1-upload-the-raw-data-into-s3-buckets)
2. [Build multiple notebooks in SageMaker](#2-build-multiple-notebooks-in-sagemaker)
3. [Create projects in Watson Studio](#3-create-projects-in-watson-studio)
4. [Download the SageMaker notebooks and import them into IBM Watson Studio](#4-download-the-sagemaker-notebooks-and-import-them-into-ibm-watson-studio)
5. [Upload the SageMaker notebooks into Watson Studio project](#5-upload-the-sagemaker-notebooks-into-watson-studio-project)
6. [Run the notebooks in Watson Studio to generate predictions and endpoints](#6-run-the-notebooks-in-watson-studio-to-generate-predictions-and-endpoints)
7. [Set up Watson OpenScale for monitoring SageMaker endpoints](#7-setup-watson-open-scale-for-monitoring-sagemaker-endpoints)
8. [Monitor SageMaker endpoints using Watson OpenScale](#8-monitor-sagemaker-endpoints-using-watson-open-scale)


## 1. Upload the raw data into S3 Buckets

Create an S3 bucket in AWS by referring to the AWS documentation.

* [Click here to create a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html).
* Click on Create Bucket and enter a name for the bucket (for example, 'Datasets').
* Enable the option `Block all public access` and then click on the `Create Bucket` button.
* Select the files `COVID19BE_CASES_AGESEX.csv` and `COVID19BE_HOSP.csv` from the [data](/data) folder and upload them into the S3 bucket. 

## 2. Build multiple notebooks in SageMaker

Log in to the SageMaker console and click on the notebook instances under Notebook in the navigation pane. 

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/sm-login.png)

Click on create notebook instance, select the notebook instance type and IAM role as shown below. It may take a couple of minutes for the instance to be created. Be patient!

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/crt-nb-sm.png)

After the instance is created, click on open Jupyter. You can also open JupyterLab if needed.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/opn-nb-sm.png)

Click on `Upload` on the top right side to upload notebooks. Navigate to the [notebooks](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/notebooks) section and download all the notebooks into your local file system. You can upload the notebooks in one go and run them in SageMaker. 

**Select the Kernel as conda_python3 when prompted and choose Set Kernel option.**

When a notebook is executed, what is actually happening is that each code cell in
the notebook is executed, in order, from top to bottom.

Each code cell is selectable and is preceded by a tag in the left margin. The tag
format is `In [x]:`. Depending on the state of the notebook, the `x` can be:

* A blank, which indicates that the cell has never been executed.
* A number, which represents the relative order that this code step was executed.
* A `*`, which indicates that the cell is currently executing.

There are several ways to execute the code cells in your notebook:

* One cell at a time.
  * Select the cell and then press the `Play` button in the toolbar.
* Batch mode, in sequential order.
  * From the `Cell` menu bar, there are several options available. For example, you
    can `Run All` cells in your notebook, or you can `Run All Below`, which will
    start executing from the first cell under the currently selected cell, and then
    continue executing all cells that follow.
    
If you want to download the notebooks, select the notebook - click on File - Download as - Notebook(.ipynb) into your local file system.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/dw-nb-sm.png)
  
## 3. Create projects in Watson Studio

Sign in to the IBM Cloud Pak for Data console and then click on the Navigation Menu and expand Projects and click All Projects. Then click on ‘New Project +’ to create new project by following below steps shown in images.

![](/images/project-1.png)

![](/images/cpd-login.png)

Click on New Project and select an empty project per below.

![](/images/crt-prj.png)

Define the project by giving it a name and hit 'Create'.

![](/images/def-prj.png)

## 4. Download the SageMaker notebooks and import them into IBM Watson Studio

`Data Pre-processing` 

Navigate to the [notebooks](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/notebooks) section and download the notebook `Data-pre-processing.ipynb` into your local file system. This notebook, which is built in SageMaker, takes care of all the pre-processing activities like data analysis, merging data, missing values, feature engineering, use cases formation, and more. The updated csv files are uploaded into the S3 bucket programmatically. Upload the notebook into the Cloud Pak for Data environment using Watson Studio in the next step.

`Time-Series Forecasting Models`

Navigate to the [notebooks](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/notebooks) section and download the notebooks `WS-Flanders-Predict.ipynb` and `WS-Belgium-Predict.ipynb` into your local file system. These notebooks, which are built in SageMaker using Deep Neural networks, take care of building time-series forecasting models at the Region and Country levels. Upload the notebooks into Cloud Pak for Data environment using Watson Studio in the next step.

`Risk Index Prediction`

Navigate to the [notebooks](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/notebooks) section and download the notebook `Risk_Index_Prediction.ipynb` into your local file system. This notebook, which is built using SageMaker Linear Learner (built-in module), takes care of building multi-class classification ML model for prediction risk index per region. Upload the notebook into the Cloud Pak for Data environment using Watson Studio in the next step.

## 5. Upload the SageMaker notebooks into Watson Studio project

Log in to the project in Cloud Pak for Data and click on `Assets`.

![](/images/assets.png)

Click on `New asset`.

![](/images/new-assets.png)

Click on `Code editors`.

![](/images/code-editor.png)

Click on `Jupyter notebook editor`.

![](/images/nb-editor.png)

Select `From file` option - choose the default runtime (1vCPU & 2GB RAM) to run the notebook. 

![](/images/from-file.png)

Click on `Kernel` and choose the `Restart & Run All` option.

![](/images/run-nb.png)

**`Repeat the above steps for uploading all the notebooks in this repository.`** This is how you can build models in AWS SageMaker and port them into Cloud Pak for Data and run them in Watson Studio.

**Note:** The home directory has to be changed in the SageMaker Notebooks after uploading them into Watson Studio. The `Save the model to home directory` cell will need the home directory replaced with `/home/wsuser/work` in cell numbers 37 to 41 for `WS-Flanders-Predict.ipynb` notebook and cell numbers 36 to 40 for the `WS-Belgium-Predict.ipynb` notebook.

## 6. Run the notebooks in Watson Studio to generate predictions and endpoints

`Deploy SageMaker model from Watson Studio & create endpoints`

Navigate to the [sagemaker-model-deploy](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/sagemaker-model-deploy/RI-SageMaker-Deploy-Wstudio.ipynb) section and download the notebook `RI-SageMaker-Deploy-Wstudio.ipynb` into your local file system. 

This notebook built in Watson Studio uses SageMaker built-in modules (Linear Learner) to create a multi-class classifier model for prediction risk index per region. The Watson Studio notebook deploys the model in theSageMaker platform and creates two endpoints with different methodologies for real-time scoring. You can choose the first or the second endpoint for monitoring using Watson OpenScale. Upload the notebooks into the Cloud Pak for Data environment using Watson Studio as shown in previous step.

## 7. Set up Watson OpenScale for monitoring SageMaker endpoints

This step requires a IBM Cloud API Key. You can generate one by referring to this [link](https://www.ibm.com/docs/en/app-connect/containers_cd?topic=servers-creating-cloud-api-key).

This step requires Cloud Object Storage credentials. You can generate one by referring to this [link](https://cloud.ibm.com/login) and following the  steps below.

![](/images/sel-cos.png)

Follow the steps to create a Cloud Object Storage instance `Lite` plan and then create the bucket per below.

![](/images/crt-bkt.png)

![](/images/crt-cred.png)

![](/images/new-cred.png)

![](/images/copy-cred.png)

You should copy the bucket name, API key, and service id crn to be updated in the `SageMaker-Monitor-OpenScale` notebook.

`Set up the monitoring of SageMaker endpoint on Watson OpenScale`

Navigate to the [open-scale-model-monitor](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/open-scale-model-monitor/SageMaker-Monitor-OpenScale.ipynb) section and download the notebook `SageMaker-Monitor-OpenScale.ipynb` into your local file system. You can upload the notebook into Watson Studio as shown in step 5. This notebook built in Watson Studio uses Watson OpenScale for setting up and monitoring SageMaker endpoints. The metrics that will be evaluated are Fairness, Quality, and Drift detection of SageMaker endpoints as per the thresholds set by the user. This will help to identify whether the SageMaker model requires retraining and eliminate bias in the scoring of the data to generate predictions. 

`Upload Drift detection model file into Watson OpenScale canvas`

Navigate to the [drift-model](https://github.com/IBM/monitor-sagemaker-models-with-watson-openscale/blob/main/drift-model/Drift-Detection-Model.ipynb) folder and download the file `Drift-Detection-Model.ipynb` into your local file system. You can upload the notebook into Watson Studio as shown in step 5. You need to update the credentials in cell number 18 and the name of the SageMaker endpoint in cell number 19 and then run the notebook. 

![](https://github.com/IBM/monitor-sagemaker-models-with-watson-openscale/blob/main/images/cred-endpoint.png)

![](https://github.com/IBM/monitor-sagemaker-models-with-watson-openscale/blob/main/images/endpoint-name.png)

Run all the cells in the notebook (`by clicking on Kernel - choose Restart & Run All`) and click on `Download Drift detection model` option at the end of the notebook and download the tar.gz file into your local file system. A sample tar.gz file is available [here](https://github.com/IBM/monitor-sagemaker-models-with-watson-openscale/blob/main/drift-model/Risk-Index-Drift_detection_model.tar.gz). This needs to be uploaded into the Watson OpenScale canvas as shown in the next step. The model has been built and trained on the Risk Index data to learn and highlight when there's a change in model performance.  

## 8. Monitor SageMaker endpoints using Watson OpenScale

`Monitor SageMaker endpoints using Watson OpenScale for Fairness, Quality and Drift detection`

**Please make sure you have created an instance of Watson OpenScale from the IBM Cloud catalog and select the internal database using the manual setup option. You might be asked to sign in to access Watson OpenScale Dashboard.**

Navigate to the [Watson OpenScale Dashboard](https://aiopenscale.cloud.ibm.com/aiopenscale/insights/) to view the different metrics like Fairness, Quality, and Drift on the SageMaker endpoint. The Fairness and Quality monitoring has been set up using the Watson Studio notebook from step 7. 

Click on Linear Learner SageMaker model in the OpenScale canvas which was imported into Watson OpenScale programmatically using the notebooks from steps 6 and 7.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/LL-wos.png)

Click on `Actions` and choose `Configure monitors`.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/conf-mon.png)

click on `Drift` under the Evaluations tab and upload the Drift model tar.gz file (which was downloaded in the previous step) by clicking on the pencil icon next to the Drift model - click next and upload the file (tar.gz) created in step 7. You can also set `Drift thresholds` and `Sample size` as per your requirement.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/upload-dd-model.png)

You should see a message per below that the configuration as been saved. 

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/dd-model-saved.png)

Click on the option below to configure the metrics.

![](https://github.com/IBM/monitor-sagemaker-models-with-watson-openscale/blob/main/images/fair-qual.png)

Set up the parameters for `Fairness` per below.

![](https://github.com/IBM/monitor-sagemaker-models-with-watson-openscale/blob/main/images/fairness-threshold.png)

Set up the parameters for `Quality` per below.

![](https://github.com/IBM/monitor-sagemaker-models-with-watson-openscale/blob/main/images/quality.png)

You can view the Watson OpenScale monitor which has metrics like Fairness, Quality, and Drift set up for monitoring the model endpoints.

![](https://github.com/IBM/pandemic-management-system-on-AWS/blob/main/images/wos.png)

Now you are all set to monitor SageMaker endpoints on Watson OpenScale for Fairness, Quality and Drift metrics. You should see that the Fairness metric is showing `Green` because the threshold is set for 80% and the score is 100%. The threshold for `Area under ROC` metric is 0.8 (80%) and the score is 0.83 (83%) which is why the Quality metric is showing `Green`. Explainability is not set up because the feature works for binary classification whereas Risk Index prediction is a multi-class classification use case. Drift metrics have been set up and will be triggered when the endpoint generates predictions on new data. 

## Summary

In this code pattern, you have learned how to extract data from different sources, pre-process the data, generate two use-cases, build models in SageMaker notebooks using SageMaker built-in modules and open source modules. You have also learned how to port SageMaker notebooks into Watson Studio to generate SageMaker endpoints. You have configured Watson OpenScale to set up and monitor SageMaker endpoints for Fairness, Quality, and Drift metrics. This is a good example that demonstrates how to integrate different services using IBM and AWS to build an end-to-end solution. 

## License

This code pattern is licensed under the Apache License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1](https://developercertificate.org/) and the [Apache License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache License FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
