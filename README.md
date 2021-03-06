# HackNEU 2017
![Outlier](https://github.com/sabbaghm/HackNEU2017/blob/master/figures/outlier.png)
Northeastern University 2017 Hackathon (HackNEU) repository includes source files and documents for developing the **Outlier** project.
## Overview
We have developed an anomaly detection system using machine learning and adaptive filter methods, to detect abnormal patterns in power traces of computers, servers, and household electric consumption. The motivation behind this system was to detect anomalous power dynamics by a device which is integrated with [Google Firebase](https://firebase.google.com/) notification platform to notify users on their mobile devices using push notifications, twitter and/or slack channels, of the detected event. The project was initially developed to notify system admins of organizations of anomalous power traces usage which can be used by hackers to determine the general behavior and possibly learn the secret encryption keys of the machine. Hence as a test case, we used a power trace from an unmasked version of Advanced Encryption Standard (AES) encryption, also we used a household power consumption dataset from the University of California, Irvine to evaluate our platform (more details about the datasets can be found [here](https://github.com/sabbaghm/HackNEU2017/blob/master/dataset)). The power traces with regular trends were categorized as normal by our machine learning model. Whereas the anomalous trace/plot clearly indicated outliers in the trace, which immediately alerts the user/admin.

## Approach
In [our first method](https://github.com/sabbaghm/HackNEU2017/blob/master/srcs/MultivariateGaussian/unmasked_001.ipynb) we used a multivariate Gaussian model based on [this work](https://aqibsaeed.github.io/2016-07-17-anomaly-detection/) by [Aaqib Saeed](https://utwente.academia.edu/ASaeed) to learn the typical power trends and detect the outliers. Moreover, various data pre-processing and post-processing challenges including restructuring our dataset for our algorithm and gathering our output data to an analyzable form have been addressed. The following figures shows the output of our tool operated on an unmasked AES power traces (red point shows outliers which alert the user/admin about and anomalous event):

<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/mogOutlier.png" alt="Multivariate Gaussian model detected anomalous points in the unmasked AES power traces" width="79%" height="auto">

*Multivariate Gaussian model detected anomalous points in the unmasked AES power traces*

In our [second method](https://github.com/sabbaghm/HackNEU2017/blob/master/srcs/KalmanFilter/Kalman_filter_integrated.py), we used a Kalman filter to track the trend of power signals and detect the abnormalities in the signal amplitude. We based our implementation for this method on [this code](http://scipy-cookbook.readthedocs.io/items/KalmanFiltering.html). In this method, we defined outliers as signal points whose amplitude are higher than the Kalman filter prediction for. The first version of the Kalman filter could not track the signal properly. After changing the process variance *Q* in the [kalman filter](https://github.com/sabbaghm/HackNEU2017/blob/master/srcs/KalmanFilter/Kalman_filter_integrated.py) code from 1e-5 to 0.5 the filter started to track the signal closely.

Here are the results of our tool categorizing time points from different signal traces of our household power consumption dataset into normal (red) and abnormal (blue) trends:
<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/col3_outliers_removed.png" alt="Global Active Power vs Time Points" width="80%" height="auto">

*Global Active Power vs Time Points*

<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/col4.png" alt="Global Reactive Power vs Time Points" width="80%" height="auto">

*Global Reactive Power vs Time Points*

<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/col5.png" alt="Voltage Deviation vs Time Points" width="79%" height="auto">

*Voltage Deviation vs Time Points*

<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/col6.png" alt="Global Intensity vs Time Points" width="78%" height="auto">

*Global Intensity vs Time Points*

<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/col7.png" alt="Sub Meter 1 vs Time Points" width="79%" height="auto">

*Sub Meter 1 vs Time Points*

<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/col8.png" alt="Sub Meter 2 vs Time Points" width="77%" height="auto">

*Sub Meter 2 vs Time Points*

<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/col9.png" alt="Sub Meter 3 vs Time Points" width="80%" height="auto">

*Sub Meter 3 vs Time Points*

For creating a remote alert system, we developed a [custom Android app](https://github.com/sabbaghm/HackNEU2017/blob/master/srcs/AndroidApp) using [Android Studio](https://developer.android.com/studio/intro/index.html) which accepts notifications from Google Firebase platform. In the other end, the Firebase platform receives an alert from our anomaly detection system in case an anomalous behavior detected in the system. Here is a screenshot of a push notification received on a Samsung Android phone in our live demo:

<img src="https://github.com/sabbaghm/HackNEU2017/blob/master/figures/pushNotification.PNG" alt="Notification" width="76%" height="auto">

*Push notification received from our anomaly detection system in a live demo*

Furthermore, We integrated *Slack* and *Twitter* APIs to post alerts on those social media sites as well.

The API integration code for the notifications can be found [here](https://github.com/sabbaghm/HackNEU2017/blob/master/srcs/NotificationInterface/notificationSend.py). Note that all of the detection and notification codes except the Android app which is written in Java, are developed using [python](https://www.python.org/).

## Challenges Faced
* Handling large datasets (10000x3125)
* Preprocessing the datasets to remove NULL or empty fields
* Converting the input datasets into appropriate formats for applying the machine learning model
* Integrating Google Firebase notification platform to send push notification to users through a custom-designed Android app
* Integrating Slack and Twitter posting APIs and authentication procedures to our system

## License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/sabbaghm/HackNEU2017/blob/master/LICENSE) file for details

## Project members
|Name|Email|Contribution|
|----|-----|------------|
|Majid Sabbagh|sabbagh.m@husky.neu.edu|Problem Modeling - Finding Datasets - Android App Design - Remote Notification Integration|
|Samkeet Shah|shah.sam@husky.neu.edu|Problem Modeling - Preprocessing Datasets - Multivariate Gaussian Implementation - Plotting|
|Shantanu Kawlekar|kawlekar.s@husky.neu.edu|Problem Modeling - Preprocessing Datasets - Kalman Filter Implementation - Plotting|
|Yixing Zhang|zhang.yixin@husky.neu.edu|Problem Modeling - Finding Datasets - Report Preparation|
## Acknowledgments
We would like to thank all the mentors especially, [Rohan Jahagirdar](https://www.linkedin.com/in/rohan-jahagirdar/) and [Shreyas Mahimkar](https://www.linkedin.com/in/shreyas-mahimkar-64593918/) for their great help during the Hackathon. We also thank all the hackathon officials for creating an awesome environment for the participants to develope their ideas.
