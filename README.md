# CxVision - AWS Marketplace Model Package

This repository offers a computer vision solution for tracking people and generate metrics that can be subsequently analyzed to improve the user experience.

This solution allows you to:

* Detect people in videos.
* Count people within a defined zone.
* Measure the spent time in each zone.
* Apply blurring on each person to maintain the privacy.


## Contents

1. [What does the solution do?](#What-does-the-solution-do?)
2. [How does the solution work?](#How-does-the-solution-work?)
    - [Real-Time Inference](#1.-Real-time-inference)
    - [Asynchronous Inference](#2.-Asynchronous-inference)
        - [Continuous video upload to S3 Bucket](#Continuous-video-upload-to-S3-Bucket)
3. [Basic concepts](#4.-Create-an-endpoint-and-perform-inferences)

## What does the solution do?

The solution allows you to define two zones in each video, a waiting zone (Dwell) and a service zone. The purpose of this is to measure how long a person takes in queue (waiting zone) and how long it takes while being served (service zone). 

> It's not necessary to define the zones. In this case, the solution will measure the time of all the people in the video

## How does the solution work?

You can deploy the CxVision Model Package from AWS Marketplace and then configure some services needed to run the solution.

With the Model Package, you can deploy an endpoint to make inferences, which can be done in two different ways:

### 1. Real-time inference
This deployment mode allows processing independent videos synchronously. The video to be processed must be stored in an S3 Bucket. In the following diagram you can see the flow of this process:

![Realtime-Inference](./imgs/realtime-inference.png)

To make real-time inferences, please follow the instructions in this notebook: [Realtime Inference Notebook](./RealTimeInference.ipynb)


### 2. Asynchronous inference:
The objective of this mode is to process a sequence of related videos. This provides the possibility of processing videos in near real-time by uploading sequential and constant video fragments to an S3 Bucket and executing a trigger for each new video fragment. The following diagram shows the flow of this process:

![Asynchronous-Inference](./imgs/asynchronous-inference.png)

#### Continuous video upload to S3 Bucket:
This alternative assume you are uploading constant videos fragments to an Amazon S3 Bucket. You could use any tool to achieve this, such as:

* **Amazon Elemental Media Live**: this AWS service allows you to create a streaming video broadcast channel and broadcast the output to different sources, one of these can be an Amazon S3 Bucket. This service can be configured to sequentially broadcast video fragments of a certain length in seconds.
* **Own implementation:** another alternative is to implement your own method to sequentially upload the video fragments to the Amazon S3 Bucket. For this solution we developed a lambda function that receives as input a video, which is divided into fragments with a given duration in seconds and then loaded to a new path of the Amazon S3 Bucket.

To make asynchonous inferences, please follow the instructions in this notebook:  [Asynchronous Inference Notebook](./AsynchronousInference.ipynb)

## Basic concepts

* Area: It refers to the area where the video was recorded. 
* Zones: It refers to the differents to the service and the dwelling zones in the video. 

![Area And Zones Example](/imgs/zones-example.jpg)





