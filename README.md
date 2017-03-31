# Alixpartners-TribeHacks-Challenge-2017

AlixPartners invites your team to solve the following problem set within the guidelines outlined below. By the end of this exercise, your team will gain insight into what risk indicators one must consider in detecting fraud as it relates to online transactions – an increasingly important issue as the volume of mobile transactions increase on an annual basis (just look at the growth of Amazon as an example). 

Fraud detection is typically handled as a binary classification problem, but the class population is unbalanced because instances of fraud are usually very rare compared to the overall volume of transactions. Moreover, when fraudulent transactions are discovered, the business typically takes measures to block the accounts from transacting to prevent further losses. Therefore, losses to the issuer must be calculated in terms of both real losses & opportunity cost of these actions. 

For the purposes of this challenge, only real losses are within scope & required for calculation – however extra consideration will be given to any team with the ability to articulate the potential lost profits based on current standard interest rates & consumer payment habits. MySQL must be utilized for all data management purposes in this challenge.

This problem takes two datasets as inputs: the transactional data – a sample of 200,000 transactions, and the transaction-level fraud data. The challenge will be to identify and present the risk factors for e-commerce in a clear and concise manner – visualizations & presentation aids included.

_____

**Step 0: Setup & Preparation**
 The first step is to install MySQL – instructions for all platforms have been provided below:
 
https://dev.mysql.com/doc/refman/5.7/en/installing.html
 
 The next step is to import the two datasets provided into your MySQL Database.
To do this, please download and create a single table of the 3 transactional datasets provided on Github. All datasets have been provided in a CSV format with headers in the first row. 

**Untagged Transaction Datasets:**

*UntaggedTransactions_Pt1.csv*

*UntaggedTransactions_Pt2.csv*

*UntaggedTransactions_Pt3.csv*

Once these 3 files listed above have been imported, you should create a single table titled UntaggedTransactions consisting of the all untagged transactions.

The second table you will create is titled FraudTransactions consisting of the subset flagged as fraudulent transactions – this dataset has also been provided in CSV format as *FraudulentTransactions_2017.csv*

______

**Step 1: Tagging**
In this step, we tag the untagged data on account level based on the fraud data. The tagging logic is the following. In fraud data, we group it by account ID and sort by time, thus, we have the fraud time period for each fraud account. For each transaction in untagged data, if the account ID is not in fraud data, this transaction is labeled as non fraud (label = 0); if the account ID is in fraud data and the transaction time is within the fraud time period of this account, this transaction is labeled as fraud (label = 1); if the account ID is in fraud data and the transaction time is out of the fraud time period of this account, this transaction is labeled as pre-fraud or unknown (label = 2) which will be removed later. Besides, we will do some re-formatting for some columns. For example, uniform the transactionTime filed to 6 digits.

_______


**Step 2: Preprocessing**
In this step, do the following:
•	Clean the tagged data (filling missing values and removing transactions with invalid transaction time and amount) – basic data integrity checks, removing invalid observations.

________
**Step 3: Create Risk Tables**
In this step, we create risk tables for bunch of categorical variables, such as location related variables. This is related to the method called "weight of evidence". The risk table stores risk (log of smoothed odds ratio) for each level of one categorical variable. For example, variable X has two levels: A and B. For each level (e.g., A), we compute the following:

•	Total number of good transactions, **n_good(A)**,

•	Total number of bad transactions, **n_bad(A)**.

•	The smoothed odds, **odds(A) = (n_bad(A)+10)/(n_bad(A)+n_good(A)+100)**.

•	The risk of level A, **risk(A) = log(odds(A)/(1-odds(A))**.

Thus, the risk table of variable X looks like the following:


_____________
|X	|Risk   |
|---|-------|
|A	|Risk(A)|
|B	|Risk(B)|
-------------
With the risk table, we can assign the risk value to each level. This is how we transform the categorical variable into numerical variable. One thing need to be mentioned, if a new level of a categorical values occurs in test dataset, we use the average risk to fill this new level. For the example, if in test set, variable X has new level "C", we can't find any risk value in the risk table. Thus, we use (risk(A)+risk(B))/2 as risk(C).

______

**Step 4: Draw Your Conclusions**
What conclusions can be drawn from your results? What is the economic impact of the fraudulent transactions (there are many levels upon which this can be comparatively measured or sliced). Are there certain indicators which would be prudent to search for in future transactions? Markets you would advise one to be cautious of? 


______

**Step 5: Visualize Your Categorical Risk Factors**
Utilizing Tableau or your choice of open-source visualization software, add some insight to your findings – what can we conclude from your results? (i.e. Location-based risk, payment / authentication method, etc.) Extra care should be taken to explain your results in a manner best suited to a crowd of business executives - not necessarily stats wizards or dev gurus – while still delving deep enough to provide insight for those with a strong quantitative background.  


______

### Judging Criterion:

1.	**Innovative SQL Code:** Creative & Efficient SQL Code to draw your conclusions.

2.	**Accuracy of Conclusions:** Are the conclusions reached supported by the underlying datasets?

3.	**Presentation & Visualizations:** Can your team present the findings to a team of executives with business & finance backgrounds – yet provide enough detail for a quantitative mind to take a closer look (and perhaps run an independent analysis?)
