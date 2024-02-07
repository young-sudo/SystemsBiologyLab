# Systems Biology - Mass Spectrometry

Younginn Park

## Introduction
The purpose of this project was to create a classification model for the Offset function, which determines the rounded integer difference between the mass of the most abundant peak $M_{mostAbundant}$ and the monoisotopic mass $M_{mono}$.

$M_{mono} = M_{mostAbundant} - Offset (M_{mostAbundant}, r)$

## Proof-of-concept
- Creation of a classification model with features: $M_{mostAb}$ (normalized min-max from 0 to 1) + all 21 possible vertex ratio features $r$
- The label for the model is the offset (integer value - here: from 0 to 13), representing the rounded difference between the highest peak and the monoisotopic mass
- Model evaluation using various metrics obtained from predictions on the test set
- Analysis of the significance of vertex ratios $r$ using SHAP values and examining whether and how the value of $M_{mostAb}$ affects this significance
- Conclusions

A multinomial logistic regression model was used in the study due to its simplicity and potential for generalization to real data, which are often noisy.

## Pre-processing and Feature Engineering

Various pre-processing steps were performed on the data, including data sampling, handling missing values, and feature engineering to create ratio features between different peaks.

## Model Training

The logistic regression model was trained using the pre-processed data, and the performance metrics were calculated on the test set.

## Analysis of the Model

SHAP values were utilized to extract information about the influence of individual features on the model's predictions. The most influential feature was found to be "mode", representing the highest peak.

## Model Training without Mode Information

A subsequent analysis was performed by training the model without mode information to understand its performance and feature importance.

## Summary and Evaluation

The project demonstrated the effectiveness of using a multinomial logistic regression model for classification in the context of mass spectrometry data. Suggestions for future improvements include feature selection techniques, noise injection in input data, and exploring alternative models such as SVM, kNN, or neural networks.

