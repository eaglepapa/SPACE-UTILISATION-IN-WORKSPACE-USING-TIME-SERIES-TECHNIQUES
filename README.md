# Space Utilisation in Workspace using Time Series Techniques

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Techniques](#techniques)
- [Data Cleaning/Preparation](#data-cleaningpreparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results/Findings](#resultsfindings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)
- [Contact](#contact)

  
### Project Overview
---
Making good use of space is important in the workplace. This helps to make good use of available space. At the workplace there is a challenge with utilising space, often leading to unnecessary costs and inefficient use of resources. My main objective in this project was to forecast the minimum number of meeting rooms required for all bookings at a particular point in time.

![Capture46](https://github.com/user-attachments/assets/c74011a5-ba96-432f-b5dc-26e5e5e981a6)


### Data Sources
Bookings Data: The dataset was provided by a company that offers booking solutions for healthcare clients. Due to confidentiality agreements, I do not have permission to share the data.

![Capture26](https://github.com/user-attachments/assets/44897a68-78d2-4e28-834f-d8255b3dce81)


### Tools
- SQLiteStudio – It was used to perform SQL queries to merge multiple datasets depending on the business questions.
  [Download here](https://www.sqlitestudio.pl/)

- Google Colab – It was used in this project for its free cloud computing, Python support and ease of access to libraries without the need for local installation.
[Download here](https://colab.research.google.com/)


### Techniques
- Pandas – It was used to perform Exploratory Data Analysis (EDA) to better understand the merged dataset.
  [Download here](https://pandas.pydata.org/)

- Statsmodels – It was a key library in this project, as all the time series forecasting models used were implemented through it.
[Download here](https://www.statsmodels.org/stable/index.html)

- Matplotlib – It was used in this project for visualizing time series data, allowing for clear interpretation of trends, patterns, and model results.
[Download here](https://matplotlib.org/)

- SciPy – It was used in this project because of its advanced mathematical functions which support statistical analysis.
[Download here](https://scipy.org/)



#### Data Cleaning/Preparation
In the initial data preparation please, I performed the following tasks:
1.	Data loading and inspection.
2.	Performing SQL queries to merge tables based on project objective
3.	Handling missing values.
4.	Data cleaning and formatting.
5.	Checking for stationarity in time series data
6. 	Building the model
7.	Forecasting room demand

   
### Exploratory Data Analysis
EDA involved exploring the movie data to answer key questions, such as: 
1.	How accurate are time series models in making predictions related to space utilization?
2.	How can time series models be optimized to improve space utilization efficiency?
3.	What future values can be forecasted from the collected dataset?

![Capture3 2](https://github.com/user-attachments/assets/bef4899c-6ade-4aea-a259-616139d617c8)

![Capture1](https://github.com/user-attachments/assets/3ded4e76-2e66-446f-a8fa-729fd8e0e009)

![Capture31](https://github.com/user-attachments/assets/b73fa0ed-18d1-4ad6-bf87-3909e599e81d)



  
### Data Analysis 
```sql
CREATE TABLE bookingoccasion_rooms AS
SELECT bookingoccasion.BookingOccasion_Id, bookingoccasion.BookingOccasion_BookingId, bookingoccasion.BookingOccasion_StartDateTime, bookingoccasion.BookingOccasion_EndDateTime
FROM booking 
INNER JOIN  bookingoccasion ON booking.Booking_Id = bookingoccasion.BookingOccasion_BookingId
INNER JOIN area_bookableunit ON area_bookableunit.BookableUnit_Id = booking.Booking_BookableUnitId
WHERE bookingoccasion.BookingOccasion_BookingOccasionStateId = '1' AND bookingoccasion.BookingOccasion_CancellationReasonId = ''
ORDER BY BookingOccasion_Id ASC;
```

```python
# Train SARIMA model
order = (3, 1, 3)  # ARIMA(p, d, q)
seasonal_order = (1, 1, 3, 13)  # SARIMA with seasonal component for hourly data
model = SARIMAX(train['number_of_rooms'], exog=train[['weekday', 'working_hour', 'time_range']],  order=order, seasonal_order=seasonal_order, enforce_stationarity=False, enforce_invertibility=False)
result = model.fit()
```


### Results/Findings
The analysis results are summarized as follows:
1.	The accuracy of the time series model was evaluated using RMSE. The model with the least error was adopted as the best model. **SARIMAX was chosen as the best model because it had the least RMSE of 7.77.**

![Capture40](https://github.com/user-attachments/assets/2ca23790-aa1f-4e50-a59c-712224afdc84)


2.	Time series models can be optimized by analysing the time series data critically to identify if there are external factors that affect the time series data. This is       because exogenous variables if exist affect the accuracy of the model. Also selecting the latest date range for the time series analysis is very necessary for the accuracy of the time series model.

![Capture](https://github.com/user-attachments/assets/5cfa1a91-8e28-4a96-8f52-093bb709e684)


![Capture2](https://github.com/user-attachments/assets/00046603-e977-4553-829a-df9914a3089e)


3.	The SARIMAX model which was adopted as the best model was used to forecast for future values from 26/11/2021 18:00 to 30/11/2021 20:00. This is because the collected data used for the analysis ended in 26/11/2021 17:00. This produced 55 future data periods; however, many values were not forecasted due to a sharp decline observed in the actual data.

![Capture46](https://github.com/user-attachments/assets/738dd330-e5a3-4c64-bee7-44d68c74cece)


### Recommendations
Hybrid model can be implemented in this project to improve the model accuracy.


### Limitations
This project used data from 2016 to 2021; however, incorporating more up-to-date datasets would be essential for generating reliable forecasts. 

Conducting the project using current data would offer valuable insights into the model's performance under present-day conditions.

### References
1.	Adhikari, R. and Agrawal, R. K. (2013) An Introductory Study on Time Series Modeling and Forecasting. [online]. Available from: http://arxiv.org/abs/1302.6613. [Accessed 7 Apr 2025].
2.	Adineh, A. H., Narimani, Z. and Satapathy, S. C. (2020) Importance of data preprocessing in time series prediction using SARIMA: A case study. International Journal of Knowledge-Based and Intelligent Engineering Systems, 24 (4), 331–342.
3.	Adineh, A. H., Narimani, Z. and Satapathy, S. C. (2020) Importance of data preprocessing in time series prediction using SARIMA: A case study. International Journal of Knowledge-Based and Intelligent Engineering Systems, 24 (4), 331–342.
4.	Albashir, N. A. and Danial, H. (2023) Forecasting Visitors in Smart Building Environments : Modeling and estimation of the number of guests using SARIMAX [online]. Available from: https://urn.kb.se/resolve?urn=urn:nbn:se:hh:diva-50892 [Accessed 3 Apr 2025].
5.	Ampountolas, A. (2025) Addressing complex seasonal patterns in hotel forecasting: a comparative study. Journal of Revenue and Pricing Management, 24 (2), 143–152.


## Contact
Feel free to reach out!  
📧 Email: [oduroprince08@gmail.com](mailto:oduroprince08@gmail.com) &nbsp;|&nbsp; 🔗 LinkedIn: [linkedin.com/in/oduroprince24](https://linkedin.com/in/oduroprince24)


🚀
📊
📈
🧠

Thanks for visiting! 😄
