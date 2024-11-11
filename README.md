# Forecasting_Retail

Creation of a predictive Machine learning model aimed at forecasting sales for the next 8 days at the store-product level.

## Objective

Our project involves developing machine learning models on a database with three years of historical data to predict sales for the next 8 days at the store-product level.

> [!NOTE]  
> The entire project was developed in Spanish.
> 
> You can access the notebooks directly in this [folder](https://github.com/TonyGonzalezData/Forecasting_Retail/tree/main/03_Notebooks/02_Desarrollo).


## About the Dataset

The data has been created synthetically. It has the following structure: one category > two stores > 10 products per store. 

The reason for storing it in .db format is to closely replicate a real-world retail business environment.

The data was then cleaned, organized, and merged in the data quality phase.


## Project Structure

The development of the Discovery project consists of the following structure:

- **Set up**: We load the data from its original files, merge the necessary data from different tables and extract one dataset for validation and another for working purposes.
- **Data Quality**: Data cleaning, including review of data types and transformation of variables, handling of duplicates and null values.
- **EDA**: Exploraty data analysis of both categorical and numerical variables
- **Data transforation**: Creation of new variables and encoding of categorical variables. 
- **Variable preselection**: We select the most predictive variables. To do this, we choose the most reliable result among those obtained through the methods of Mutual Information, Recursive Feature Elimination, and Permutation Importance.
- **Modeling for regression**: We test the modeling process by first applying it to a single product. Then, we test the process on the entire set and verify that the results align with expectations.
- **Preparation of production code**: we combine all previously developed code, including the modeling, and make it ready for deployment.

## Highlights


This project addresses four distinct challenges:

### 1. How to solve **hierarchical forecasting**?

In the commercial catalog, there are different hierarchical levels, and it may be useful to predict at each of these levels. Since the forecast is probabilistic, predictions at different levels may not always match.

**Solution**: It is necessary to reconcile the results by choosing between a *bottom-up*, *top-down*, or hybrid approach. I opted for the hybrid approach.

### 2. How to solve the problem of **intermittent demand**?

There are periods when sales are zero, without knowing the exact cause:

   - There may have been stock, but no sales.
     
   - The product may have been out of stock.
     
This generates noise in the data.

**Solution**: Use stock-out markers to identify these periods.

### 3. How to solve the issue of **hundreds or thousands of SKUs**?

In sectors like retail, ecommerce, or media, there are usually thousands or even tens of thousands of different products. This volume requires a model for each element to be predicted, which can be too computationally intensive.

**Solution**:

   - Use a *Machine Learning* approach that, once the models are trained, is faster than classic methods.
   - Choose faster models such as **LightGBM**.
   - Model at a higher hierarchical level and disaggregate predictions through a *top-down* approach (without lower-level models).

### 4. How to address the need to **predict multiple days into the future**?

In real-world settings, it’s not enough to predict the next day only; multiple days ahead are needed to allow time for action. Classic models automatically predict multiple days forward, but ML models do not, as they predict record-by-record and often require future information that is unavailable.

**Solution**:

   - *Recursive* approach: develop a single model that, in step 1, predicts for \( t \), then uses that prediction as the basis for \( t+1 \), and so on.
   - Prediction structure:
     - \( \text{Prediction}(t) = \text{model1}(\text{obs}(t-1), \text{obs}(t-2), ..., \text{obs}(t-n)) \)
     - \( \text{Prediction}(t+1) = \text{model1}(\text{pred}(t), \text{obs}(t-1), ..., \text{obs}(t-n)) \)


