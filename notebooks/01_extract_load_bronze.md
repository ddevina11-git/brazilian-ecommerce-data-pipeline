# 01 - Extract & Load to Bronze Lakehouse

## Overview
This notebook downloads the Olist dataset from Kaggle and uploads all 9 CSV files to the Bronze Lakehouse in Microsoft Fabric.

## Steps:
1. Download dataset from Kaggle
2. Unzip files
3. Upload to Bronze Lakehouse
4. Verify data loaded

---

# Step 1: Download Dataset from Kaggle

```python
# Install kagglehub if not already installed
!pip install kagglehub

import kagglehub
import pandas as pd
import os

# Download the Olist dataset
print("Downloading Olist dataset from Kaggle...")
path = kagglehub.dataset_download("olistbr/brazilian-ecommerce")
print(f"Dataset downloaded to: {path}")

# List all files in the downloaded folder
files = os.listdir(path)
print(f"Files found: {files}")
```
