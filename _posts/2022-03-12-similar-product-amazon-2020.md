---
title:  "Similar Product Identification System in Amazon (2020)"
date:   2022-03-12
categories:
    - blog
tags: 
    - Amazon
    - Product Similarity
---

I was trying to learn more about identifying similar products using ML, when I came across this [paper](https://www.amazon.science/publications/a-flexible-large-scale-similar-product-identification-system-in-e-commerce) by a few
data scientist in Amazon. Here are some highlights from the paper:

## Product Embeddings

Embeddings are a common way to extract features from image and text. They are obtained by passing an observation (e.g. image or text) through a large pre-trained model, and extract one of the last few layers as embeddings. In this paper, they used:

* AlexNet for image feature
* Word2Vec trained on Google News for text features

After extraction, each embeddings are normalized and concatenated together for the next step.

## SiameseNets Deep Learning Architecture

A [Siamese neural network](https://en.wikipedia.org/wiki/Siamese_neural_network) is used to train the model. During training, 2 items will be compared against each other, and a L2 distance will be computed between them. A pariwise loss will then be computed, and back-propagated to optimize the network. We need two types of labels here: positive - 2 products that are similar to each other, negative - 2 products that are not similar to each other.

## Labelled data

I was interested to know whether there are any data-driven approach to mine labels automatically. In this paper, they tried to use customer view-to-purchase data.

* For positive pairs, the most recent viewed products and the purchased product right after that were selected
* For negative pairs, producted viewed N slots before the purchaesd product were selected

Hand-labelled dataset still performed better than the data-driven method. But a combination of both seems to produce the best result.

## Other approaches

* In the [Shopee - Price Match Guarantee Kaggle competition](https://www.kaggle.com/c/shopee-product-matching/overview), embeddings and [ArcFace](https://www.kaggle.com/c/shopee-product-matching/discussion/226279) were used
