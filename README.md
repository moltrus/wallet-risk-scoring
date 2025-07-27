# Wallet Risk Scoring

Credit scoring for DeFi wallets using **unsupervised learning with clustering analysis** to assess wallet risk based on transaction behavior patterns.

## Model Architecture

The credit scoring system employs **K-means clustering with anomaly detection** using the following architecture:

### Data Pipeline
```
CSV Wallet Data -> API Transaction Fetch -> Feature Engineering -> Clustering + Anomaly Detection -> Risk Scores
```

### Core Components

#### 1. **Data Collection**
- **Wallet Loading**: Reads wallet addresses from CSV file
- **Transaction Fetching**: Uses Covalent API to retrieve Ethereum mainnet transaction data
- **Rate Limiting**: Implements proper API rate limiting and error handling
- **Data Validation**: Validates API responses and handles missing data

#### 2. **Data Preprocessing**
- **Type Conversion**: Converts string values to appropriate numeric types
- **Missing Value Handling**: Fills missing addresses with 'Unknown' placeholders
- **Duplicate Removal**: Removes duplicate transactions based on wallet ID and transaction hash
- **Temporal Processing**: Converts timestamps to datetime objects for time-based analysis

#### 3. **Feature Engineering**
The model extracts comprehensive wallet-level features by aggregating transaction data:

**Transaction Volume Features:**
- `total_transactions`: Total number of transactions per wallet
- `successful_transactions`: Count of successful transactions
- `failure_rate`: Ratio of failed to total transactions

**Financial Features:**
- `total_value_transferred`: Cumulative transaction value in Wei
- `avg_transaction_value`: Mean transaction value
- `value_volatility`: Standard deviation of transaction values
- `total_fees_paid`: Cumulative gas fees paid
- `transaction_efficiency`: Value transferred per unit of fees paid

**Behavioral Features:**
- `unique_recipients`: Number of distinct receiving addresses
- `unique_senders`: Number of distinct sending addresses
- `avg_gas_price`: Average gas price used
- `gas_price_volatility`: Gas price variation patterns

**Temporal Features:**
- `account_age_days`: Days since first transaction
- `activity_span_days`: Days between first and last transaction
- `active_days`: Number of unique active days
- `avg_daily_transactions`: Average transactions per day
- `avg_daily_value`: Average value transferred per day

#### 4. **Data Scaling and Preprocessing**
- **Imputation**: Uses median imputation for missing values
- **Outlier Handling**: Applies RobustScaler to minimize outlier impact
- **Feature Selection**: Removes features with insufficient variance
- **Infinity Handling**: Replaces infinite values with zeros

#### 5. **Clustering Analysis**
- **Algorithm**: K-means clustering with optimal cluster number detection
- **Optimization**: Uses silhouette analysis, Calinski-Harabasz, and Davies-Bouldin scores
- **Validation**: Multiple clustering quality metrics for robust evaluation
- **Distance Calculation**: Computes distance from cluster centers for refinement

#### 6. **Anomaly Detection**
- **Algorithm**: Isolation Forest with 10% contamination rate
- **Integration**: Combines clustering results with anomaly detection
- **Scoring**: Uses anomaly status as additional risk factor

#### 7. **Risk Score Calculation**
Multi-factor scoring system considering:
- **Cluster Assignment**: Base score from cluster membership (200-800 range)
- **Anomaly Penalty**: 200-point penalty for detected anomalies
- **Failure Rate Penalty**: Up to 300-point penalty based on transaction failures
- **Activity Bonus**: Up to 100-point bonus for high transaction volume
- **Age Bonus**: Up to 50-point bonus for account maturity
- **Distance Penalty**: Up to 100-point penalty for cluster center distance

Final scores range from 100 (highest risk) to 900 (lowest risk).

## Features
- Loads wallet addresses from CSV format
- Fetches real-time transaction data via Covalent API
- Comprehensive feature engineering from transaction patterns
- Unsupervised clustering to identify behavioral groups
- Anomaly detection for outlier identification
- Multi-factor risk scoring algorithm
- Extensive validation and visualization tools

## Requirements
Install dependencies using pip:

```bash
pip install pandas numpy requests scikit-learn matplotlib seaborn
```

## Usage
The system is implemented as a Jupyter notebook for interactive analysis and development.

### Data Processing
1. Load wallet addresses from CSV file
2. Configure Covalent API key as environment variable
3. Fetch transaction data with proper error handling
4. Clean and validate the collected data

### Feature Engineering
Generate comprehensive behavioral features from transaction data:
- Volume and frequency metrics
- Success rate analysis
- Temporal activity patterns
- Gas usage efficiency

### Model Training
Execute unsupervised learning pipeline:
- Determine optimal cluster count using multiple metrics
- Apply K-means clustering with anomaly detection
- Validate clustering quality using silhouette analysis

### Risk Assessment
Calculate final risk scores using multi-factor approach:
- Cluster-based base scoring
- Anomaly detection penalties
- Activity and age bonuses
- Distance-based adjustments

## File Overview
- `wallet_risk_scoring.ipynb`: Main notebook containing the complete pipeline
- `wallets.csv`: Input file containing wallet addresses
- `wallet_risk_scores_final.csv`: Output file with final risk scores

## Model Performance
The model uses multiple validation metrics:
- **Silhouette Score**: Measures cluster separation quality
- **Calinski-Harabasz Score**: Evaluates cluster density and separation
- **Davies-Bouldin Score**: Assesses cluster compactness and separation
- **PCA Visualization**: 2D representation of cluster structure

Cross-validation through multiple clustering metrics ensures robust performance assessment.

## Visualization
The system provides comprehensive visualizations:
- PCA plots showing cluster separation
- Risk score distribution histograms
- Anomaly detection scatter plots
- Cluster characteristic heatmaps
- Activity pattern analysis charts
