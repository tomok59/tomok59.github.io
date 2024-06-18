---
layout: page
title: Chicken Disease Detection
description: CNN used to detect diseases from pictures of chicken fecal matter
nav_order: 4
---

# Leveraging Neural Networks for Poultry Disease Identification

<a href="https://github.com/tomok59/tomok59.github.io/raw/main/181 Project.pdf" target="_blank">Report Download</a>
<a href="https://github.com/tomok59/181_final/blob/main/181_final_notebook.ipynb" target="_blank">Link to Notebook </a>
<a href="https://github.com/zaki1503" target="_blank"> My Project Partner </a>

## Abstract

This study applies Convolutional Neural Networks (CNNs) to a large dataset of chicken excrement images for early detection of avian diseases, a critical issue in poultry agriculture. The dataset, from the Kaggle repository "Poultry Pathology Visual Dataset," includes images from Tanzania, classified into four categories: Healthy, Coccidiosis, NewCastle Disease, and Salmonella. 
We developed a sophisticated neural network, LNN-PDI, and used augmentation techniques to handle real-world variability. Optimization methods, including a transition to ADAMW and experimentation with different pooling and activation functions, were employed for model refinement. Despite computational constraints, our model achieved an accuracy of 89%, demonstrating the potential of CNNs in disease detection. Our findings highlight the importance of balanced datasets, strategic optimization, and adequate computational resources in developing effective tools for early avian disease detection.

## Results

Our final model resulted in a test accuracy of 88%. For our previous models we used the validation set in order to preserve the integrity of the evaluation. This is best practice as we do not want to fit the test data to our model, and it is also a way of testing “real world” data as a test for our model. Our validation scores went from 65% for the first model, to 73% in the second model. This showed we were going in the right direction and for the final model we went with a much deeper network which resulted in an 89% validation accuracy.

Confusion Matrix of our Result
<iframe src="assets/confusion_matrix_cnn.png" width=800 height=600 frameBorder=0></iframe>
