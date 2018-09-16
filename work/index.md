---
layout: page
title: "Work"
permalink: /work
---
Professional Experience
===

### Hiya, Inc.
*Android Software Engineering Intern*  
*Seattle, WA*  
*May - Jul 2018*

  * Lead architecture design and implementation of Smart Dial for Hiya.
  * Worked on Kotlin adoption, RxJava, Dagger, Glide, MVP architecture, JobScheduler, improving test coverage, re-architecture of code, bug fixes and UI/UX enhancements.
  * Built an on-device algorithm to continuously rank people and businesses a user is most likely to call.
  * Gained further experience with Kotlin, RxJava, Dagger, Glide, MVP architecture, JobScheduler and several other Android APIs.

  <p align="center">
  <img src="/images/HiyaSmartDial.png" width="200" hspace="50" vspace="20">
  </p>

  ***

### TiVo, Inc.
*Associate Software Engineer*  
*Bangalore, India*  
*Jul 2016 - Jun 2017*

  * Worked on building a Knowledge Graph of films, TV shows, celebrities, sports, music etc that is the brain behind the conversational TiVo TV box.
  * Built Knowledge Graph connections between movies/TV shows, cast/crew and roles they played.
  * Created a 12% increase in Knowledge Graph coverage by building a Logistic Regression model to automatically infer program attributes in 26 languages.

***

### Hike Messenger
*Android Software Engineering Intern*  
*New Delhi, India*  
*Jul - Dec 2015*

  * Designed and developed Hike Discover used by 70 million users to discover Hike micro-apps.
  * Built a messaging bot POC using wit.ai which could interact with users and deliver services like mobile phone payments.
  * Improved the Hike Platform SDK for communication between byte-sized WebApps and Android code.

  <p align="center">
  <img src="/images/HikeDiscover.png" width="240" hspace="50" vspace="20">
  <img src="/images/CustomKeyboards.png" width="240" hspace="50" vspace="20">
  <img src="/images/RechargeBot.png" width="240" vspace="20"><br>
  </p>

  ----

Personal projects
===

### Groovy
*An Android application for discovery of Movies and TV Shows built using the TMDB API.*

Work in progress. [Github repo.](https://github.com/amanps/Groovy){:target="_blank"}

* Built purely in Kotlin
* Built using the MVP architecture with Dagger 2, RxJava 2 and several key Android libraries / APIs.

<img src="/images/Groovy1.png" width="200"> <img src="/images/Groovy2.png" width="200">
<img src="/images/Groovy3.png" width="200"> <img src="/images/Groovy4.png" width="200">

***

### Music Genre Classification
*Built a Convolutional Neural Network to predict the genre of a song from a 3 second raw mp3 clip.*  
[Project presentation.](https://docs.google.com/presentation/d/16QbXJ_cKZ5_p9sIonV_zEVIoQ9OOkAEtcVElQGeXKM0/edit#slide=id.g383cb1a59a_0_119){:target="_blank"}
  * Converted raw mp3 clips into their Spectrogram representation using Fourier Transforms.  
  * Sliced each spectrogram into 128x128 pixel slices representing 2.56 seconds of music per slice.  
  * Applied a CNN with 4 convolutional layers, each followed by a Max Pooling layer and finally Fully Connected layers and Softmax at the end.  
  * Test Accuracy achieved per slice : 86%.  
  * Applied a voting scheme to all slices of a test song to improve accuracy to 92%.

***

### Machine Learning
*A collection of Machine Learning and Data Science related projects I've been a part of.*  

  * Conducted [EDA on TED talks](https://github.com/amanps/TED-Data-Analysis){:target="_blank"} using Python & Pandas to derive insights like audience engagement, recurring topic trends, correlation between speaker speed, audience response and keywords.
  * Developed a [Logistic Regression model](https://github.com/amanps/LogisticRegression/blob/master/LogisticRegression.ipynb){:target="_blank"} from scratch using only Python and numpy to predict admission probability of a student.
