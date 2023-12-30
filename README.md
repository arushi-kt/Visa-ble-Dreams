# Visa-ble-Dreams
Visa-ble Dreams: U.S. H-1B Visa Analysis

Journey to the Land of Code and Opportunity!


**Problem Statement:**
Our problem revolves around the complexity of U.S. immigration dynamics, specifically concerning H-1B visas. There are approximately 500k applications filed each year and each application costs ~6000 USD. As of now, there is an average rejection rate of 5% overall which leads to a loss of 150mn USD. Moreover, this also impacts the employee’s personal life mentally and physically causing a lot of stress. This makes it necessary and useful to understand patterns and characteristics in H-1B visa issuance for policy making, corporate hiring strategies, and job seekers' decisions. 

**Outcome:**
Create a framework that identifies the withdrawn cases based on certain inputs in the data.  Provide solutions to reduce the number of withdrawn cases and improve chances of acceptance based on the profound understanding of key factors influencing H-1B visa issuance through extensive data analysis.

**Proposal:**
Analysis of various features such as case status, employer name, worksite coordinates, job title, prevailing wage, occupation code etc. to identify the most important factors affecting the Visa status of being accepted. Once we identify the important features, we build a framework consisting of an ML Model and insights from the data that increases the chances of Visa certification. 


**Data Understanding:**
The H-1B Dataset selected for this project contains data from employer’s Labor Condition Application and the case certification determinations processed by the Office of Foreign Labor Certification (OFLC). The Labor Condition Application (LCA) is a document that a perspective H-1B employer files with U.S. Department of Labor Employment and Training Administration (DOLETA) when it seeks to employ non-immigrant workers at a specific job occupation in an area of intended employment for not more than three years. The datasets are from the Department of Labor's website. 

There is data collated from 2011 to 2018 and contains ~4mn rows with 21 columns. The Features include date of application submission, decision date, employer details such as the industry, name and location. It also includes the details of employee wages, employee work location and whether the employment is full time or part time. Most of the information/features in the data were redundant or null i.e., they lead to a very narrowed down version for the analysis which would lead us to have multiple columns during one hot encoding or give unusually higher weights during label encoding. Hence, we removed a few redundant columns and ended up with 9 columns. We further appended another US geographical dataset containing Region mapping of each state such as North, South, East, West and Midwest to perform analysis at a region level. 

Hence, our final dataset had 10 columns which are: case status, employer name, job title, soc code, soc name, naics code, work city, work zip, region, prevailing wage. 
(Note: Data dictionary and data snip attached in the ppt slide 5 and slide 6)

**Solution Approach Steps:** The steps used to arrive at our solution include the following.

Data Preprocessing and EDA: Check for nulls values and handle accordingly. Solve data imbalance, Label encoding for categorical features, analyse various features and obtain findings
Data Split and Modelling: 1mn rows of the data was sampled then split in 70:20:10 ratio across training, validation and test sets. Various models were trained and tested to obtain the best results
Result validation and Model Tuning: Metrics such as Accuracy and ROC were used to compare and finalise the model
Result Interpretation and Feature Importance: The obtained error metrics are then converted to required business solutions using the confusion matrix and feature importance to deliver insights
(Note: Refer ppt slide 8)

**Exploratory Data Analysis: **(Note: Refer ppt slide 10 to 14)
The data being 4mn rows would have taken too much space and time to analyse. Hence, we limited ourselves to the latest data of 2017 and 2018 which resulted in ~1mn data points.

**Outlier Analysis** - We had only one numerical column in our dataset which is the wage of the employee. On doing outlier analysis using IQR, we noticed that the lower limit was 12k USD and the higher limit was 140K USD. As 12k USD is generally quite low for someone on a H1B visa, we dropped such rows. All data points above 140k were capped to 140k

**Frequency Plots **– Most of our columns including the Target variable were categorical. We implemented frequency plots to find some trends and performed encoding to implement correlation plots and the model
**Case status:** This is the target variable. In the entire data of 4mn rows, we had only 5% of withdrawn vs 95% of accepted cases. We treated these using oversampling
Region vs Case Status: Plotted a percentage plot of region wise split of accepted cases vs withdrawn cases and didn’t notice any drastic difference at a region level
State vs Case Status: As the number of states were high, plotting a graph for the same would make it messy. Hence, we obtained a table of the state and the corresponding filings per state and rejection ratio per state. We noticed that California, Texas and New York have had the highest filings (almost 100k) across 2017 to 2018 whereas Montana, New Mexico and West Virginia have the highest rejection Ratio (~8%)
**Employer vs Case Status:** Filings for each employer and the rejection ratio was obtained. We noticed that employers such as Infosys and TCS have the highest filings and least rejection rate whereas there are a range of companies with high rejection rate and low filings across this time period. Since we were also given the industry code, we mapped the data to the employer and noticed that technical services and Professional companies have the highest filings and this also aligns with the Employer filings
**No of days from Submission:** We had the submission and decision date of the filing. We calculated the average no of days it takes to release a decision if it is accepted or withdrawn. We noticed that on average, if the decision is obtained within 32 days, it is most likely an accepted case whereas the average days for withdrawn cases is 83 days. When looking at percentile trends of approval we see that by the 300-day mark 82% of total test set employees receive their approval, implying that a delay beyond that time is more likely to be a rejection

**Modelling:**(Note: Refer ppt slide 16)
We split our data into train, validation and test sets. The models we’ve tried are given below: 

**Logistic Regression: **Logistic regression is a simple but effective classification algorithm that predicts the probability of an instance belonging to a particular class using a linear equation transformed by a logistic function. This gave an accuracy of 54.2% and ROC of 56.2%
Random Forest: Random Forest is an ensemble learning method that creates multiple decision trees and combines their predictions to improve accuracy and reduce overfitting, making it suitable for both classification and regression tasks. This gave us an accuracy of 63.94% and ROC of 70.67%
Boosting: Boosting is another ensemble technique that iteratively builds a strong model by giving more weight to misclassified instances, gradually improving its predictive power through a weighted combination of weak learners. Here we got an accuracy of 64.8% and ROC of 71.53%
Neural Networks: Neural networks are deep learning models inspired by the human brain's structure, consisting of interconnected layers of artificial neurons. They excel at complex pattern recognition and have revolutionized tasks like image and text analysis. Here we have an accuracy of 56.23% and ROC of 71.53%


**Result Interpretation:** (Note: Refer ppt slide 18)

The best model we obtained was gradient boosting with 64.8% accuracy and 71.5% ROC. For this model, we noticed that the employer Name, Naics Code and Job title were the most important features. But since these are features that can’t be changed, the first insight is that the employer and industry an applicant is filing from matters the most. 

Furthermore, we see that prevailing wages is the next important feature. So there are 2 ways a company can use our solution. 

Case Setup: For example, company X has filed 10000 applications and spent ~6000 USD per application. It has a rejection rate of 5%. This translates to 500 rejected applicants leading to a loss of ~3mn USD. 

Solution 1: Using our model, with an accuracy of 64.8%, we can predict that ~325 applications are going to be rejected which is a loss of 1.95mn. So, the company can refrain from filing those applications or file them differently which leads to our second solution

Solution 2: The company can analyse the applications of the rejected vs accepted filings and make necessary changes w.r.t wages and location for a particular application to increase the chances of acceptance

