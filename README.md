# A/B Testing 

## Project Background

This project is designed to assist a US-based e-commerce company in optimizing promotional strategies through experimentation and data-driven insights. 

This project utilizes ANOVA, Logistic Regression, A/B testing, and Bayesian inference to analyze the impact of different promotional strategies on customer conversion rates. The analysis focuses on identifying the most effective discount structures, assessing the role of urgency in messaging, and determining the optimal communication channel to enhance conversion rates. By analyzing customer interactions to different features, this project aims to enhance conversion rates, improve campaign efficiency, and maximize revenue potential.

The analyses leverage a structured dataset of promotional campaigns and customer activities over an unidentified defined period, enabling a deeper understanding of what drives purchase decisions. The dataset includes details on customer purchases, promotional exposure, and conversion outcomes. It captures key variables such as discount types, urgency messaging, communication channels, and customer segments.

Feature significance and other findings are detailed below. [Recommendations](https://github.com/hallie-marshall/ecommerce-promotional-abtesting/edit/main/README.md#recommendations) are provided for next steps, including strategies for future campaign testing and follow-up projects that build on these insights.


## Data Structure

The singular table used for this project consists of 285,100 rows of promotional details and its results, including:

**CustomerID:** unique identifier for customers who’ve received the promotional message

**ProductID:** unique identifier for products purchased on confirmed customer orders

**EmailSubject:** string of the message title sent to the customer. While this metric is titled as “EmailSubject,” it also applies as the title of the body in an SMS.

**DiscountType:** string to categorize the discount as a dollar amount as “Flat” or a “Percentage”

**UrgencyMessage:** string to categorize the style of the message’s body as either urgent (“Limited Time Only!”) or not urgent (“No Urgency”)

**CommunicationChannel:** string to categorize the promotional medium as “Email” or “SMS”

**EmailOpened:** binary value noting if the Email or SMS was opened

**Clicked:** binary value noting if the website link in the Email or SMS was clicked

**Converted:** binary value noting if the customer proceeded to purchase the promoted product or products 


Additional datasets were uploaded to modify the main table. These include two lists, Top 20 customers and Top 5 products, sourced from the Customer Lifetime Value project. The following columns were added based on these lists:

**CustomerType:** string to categorize the CustomerID as “Top 20” or “Non-Top 20”

**ProductType:** string to categorize the ProductID as “Top 5” or “Non-Top 5”

**Segment:** concatenated string of CustomerType and ProductType to utilize as a singular feature


## Executive Summary
This project analyzes the effectiveness of different promotional strategies in driving customer conversions for an E-Commerce business. By leveraging several models, this project identifies which features, and the combination of these features, yield the highest conversion rates.

To understand which promotional strategies may work best, the following statistical techniques and models were employed:

**ANOVA** to determine if specific features significantly impact conversion rates

**Logistic Regression** to predict the likelihood of conversion based on the combination of features and customer segments

**Bayesian A/B Testing** to quantify the probability that one strategy is more effective than another, reducing uncertainty in decision-making

Key findings that were consistent across all three models include:

- Top20 customers convert almost 2.5 times more frequently than Non-Top20 customers.
-	Flat discounts consistently outperform Percentage discounts for Non-Top20 customers while Percentage discounts perform well for Top20 customers.
-	Email performs slightly better than SMS, but it is not statistically significant.
-	Urgency messaging has no significant impact; however, No Urgency typically outperforms Urgency messaging, unless coupled with SMS.
-	The best combination of features: Top20 customers purchasing Non-Top5 products via SMS with “Your Favorite Product is On Sale!” using a Percentage discount with No Urgency messaging
-	The lowest conversion rate among multi-variates: Non-Top20 customers purchasing Top5 products via Email with “Exclusive Deal for You!” using a Percentage discount and No Urgency messaging
  
By implementing the insights from this study, businesses can:

- Optimize promotional spending by investing in the most effective discount structures
-	Improve conversion rates by using data-driven discounting and messaging strategies
-	Segment customers more effectively, offering the right promotions to the right audience
-	Use Bayesian A/B testing to make quicker, more reliable marketing decisions compared to traditional statistical methods
  
## Insights

### ANOVA
ANOVA was conducted to test whether the provided features (EmailSubject, DiscountType, UrgencyMessage, CommunicationChannel, and Segment) had statistically significant effects on conversion rates.

<ins>**Findings**</ins>
-	The only significant singular features are Segment and Email Subject. However, the interaction between DiscountType, UrgencyMessage, and CommunicationChannel is highly significant (p = 0.010), indicating their combined effect has more influence than an individual feature.
-	Particular combinations of the significant multi-variate (DiscountType, UrgencyMessage, and CommunicationChannel) perform better than others. Flat discounts with No Urgency sent via Email have the highest overall conversion rate (26.32%). For SMS, Percentage discounts with No Urgency sent via SMS have the highest conversion for the CommunicationChannel and the second highest overall converstion rate (25.98%).  
-	While UrgencyMessage is insignificant, No Urgency consistently performs slightly better than Urgency for all Segments.


### Logistic Regression
Logistic Regression was used to determine which promotional factors most strongly predict conversion.

<ins>**Key Definitions**</ins>

**Odds Ratio:** measure of how each promotional factor increases or decreases conversion probability

**ROC-AUC Score:** Evaluates how well the model distinguishes between converters and non-converters

<ins>**Findings**</ins>

- Accuracy is less than 75% (74.86%) and the model's ability to distinguish between customers who convert compared to those who don't convert is only 53.3%. These scores are much lower than recommended for use of this model. Improving performance through class balancing, or trying alternate regression models, is suggested.
- The good news: the Confusion Matrix shows that, while ROC-AUC is 53%, the False Positives - customers who were predicted to convert that did not - are the smallest group at 801 out of 57,020 (1.4%). 
-	The top predictors for conversion are the following: Top20 customers and Non-Top5 products (80%), Top20 customers and Top5 products (79%), Urgency messaging for Top20 customers and Non-Top5 products (43%), Flat discounts for Top20 customers and Top5 products (42%), and Percentage discounts for Top20 customers (31%). 
-	Though EmailSubject was deemed significant, using the line "Your Favorite Product is On Sale!" only has a 2% higher likelihood of conversion.
-	Non-Top20 customers are slightly less likely to purchase Top5 products when there is no Urgency Message (-11%). Non-Top20 customers are less likely to purchase Non-Top5 products where the pricing is Percentage-based (-16%).

Further feature additions, class rebalancing, or alternative model introductions (e.g. XGBoost) could improve predictive accuracy.


### Bayesian A/B Testing
Bayesian A/B Testing was conducted to quantify the probability that one EmailSubject, DiscountType, UrgencyMessage, or CommunicationChannel outperforms another. These are also tested as multi-feature interactions.

<ins>**Findings**</ins>

**Email Subject:** The probability that the Email Subject "Your Favorite Product is On Sale!" outperform "Exclusive Deal for You!" is 9.27%, with conversion rates of 26.02% for "Your Favorite Product is On Sale!" and 25.68% for "Exclusive Deal for You!".

**Discount Type:** The probability that Percentage discounts outperform Flat discounts is 9.78%, with conversion rates of 25.96% for Flat discounts and 25.74% for Percentage discounts.

**Urgency Message:** The probability that Urgency is better than No Urgency is 30.82%, which is not considered significant. No Urgency messaging converts at 25.81% while No Urgency messaging converts at 25.89%.

**Communication Channels:** The probability that Email performs better than SMS is 33.59%, which is not considered significant. Email converts at 25.82% while SMS converts at 25.89%.

**Multi-Feature Interactions**

The following interaction groups were included: 
Segment and DiscountType;
Segment and UrgencyMessage;
CommunicationChannel, UrgencyMessage, and DiscountType


- The probability that Flat discounts work better than Percentage discounts for Non-Top20 customers to convert with Non-Top5 products is 93.5%. This confirms that Flat discounts should be prioritized for Non-Top20 customers. 
-	The combination of No Urgency with a Flat discount sent via Email is consistently the highest conversion rate and significantly high for probabilities.
-	The probability that Urgency Messaging performs better than No Urgency is 49.1%, meaning there is no significance. Urgency Messaging is, however, more significant for probability when it is included in SMS message and a Flat discount. 
-	For Top20 customers purchasing non-Top5 products, Percentage discounts have significant probability of conversion compared to Flat discounts. These customers also prefer No Urgency. Whereas, Non-Top20 customers prefer Flat discounts and Urgency messaging for non-Top5 products.
-	Top20 customers purchasing Top5 products react more favorably to Percentage discounts and No Urgency, while Non-Top20 customers have a higher probability of converting when presented Flat discounts with No Urgency.
  
## Recommendations

-	Incorporate additional datasets to refine existing models and introduce advanced regression techniques like XGBoost and Random Forest to improve classification accuracy.
-	Utilize Multi-Armed Bandit (MAB) algorithms to dynamically allocate promotional offers based on real-time performance, optimizing marketing spend and increasing conversion rates.
-	Apply causal inference techniques to isolate the true effect of promotional experiments and refine future marketing investments.
-	Assess the impact of different discount structures on demand by modeling price elasticity and customer response to pricing changes, ensuring optimal promotional effectiveness.
-	Implement a Marketing Mix Model (MMM) to measure the effectiveness of marketing channels in driving conversions, predicting the impact of frequency and recency adjustments, and maximizing ROI.
-	Further segment customers through predictive behavior analysis, leveraging hierarchical clustering and machine learning models to deliver hyper-personalized promotions.


 ## Assumptions and Caveats
 
-	Data was only used for US-based customers. Including foreign customer data may lead to differing results.
-	The dataset is based on historical campaigns and does not consider new customers and their behaviors.
-	Seasonality or past events cannot be considered as time-series information is not available for the data.
-	Models do not account for external influences such as economic trends, competitor activity, or shifts in purchasing behavior.

