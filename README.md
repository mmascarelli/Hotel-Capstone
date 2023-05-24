# Project Overview
<p align="center">
  <img width="610" alt="hotels" src="https://github.com/mmascarelli/Hotel-Capstone/assets/116842582/aad4e71c-1d6c-4cae-94c9-0d925da4a2c1">
</p>
Cancellations can profoundly affect revenue within the hospitality industry. To address this issue, numerous hotels employ cancellation policies or utilize overbooking strategies as a means of minimizing the impact. However, these measures can sometimes lead to adverse effects on both revenue and the hotel's reputation. By developing a machine learning model capable of predicting the likelihood of booking cancellations, hotels can enhance their strategic planning and employ more
effective techniques to mitigate the financial impact caused by cancellations. By identifying bookings with a higher probability of cancellation, hotels can target them with enticing incentives, such as complimentary meals, extended stays, or other services provided by the hotel, in order to encourage guests to retain their bookings.
  
# The Data

The data is from ScienceDirect, but was originally sourced from Property Management System SQL databases. Included in the data are 31 columns from two different hotels in Portugal. Hotel 1 has 40,060 observations and hotel 2 has 79,330, where each observation represents a hotel booking. The bookings span from 2015 until 2017. The target column ‘IsCanceled’ tells us if the reservation was canceled (1) or not (0).
Reference: https://www.sciencedirect.com/science/article/pii/S2352340918315191#bib4

# Data Wrangling

The notebook can be found here: [Data Wrangling](https://github.com/mmascarelli/Hotel-Capstone/blob/main/Data%20Wrangling.ipynb)

Summary:
* Loaded and merged the data for the 2 hotels.
* Dealt with missing values in the `Children` and `Country` columns
* Removed the duplicated rows
* Dealt with outliers in the `ADR` column
* General analysis of the columns

# EDA
The notebook can be found here: [EDA](https://github.com/mmascarelli/Hotel-Capstone/blob/main/EDA.ipynb)

Summary:
* Removing rows with impossible values to improve data quality (bookings with 0 people listed, and 0 nights, etc.)
* Extensive feature engineering
* Deep exploration of the relationship between the features and the target
* Checking for correlation between features to avoid having multicollinearity
* Chi-squared tests to check for associations between the target and each feature

# Preprocessing
The notebook can be found here: [Preprocessing](https://github.com/mmascarelli/Hotel-Capstone/blob/main/Preprocessing.ipynb)

Summary:
* Separated the data into features and target (X and y)
* One-hot encoded the categorical features
* Split the data into training and test sets, with a 80/20 split
* Normalized the features
* PCA

# Modeling
The notebook can be found here: [Modeling](https://github.com/mmascarelli/Hotel-Capstone/blob/main/Modeling.ipynb)

Summary:
* Fit the training data using logistic regression, decision tree, random forest, XGboost, LightGBM
* Hyperparameter tuning to optimize the F1 score
* Analyzed feature importance outputs from the models
* Modeling attempts using SMOTE, and a stacking classifier (neither of which made a significant impact)
* Best model was LightGBM with a recall = 76% and precision = 55%
* Looked at the predicted probabilites

# Conclusions
<p align="center">
  <img width="917" alt="cm" src="https://github.com/mmascarelli/Hotel-Capstone/assets/116842582/396318d9-6c57-4ef7-a76b-5697eeb332ea">
</p>

* The LightGB model has shown the most favorable overall results in relation to the specific business value we aim to provide. Out of the 4,784 canceled bookings in our test set, the model accurately identifies 76% of them. However, it is worth noting that this model exhibits a notable decrease in precision. Nevertheless, this decrease in precision is not necessarily problematic since it refers to bookings that were predicted to cancel but actually did not.

* To reiterate the project's objective, our goal is to identify bookings that are likely to cancel in order to make efforts to retain them. If a customer was not planning to cancel in the first place, misclassifying them does not have a significant negative impact. Among the 12,458 successful bookings in the test set, only 22.7% are mistakenly classified as cancellations

* It's important to consider that the chosen strategy to retain bookings may potentially affect profitability. For instance, if the hotel decides to offer complimentary nights to every booking predicted to be at risk of cancellation, it could result in substantial costs. However, since we lack access to the financial data of these hotels, we cannot accurately assess the actual expenses. One approach the business can adopt is to base decisions not solely on the model's classifications, but also on the corresponding probabilities.

<img width="758" alt="prob" src="https://github.com/mmascarelli/Hotel-Capstone/assets/116842582/25d33e85-d247-4377-a224-a0202c0ccdec">

* The table above shows a sample of 5 bookings. Row 1 was a successful booking, but was predicted to be canceled. The model made that decision with only a 58.7% probability. Row 2 on the other hand was a canceled booking that was correctly predicted to cancel with 88% probability. A probability threshold may be needed when making the final decision on which bookings should be targeted.

## Implementation
1. Bookings predicted to cancel with at least 70% probability will be considered high risk. They will receive the following:
    - An email
    - A phone call
    - Complementary nights, meals, other services (if costs allow)
2. Bookings predicted to cancel with less than a 70% probability will be considered low risk and receive the following:
    - An email
    - A phone call

* The purpose of this strategy is to provide valuable incentives to high-risk bookings, even though
they may be expensive, because the potential revenue generated from successful bookings is expected to outweigh those costs. On the other hand, for low-risk bookings, simply contacting the customer may be sufficient to retain their booking, assuming they were contemplating cancellation in the first place. By implementing this plan, we can proactively reach out to all predicted cancellations without adversely affecting profits.

# Further Work
* Without financial data related to the hotel revenue and profit, there is no way of knowing how many incentives the hotel can realistically offer. Of the 6,483 bookings predicted by the model to cancel, how many complementary rooms or meals can be given without hurting the profits? With financial data this can be explored further, and a more accurate business decision can be made.

* In terms of improving the model, one feature that is currently unknown to us is weather information. It is intuitive to expect that many cancellations are weather related, and so having weather forecast data may be helpful. For instance, if a storm is expected over the North Atlantic Ocean, then customers traveling from North America will likely cancel.
