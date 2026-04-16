# The Shopee Affiliate Playbook 🛒🍊
> This is one of the projects I am most proud of. I worked on it from data collection to modeling, and it can help me earn more income.

## Project Overview 
I earn extra income through Shopee Affiliate by posting product reviews in rented Facebook groups. I receive commission when users purchase products through my affiliate links.   
To optimize this process, I developed this Machine Learning project to predict whether a Shopee Affiliate order will generate **High or Low commission**, supporting smarter decisions on *when* and *what* to post on Facebook groups.

## Business Objective
Use Machine Learning  to assist in selecting the optimal **"posting time"** and **"product"** for Shopee Affiliate promotions across Facebook groups — with the goal of maximizing commission income.

## ML Problem
**Binary Classification** — Predict the probability that an order will generate a **High commission**, using features related to product, Facebook group, and time-based behavior.
- `1` = High Commission (above median)
- `0` = Low Commission (at or below median)

## Dataset
### Data Collection Period
**March 2026 (31 days)** — Tracking via `sub_id` embedded in every affiliate link.

### orders_report_Mar_2026.csv
- **Description:**  Each row represents a Shopee Affiliate order, including product details, timestamps, order status, Sub_id1, Sub_id2 and net commission earned.
- **Rows:** 1,450
- **Columns:** 47

### clicks_report_Mar_2026.csv
- **Description:**  Each row represents a user click on an affiliate link, including timestamp, location, and Sub_id indicating product and Facebook group.
- **Rows:** 8,969
- **Columns:** 5


### Sub_id Tracking Structure
| Sub_id | Description |
|---|---|
| `Sub_id1` | Product name promoted in the post (`colgateOpticWhite`, `dentistemax`, `judydoll`, etc.) |
| `Sub_id2` | Facebook group color code (`white`, `purple`, `red`, `blue`) |

### Facebook Groups
| Color | Group Name | Monthly Cost |
|---|---|---|
| `white` | แต่งห้อง แต่งบ้าน แต่งเห๊อะ ขายทุกสิ่ง รีวิวทุกอย่าง | 200 ฿/month |
| `purple` | ของดีต้องแชร์ Shopee / Lazada | Package |
| `red` | ช้อปขั้นเทพ | Package |
| `blue` | รีวิวครอบจักรวาล | Package |
> **Note:** `purple + red + blue` are rented as a bundle at 1,000 ฿ / 100 days.

## Tools & Tech
- JupyterLab
- Python
- Pandas, NumPy
- Matplotlib, Seaborn
- Scikit-learn
- joblib

## Project Structure

```
.
├── data/
│   ├── raw/
│   │   ├── orders_report_Mar_2026.csv      
│   │   └── clicks_report_Mar_2026.csv      
│   └── processed/
│       ├── orders_cleaned.csv
│       ├── X_train.csv
│       ├── X_test.csv
│       ├── y_train.csv
│       └── y_test.csv
│
├── models/
│   ├── encoder.pkl 
│   └── best_model_random_forest.pkl
│
├── outputs/
│   └── model_results.csv   
│
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_data_preparation.ipynb
│   ├── 03_modeling.ipynb
│   ├── 04_data_evaluation.ipynb
│   └── 05_deployment.ipynb
│
└── README.md
```

## CRISP-DM Workflow
### 1. Business Understanding
- Personal Shopee Affiliate side income via Facebook group posts
- Commission rate varies per product and per seller, even for the same item
- KPI: `ค่าคอมมิชชันสุทธิ(฿)` 

### 2. Data Understanding (`01_data_understanding.ipynb`)
**Key Findings:**
- Commission distribution is **right-skewed** (median = 0.51 ฿, mean = 5.18 ฿) income depends heavily on a small number of high-value orders
- **Facebook group `purple`** generates the highest number of orders, clicks, and total commission
- Top promoted products: `colgateOpticWhite`, `judydoll`, `nude`, `oppo`
- Price and commission have a **moderate positive correlation** (r = 0.56)

### 3. Data Preparation (`02_data_preparation.ipynb`)

### 4. Modeling (`03_modeling.ipynb`)  
**Best Model: Random Forest**    
highest Test Accuracy and F1-score via ensemble learning.   

### 5. Evaluation (`04_data_evaluation.ipynb`)
**Key Insight**    
Posting **time** matters more than product or group selection for predicting high commission.

**Generalization Check:**    
CV Mean (0.6018),  Test Acc (0.6154) →  Good generalization, model is stable.

### 6. Deployment (`05_deployment.ipynb`)
**Pipeline:**
1. Load `best_model_random_forest.pkl` and `encoder.pkl`
2. Validate input columns
3. One-Hot Encode categorical features
4. Align feature columns to match model's training schema
5. Return predicted class + probability + business interpretation

## Business Recommendations
1. **Focus on Facebook group `purple`** — highest ROI (3,137.6%) and best conversion from clicks to orders
2. **Post at 6:00 AM and 6:00 PM** — time of click is the strongest predictor of commission level
3. **Avoid posting at 11:00 AM** — lowest probability of generating high commission
4. **Prioritize products:** `colgateOpticWhite` and `nude` — highest purchase frequency and commission signal
5. **The purple+red+blue package is worth the investment** — all three groups are profitable
