# Data Directory

This directory contains all the data files used in the churn prediction project, organized by processing stage and data type.

## Directory Structure

```
data/
├── raw/                    # Original, immutable data
├── external/              # External data sources
├── interim/               # Intermediate data that has been transformed
├── processed/             # Final, canonical data sets for modeling
├── data_dictionary.yaml   # Comprehensive data dictionary
└── README.md              # This file
```

## Data Flow

The data processing pipeline follows this flow:

1. **Raw Data** → 2. **Interim Data** → 3. **Processed Data**
   - External data is merged at various stages

### Raw Data (`data/raw/`)

Contains the original, unprocessed data files:

- `customer_data.csv` - Primary customer demographics and subscription information
- `usage_data.csv` - Customer usage patterns and behavior metrics
- `transaction_data.csv` - Billing and payment transaction information

### External Data (`data/external/`)

Contains data from external sources:

- `market_data.csv` - Market conditions and competitive information
- `demographic_data.csv` - Regional demographic and socioeconomic data

### Interim Data (`data/interim/`)

Contains data that has been cleaned and partially processed:

- `cleaned_customer_data.csv` - Customer data with cleaned and encoded categorical variables
- `feature_engineered_data.csv` - Data with derived features and risk scores

### Processed Data (`data/processed/`)

Contains the final datasets ready for machine learning:

- `train_features.csv` - Training features (normalized and one-hot encoded)
- `train_labels.csv` - Training labels (churn indicators)
- `test_features.csv` - Test features
- `test_labels.csv` - Test labels
- `validation_features.csv` - Validation features
- `validation_labels.csv` - Validation labels

## Data Description

### Customer Data Features

#### Demographics
- `age`: Customer age in years
- `gender`: Customer gender (Male/Female)
- `senior_citizen`: Whether customer is a senior citizen (0/1)
- `partner`: Whether customer has a partner (Yes/No)
- `dependents`: Whether customer has dependents (Yes/No)

#### Account Information
- `customer_id`: Unique customer identifier
- `tenure_months`: Number of months as customer
- `contract_type`: Contract type (Month-to-month, One year, Two year)
- `payment_method`: Payment method preference
- `paperless_billing`: Whether customer uses paperless billing
- `monthly_charges`: Monthly subscription fee
- `total_charges`: Total amount charged over tenure

#### Services
- `phone_service`: Whether customer has phone service
- `multiple_lines`: Whether customer has multiple phone lines
- `internet_service`: Type of internet service (DSL, Fiber optic, No)
- `online_security`: Whether customer has online security add-on
- `online_backup`: Whether customer has online backup add-on
- `device_protection`: Whether customer has device protection add-on
- `tech_support`: Whether customer has tech support add-on
- `streaming_tv`: Whether customer has streaming TV add-on
- `streaming_movies`: Whether customer has streaming movies add-on

#### Target Variable
- `churn`: Whether customer churned (0 = No, 1 = Yes)

### Usage Data Features

- `data_usage_gb`: Monthly data usage in gigabytes
- `call_minutes`: Total call minutes used
- `sms_count`: Total SMS messages sent
- `support_tickets`: Number of support tickets raised
- `late_payments`: Number of late payments
- `service_outages`: Number of service outages experienced
- `avg_session_duration`: Average session duration in minutes
- `total_logins`: Total number of account logins
- `device_count`: Number of devices connected to account

### Transaction Data Features

- `transaction_id`: Unique transaction identifier
- `transaction_date`: Date of transaction
- `amount`: Transaction amount in USD
- `transaction_type`: Type of transaction (Monthly Bill, Setup Fee, etc.)
- `status`: Transaction status (Completed, Failed, Pending)
- `billing_cycle_day`: Day of month for billing cycle
- `discount_applied`: Discount amount applied
- `tax_amount`: Tax amount charged

### Feature Engineering

The processed data includes several engineered features:

- `avg_monthly_spend`: Average monthly spending (total_charges / tenure_months)
- `tenure_category`: Categorical tenure (Short/Medium/Long)
- `payment_risk_score`: Risk score based on payment behavior (0-1)
- `service_complexity_score`: Score based on number of services (0-1)
- `engagement_score`: Customer engagement score (0-1)
- `satisfaction_proxy`: Proxy for customer satisfaction (0-1)
- `churn_risk_score`: Calculated churn risk score (0-1)

## Data Quality

### Data Validation Rules

1. **Customer ID**: Must be unique and follow format CUST_XXX
2. **Age**: Must be between 18 and 100
3. **Tenure**: Must be non-negative
4. **Charges**: Must be positive values
5. **Categorical Variables**: Must match predefined values
6. **Dates**: Must be valid dates in YYYY-MM-DD format

### Known Data Issues

1. Some customers have missing `total_charges` values
2. New customers may have tenure_months = 0
3. Some transactions may have status = "Pending"

## Usage Instructions

### Loading Data

```python
import pandas as pd

# Load raw data
customer_data = pd.read_csv('data/raw/customer_data.csv')
usage_data = pd.read_csv('data/raw/usage_data.csv')
transaction_data = pd.read_csv('data/raw/transaction_data.csv')

# Load processed data for modeling
train_features = pd.read_csv('data/processed/train_features.csv')
train_labels = pd.read_csv('data/processed/train_labels.csv')
```

### Data Processing Pipeline

1. **Data Cleaning**: Handle missing values, fix data types
2. **Feature Engineering**: Create derived features
3. **Encoding**: Convert categorical variables to numeric
4. **Scaling**: Normalize numerical features
5. **Splitting**: Split into train/validation/test sets

### DVC Integration

This project uses DVC (Data Version Control) to track data files:

```bash
# Pull latest data
dvc pull

# Check data status
dvc status

# Add new data files
dvc add data/raw/new_data.csv
```

## Data Lineage

```
Raw Data Sources
├── Customer Database → customer_data.csv
├── Usage Logs → usage_data.csv
├── Billing System → transaction_data.csv
├── Market Research → market_data.csv
└── Census Data → demographic_data.csv

Processing Steps
├── Data Cleaning
├── Feature Engineering
├── Data Encoding
├── Normalization
└── Train/Test Split
```

## Security and Privacy

- All customer data is anonymized with synthetic IDs
- No personally identifiable information (PII) is stored
- Data access is restricted to authorized personnel
- Regular data audits are performed

## Contact

For questions about the data or to request additional datasets, please contact the Data Science team.

## Version History

- v1.0: Initial dataset creation
- v1.1: Added usage and transaction data
- v1.2: Added external market and demographic data
- v1.3: Enhanced feature engineering