# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
```
import pandas as pd
import numpy as np
from scipy import stats

def initial_data_info(df):
    print("Info:")
    print(df.info())
    print("\nDescribe:")
    print(df.describe())
    print("\nNull values per column:")
    print(df.isnull().sum())
    print('=' * 40)

def remove_nulls(df):
    return df.dropna()

def remove_outliers_iqr(df, columns):
    for col in columns:
        Q1 = df[col].quantile(0.25)
        Q3 = df[col].quantile(0.75)
        IQR = Q3 - Q1
        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR
        df = df[(df[col] >= lower) & (df[col] <= upper)]
    return df

def remove_outliers_zscore(df, columns, thresh=3):
    for col in columns:
        z = np.abs(stats.zscore(df[col].dropna()))
        filtered_entries = (z < thresh)
        valid_indices = df[col].dropna().index[filtered_entries]
        df = df.loc[valid_indices]
    return df

file_names = [
    "Data_set.csv",
    "heights.csv",
    "iris.csv",
    "Loan_data.csv",
    "SAMPLEIDS.csv"
]

for file in file_names:
    print(f'Processing file: {file}')
    df = pd.read_csv(file)
    
    print("Initial Info")
    initial_data_info(df)
    
    df_no_nulls = remove_nulls(df)
    print("After Removing Nulls")
    print(df_no_nulls.shape)
    
    numeric_cols = df_no_nulls.select_dtypes(include=[np.number]).columns.tolist()
    
    df_iqr = remove_outliers_iqr(df_no_nulls.copy(), numeric_cols)
    print("After Removing Outliers (IQR method):", df_iqr.shape)
    
    df_z = remove_outliers_zscore(df_no_nulls.copy(), numeric_cols)
    print("After Removing Outliers (Z-score method):", df_z.shape)
    print('=' * 80)

```
#Result/Output

<img width="829" height="680" alt="Screenshot 2025-10-07 203304" src="https://github.com/user-attachments/assets/964b911b-44a6-4dcb-a22d-e7a7f6293bb6" />
            <img width="818" height="579" alt="Screenshot 2025-10-07 203326" src="https://github.com/user-attachments/assets/b1058294-8d7b-41d8-8e77-51c218aad238" />
            <img width="939" height="746" alt="Screenshot 2025-10-07 203337" src="https://github.com/user-attachments/assets/e7f812d0-9ee5-466b-9174-9cff3ca5e449" />
            <img width="871" height="776" alt="Screenshot 2025-10-07 203349" src="https://github.com/user-attachments/assets/3ac01bea-2fea-445e-8d10-dd8db83e8941" />
            <img width="996" height="624" alt="Screenshot 2025-10-07 203406" src="https://github.com/user-attachments/assets/d3095dbe-392a-4c95-ad2a-51715e9a981c" />
            <img width="1072" height="763" alt="Screenshot 2025-10-07 203417" src="https://github.com/user-attachments/assets/08f3021b-7d72-45bf-94de-964d06111272" />
            <img width="1156" height="609" alt="Screenshot 2025-10-07 203528" src="https://github.com/user-attachments/assets/13bd6cb2-8d97-4a14-9cfb-7a8ae8bdb6ed" />
            <img width="922" height="726" alt="Screenshot 2025-10-07 203542" src="https://github.com/user-attachments/assets/92dc00d5-4c43-426d-8748-1bd575bdf80f" />
            <img width="1086" height="469" alt="Screenshot 2025-10-07 203551" src="https://github.com/user-attachments/assets/cc965f42-8f78-4b10-8ec1-0b03932db423" />

            
 Result/Output

            







                    


