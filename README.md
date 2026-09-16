# Insurance Cross-Sell Prediction API

A production-oriented machine learning application for predicting whether an existing insurance customer is likely to purchase an additional vehicle insurance policy.

The project combines a machine learning classification pipeline with a **FastAPI REST API** and **Docker containerization**, making the trained model accessible as a reproducible prediction service.

---

## Project Overview

Insurance companies often have existing customers who may be interested in purchasing additional insurance products. Identifying customers with a higher probability of conversion can help prioritize marketing and cross-selling campaigns.

This project builds a binary classification system that predicts whether a customer is likely to respond positively to a vehicle insurance cross-sell offer.

### Problem

Given customer demographic, vehicle, policy and historical information, predict:

* `0` → Customer is unlikely to purchase
* `1` → Customer is likely to purchase

The trained model is exposed through a REST API using FastAPI and packaged inside a Docker container for reproducible deployment.

---

## Architecture

```text
                    Customer Data
                         │
                         ▼
                Data Preprocessing
                         │
                         ▼
                 Feature Engineering
                         │
                         ▼
                  Model Training
                         │
                         ▼
                  Model Evaluation
                         │
                         ▼
                  Saved ML Pipeline
                         │
                         ▼
                   FastAPI Server
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
           /predict    /health   /model-info
              │
              ▼
        Prediction Response
              │
              ▼
       Docker Container
```

---

## Dataset

The dataset contains customer-level insurance information including:

| Feature                | Description                                                |
| ---------------------- | ---------------------------------------------------------- |
| `id`                   | Customer identifier                                        |
| `Gender`               | Customer gender                                            |
| `Age`                  | Customer age                                               |
| `Driving_License`      | Whether the customer has a driving license                 |
| `Region_Code`          | Customer region                                            |
| `Previously_Insured`   | Whether the customer already has vehicle insurance         |
| `Vehicle_Age`          | Age of the customer's vehicle                              |
| `Vehicle_Damage`       | Whether the vehicle has previously been damaged            |
| `Annual_Premium`       | Annual insurance premium                                   |
| `Policy_Sales_Channel` | Channel used to reach the customer                         |
| `Vintage`              | Number of days associated with the customer's relationship |
| `Response`             | Target variable                                            |

The target variable is `Response`.

---

## Machine Learning Pipeline

The project follows a structured machine learning workflow:

1. Load the dataset
2. Perform exploratory data analysis
3. Check data quality and missing values
4. Separate features and target
5. Split data into training, validation and test sets
6. Perform preprocessing and feature engineering
7. Train the classification model
8. Evaluate model performance
9. Serialize the trained model/pipeline
10. Load the model through FastAPI
11. Serve predictions through a REST endpoint

---

## Handling Class Imbalance

The target variable is imbalanced, with substantially more negative responses than positive responses.

Therefore, model evaluation is not based only on accuracy.

The project considers classification metrics such as:

* Precision
* Recall
* F1-score
* ROC-AUC
* Confusion Matrix

This is important because a cross-selling system needs to distinguish between customers who are likely and unlikely to respond to an offer.

---

## FastAPI

The trained model is served through a REST API built with FastAPI.

### Available Endpoints

#### `GET /`

Basic API health/status endpoint.

#### `POST /predict`

Accepts customer information and returns the model prediction.

Example request:

```json
{
  "Gender": "Male",
  "Age": 35,
  "HasDrivingLicense": 1,
  "RegionID": 28,
  "Switch": 0,
  "PastAccident": "Yes",
  "AnnualPremium": 32000
}
```

Example response:

```json
{
  "predicted_class": 1
}
```

FastAPI also provides interactive API documentation through Swagger UI.

---

## Docker

The application is containerized using Docker.

Docker packages:

* Python runtime
* application code
* trained model
* Python dependencies
* FastAPI application

This allows the API to run consistently across different environments.

### Build the Docker Image

```bash
docker build -t insurance-cross-sell-api .
```

### Run the Container

```bash
docker run -p 8000:8000 insurance-cross-sell-api
```

The API can then be accessed locally at:

```text
http://localhost:8000
```

Swagger documentation:

```text
http://localhost:8000/docs
```

---

## Project Structure

```text
Insurance-Cross-Sell-Pred/
│
├── data/
│
├── models/
│   └── model.pkl
│
├── notebooks/
│
├── src/
│   ├── data_processing.py
│   ├── train.py
│   └── evaluate.py
│
├── api/
│   ├── main.py
│   └── schemas.py
│
├── tests/
│
├── Dockerfile
├── requirements.txt
├── .dockerignore
├── .gitignore
└── README.md
```

> The exact directory structure may change as the project evolves.

---

## Technologies Used

#
