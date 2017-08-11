---
layout: post
title: Experience&#58; Fifth Elephant Conference 2017
---

At Gradeup, we organize weekly engineering sessions, where in our whole engineering team shares their experiences, rants about some technology or having a knowledge session over some topic.

This week, the topic for our session was __Experience: Fifth Elephant Conference__ being organized by Taranjeet Singh [@taranjeet](https://github.com/taranjeet). He attended [Fifth Elephant Conference 2017](https://fifthelephant.in/2017/), organised by [hasgeek](https://hasgeek.com/). He shared his experience and learnings of the conference.

A brief summary of the talk is as follows


## How We Built Our Machine Intelligence To Help Humans Save Lives

A healthcare startup, aiming to reduce the diagnosis time for a heart attack by using advanced algorithm and creating data visualization platforms. Time is very critical as a large amount of heart muscles can be damaged in a very short span of time. Moreover the symptoms of heart attack and acidity are nearly the same. This is further triggered by the fact that most of the general practitioners(GP) in India are not equipped with an ECG machine.


They have built a platform which provided real time cardiac diagnosis amplifying the work of a few doctors to reach out to all patients worldwide. The report from the resident location is sent to the remote location, where doctors are available for diagnosis 24X7, and they then help in remote location diagnosis and in initiating treatment for heart disease

The talk can be found [here](https://fifthelephant.talkfunnel.com/2017/99-how-we-built-our-machine-intelligence-to-help-huma)

## Machine Learning from Practice to Production

A well structured talk, focusing on what changes when moving machine learning pipeline from development environment to production. The talks was divided into several parts, from which part 1 __Data data data__ was most interesting.

While following most of machine learning tutorial, data is already available, We just need to transform the data and start applying ML algorithms. This is not the case in production. For our specific case, dataset might not even exist and building a dataset could take much longer than expected. After the data is collected, transforming it to a form which can be understood by the machine is another task. This can be considered as a preprocessing task. For text, following can be the steps

* Tokenization

* Stopwords Removal

* Stemming

* Lemmatization

For image, following can be the steps

* Resizing

* Binarization

* Cropping

* Filtering

* Detecting Edges

The talk can be found [here](https://fifthelephant.talkfunnel.com/2017/24-machine-learning-from-practice-to-production)

## Do you know what's on TV?

Tv around us are still not smart. We still do not have smart data about what ads are being played on the TV, which celebrities are on screen right now. Deeper data about TV enables not only better experience for the viewer but for the entire industry. Actionable TV ads can change mobile marketing. Mobile ads can follow fast, Imaging a pizza discount pop-up right after you saw a PIzza ad on TV.

A little more into how you can detect a video if it contains some episode and ad.

Ad are

* short, repeat a lot

* Occurs across channels

* 2-3 ads always come together

Episodes are

* Screen contain important actors.

* Shows have screen tie.

* Parse show meta data to extract images of actors.

The talk can be found [here](https://fifthelephant.talkfunnel.com/2017/59-do-you-know-whats-on-tv)


## 5 Lessons I’ve Learned Tackling Product Matching for E-commerce

This presentation focused on 5 stories. First story was about creating a good dataset for training and validation. The key takeaway here was

> When working on custom problems, you cant make the assumption that your dataset is flawless

You have to manually intervene into your data to look if everything is right or not.

Another story here was combining signals from multiple models into a single multimodal model. Signals can be from text or from image. The novel approach here is to take the maximum from signal and add some delta value to it. A slightly better approach here can be a shared representation where in both signals constitutes.

The talk can be found [here](https://fifthelephant.talkfunnel.com/2017/98-5-lessons-ive-learned-tackling-product-matching-fo)

## Augmenting Solr’s NLP Capabilities with Deep-Learning Features to Match Images

This talk was about using Solr to match images from a collection of images. A deep learning model can be created to extract features from a image but that is an expensive training process. Hence text description of the product can be used to enhance the performance of image matching.

Compute features of a image and store them as a numeric list. Then create bucket of similar images. Self taught hashing was used as it offers a combination of supervised and unsupervised learning.
In STH, we find the optimal l-bit binary codes for all documents in the given corpus via unsupervised learning and then train l classifiers via supervised learning to predict the l-bit code for any query document unseen before.

To re-rank the results and decide which results are similar, cosine similarity is used which is a measure of similarity between two vectors. Calculating cosine similarity normalizes the score to 0 and 1 of each document.

The talk can be found [here](https://fifthelephant.talkfunnel.com/2017/80-augmenting-solrs-nlp-capabilities-with-deep-learni)

PS: All Stretching Sessions were really fun.
