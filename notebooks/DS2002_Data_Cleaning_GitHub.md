# DS 2002 Midterm Project — Data Cleaning

Data Cleaning workflow

## Setup and Initial Data Review

Import the required libraries, load the CSV datasets from GitHub, and inspect basic data-quality issues.

### Import libraries

Load Pandas, NumPy, Sqlite 3 etc...

```python
import pandas as pd
import numpy as np
import urllib.request
import os
import sqlite3
```

### Load source CSV files

Read the transaction, product, store, and distribution-center CSVs from the project repository.

```python
# providing a base URL to our Data folder and then specific directions to each individual dataset file
# Omitting "Inventory and Sales" for right now because it is a different file type
base_url = "https://raw.githubusercontent.com/junseo25/ds2002-walmart-hurricane-project/main/data"

transactions = pd.read_csv(f"{base_url}/walmart_transactions.csv")

product = pd.read_csv(f"{base_url}/product_catalog.csv")

store = pd.read_csv(f"{base_url}/store_locations.csv")

distribution = pd.read_csv(f"{base_url}/distribution_centers.csv")
```

### Assess data quality

Count rows, duplicate records, and missing values in each CSV dataset.

```python
#Creating a dictionary first
datasets = {
    "transactions": transactions,
    "product": product,
    "store": store,
    "distribution": distribution
}

# defining rows, duplicates, and missing values for all four files
for name, data in datasets.items():
    print(f"Dataset: {name}")
    print(f"Rows: {len(data)}")
    print(f"Duplicates: {data.duplicated().sum()}")
    print(f"Missing Values: {data.isnull().sum().sum()}")
```

## Product Catalog Cleaning

Standardize product names, SKUs, costs, categories, subcategories, suppliers, and package sizes.

### Preserve original product identifiers

Copy the product catalog and retain the original SKU.

```python
# Define products_clean by creating a copy of the raw product DataFrame
products_clean = product.copy()
products_clean["original_sku"] = products_clean["sku"]
```

### Standardize product names

Normalize text formatting and product-name variants.

```python

# Standardizing product formatting while replacing underscores with spaces, preserving word separations, and capitalizing
products_clean['product_name'] = (
    products_clean['product_name']
    .str.lower()
    .str.replace('(', '', regex=False)
    .str.replace(')', '', regex=False)
    .str.replace('_', ' ', regex=False)  # Replaces underscores with a single space
    .str.replace('pk', '-pack', regex=False)
    .str.replace('sz', 'size', regex=False)
    .str.replace(',', '', regex=False)
    .str.replace(r'\s+', ' ', regex=True)  # Condenses any multiple spaces into a single space
    .str.strip()
    .str.title()
    .str.replace('Aa', 'AA', regex=False)  # 2. Convert 'Aa' to 'AA' last so title() doesn't overwrite it
)

#Creating a product map - Logic: Put the Product Type first, then quantity (if applicable)
PRODUCT_MAP = {'Strawberry Pop-Tart': 'Pop-Tarts Strawberry',
            'Strawberry Pop-Tart': 'Pop-Tarts Strawberry',
            'Water - 24 Pack': 'Bottled Water 24-Pack',
            'Batteries AA 4-Pack': 'AA Batteries 4-Pack',
            'Board Game - Monopoly': 'Monopoly Board Game',
            'Streaming Device': 'Roku Streaming Stick',
            'Soup - Canned': 'Canned Soup',
            'Portable Phone Charger': 'Portable Charger',
            'Variety Pack - Chips': 'Chips Variety Pack',
            'Toilet Paper 12-Roll': 'Toilet Paper 12-Pack',
            'Baby Diapers - Size 3': 'Diapers Size 3',
            'Infant Formula': 'Baby Formula',
            'Tarp - Heavy Duty': 'Heavy Duty Tarp',
            'First-Aid Kit': 'First Aid Kit',
            'Flashlight Led': 'LED Flashlight',
            'Candles Emergency': 'Emergency Candles'
}
#applying the mapping
products_clean["product_name"] = (
    products_clean["product_name"].replace(PRODUCT_MAP)
)
print(products_clean["product_name"].unique())
```

### Map products to suppliers

Create and apply a product-to-supplier reference mapping.

```python
# 1. Create a clean dictionary mapping standardized product names to their correct suppliers
PRODUCT_TO_SUPPLIER = {
    'Pop-Tarts Strawberry': "Kellogg's",
    'Bottled Water 24-Pack': 'Nestle',
    'AA Batteries 4-Pack': 'Duracell',
    'Flashlight Led': 'Everactive',
    'Light Beer 24-Pack': 'Anheuser-Busch',
    'Monopoly Board Game': 'Hasbro',
    'Playing Cards': 'Bicycle',
    'Roku Streaming Stick': 'Roku Inc',
    'Canned Soup': "Campbell's",
    'White Bread': 'Sara Lee',
    'Peanut Butter': 'J.M. Smucker',
    'Emergency Candles': 'Sterno',
    'First Aid Kit': 'Johnson & Johnson',
    'Portable Charger': 'Anker',
    'Chips Variety Pack': 'Frito-Lay',
    'Paper Towels 6-Pack': 'Georgia-Pacific',
    'Toilet Paper 12-Pack': 'Kimberly-Clark',
    'Diapers Size 3': 'Procter & Gamble',
    'Baby Formula': 'Abbott',
    'Heavy Duty Tarp': 'Tarps Plus'
}

# 2. Map the correct supplier onto the products_clean DataFrame using product_name
products_clean['supplier'] = products_clean['product_name'].map(PRODUCT_TO_SUPPLIER).fillna(products_clean['supplier'])

# 3. Display the standardized mapping table
supplier_mapping_df = products_clean[['product_name', 'supplier']].drop_duplicates().sort_values('product_name')
display(supplier_mapping_df)
```

### Canonicalize product SKUs

Map inconsistent SKU variants to shared canonical identifiers.

```python
# Moving onto a SKU map - logic: give the most info possible in each SKU
SKU_MAP = {'PT-12': 'POPTART-STRAW',
           'SKU#459812': 'POPTART-STRAW',
           'PT_STRAWBERRY': 'POPTART-STRAW',
           'WTR-24': 'WATER24PK',
           'WTR_24PK': 'WATER24PK',
           'BATT-100': 'BATT_AA_4PK',
           'BAT-AA4': 'BATT_AA_4PK',
           'FLASH-01': 'LED-FLASH',
           'BEER-24': 'LTBEER24',
           'BG-MONO': 'GAME-MONO',
           'MONOPOLY-01': 'GAME-MONO',
           'PLAY-CARD': 'PLAYCARD',
           'CARDS-01': 'PLAYCARD',
           'PLAYCARD-STD': 'PLAYCARD',
           'ROKU-STD': 'ROKUSTREAM',
           'STREAM-01': 'ROKUSTREAM',
           'CNSOUP-01': 'SOUP-CAN',
           'BREAD-WH': 'WHITEBREAD',
           'PNUTBTR-16': 'PEANUTBTR',
           'PB-16': 'PEANUTBTR',
           'CNDL-EM': 'CANDLE-EMRG',
           'FAID-01': 'FIRSTAID-KIT',
           'CHRG-PORT': 'PORTCHARGER',
           'CHIPS-VP': 'CHIPVARIETY',
           'PTOWEL-6': 'PAPERTOWEL',
           'PAPERTOWEL6': 'PAPERTOWEL',
           'TP-12': 'TOILETPAPER12',
           'DIAP-S3': 'DIAPERS-S3',
           'DIAPERS-3': 'DIAPERS-S3',
           'BFORM-01': 'BABYFORMULA',
           'HDTARP-01': 'TARP-HD',
}

products_clean["canonical_sku"] = (
    products_clean["sku"].replace(SKU_MAP)
)
```

### Normalize costs and deduplicate

Convert costs to numeric values, calculate mean cost per canonical SKU, and remove duplicate products.

```python
# first making sure unit_cost is numeric
products_clean['unit_cost'] = pd.to_numeric(products_clean['unit_cost'], errors='coerce')

# calculating the mean cost for each canonical product type
products_clean['unit_cost'] = products_clean.groupby('canonical_sku')['unit_cost'].transform('mean')

# Rounding the unit_cost to 2 decimal places
products_clean['unit_cost'] = products_clean['unit_cost'].round(2)

#Droping duplicate rows based on canonical_sku to keep only one unique entry per product type
products_clean = products_clean.drop_duplicates(subset=['canonical_sku']).reset_index(drop=True)

final_clean_catalog = products_clean[['canonical_sku', 'product_name', 'category', 'subcategory', 'supplier', 'package_size', 'unit_cost']]
display(final_clean_catalog)
```

### Standardize categories

Normalize capitalization and consolidate category labels.

```python
# organizing the cateory column - Logic: stripping unnecessary descriptions + standardizing capitalization conventions
products_clean['category'] = (
    products_clean['category']
    .str.lower()
    .str.title()
)
print(products_clean)
# creating a category map
CATEGORY_MAP = {'Breakfast Foods': 'Breakfast',
           'Emergency Supplies': 'Emergency',
           'Alcoholic Beverages': 'Alcohol',
           'Grocery': 'Groceries',
           'Baby': 'Baby Products',
}
products_clean['category'] = products_clean['category'].replace(CATEGORY_MAP)
print(products_clean)
```

### Fill subcategories within SKUs

Forward- and backward-fill subcategory values within each canonical SKU group.

```python
products_clean.groupby("canonical_sku")["subcategory"].nunique()
products_clean["subcategory"] = (
    products_clean.groupby("canonical_sku")["subcategory"]
    .transform(lambda x: x.ffill().bfill())
)
print(products_clean)
```

### Review product cleanup

Display a sample of product identifiers, names, and categories.

```python
#progress check

products_clean[
    ["original_sku", "canonical_sku", "product_name", "category", "subcategory"]
].head(25)
```

### Normalize supplier names

Standardize supplier capitalization and whitespace.

```python
# preserving missing values for costs at this stage
#moving on to suppliers, standardizing names

products_clean ["supplier"] = (
    products_clean["supplier"]
    .str.lower()
    .str.title()
    .str.replace(r"\s+", " ", regex=True)
)
print(products_clean)
```

### Normalize package sizes

Make package-size descriptions more consistent.

```python
#standardizing how package sizes are written
products_clean["package_size"] = (
    products_clean["package_size"]
    .str.lower()
    .str.title()
    .str.replace(r"\s+", " ", regex=True)
    .str.replace('Pk','Pack', regex=False)
    .str.replace('Single','1 Pack', regex=False)
    .str.replace('-', ' ', regex=False)
    # Insert a space between any number and 'Pack' (e.g., '24Pack' -> '24 Pack')
    .str.replace(r'(\d+)(Pack)', r'\1 \2', regex=True)
    .str.strip()
)
print(products_clean)
```

### Inspect cleaned product fields

Preview selected cleaned catalog columns.

```python
#Progress so far

print(products_clean[["original_sku", "canonical_sku", "product_name", "category", "subcategory", "supplier", "package_size"]].head(25))
```

## Store Data Cleaning

Normalize store identifiers and state abbreviations.

### Copy store data

Create a working store DataFrame.

```python
# Define stores_clean by creating a copy of the raw product DataFrame
stores_clean = store.copy()
```

### Format store IDs

Normalize store IDs to a hyphenated state-prefix format.

```python
#creating a function to standardize state code to be "FL-123"
def format_store_id(store_id):
    # Replace underscores
    store_id = store_id.replace('_', '-')

    # If the hyphen is already there, keep as is
    if '-' in store_id:
        return store_id

    # insert the hyphen after the first 2 state characters, if not present
    return f"{store_id[:2]}-{store_id[2:]}"

#Applying the function to the column
stores_clean = store.copy()
stores_clean['store_id'] = stores_clean['store_id'].apply(format_store_id)
print(stores_clean['store_id'].unique())
```

### Normalize state abbreviations

Convert Florida state-name variants to FL and inspect the result.

```python
# moving onto state name
def state_abbreviation(state):
    # Replace "Fla." with "FL"
    # Replacing "Florida" with "FL"
    state = state.replace('Fla.', 'FL')
    state = state.replace('Florida', 'FL')

    # If the hyphen is already there, keep as is
    if 'FL' in state:
        return state

#Applying the function to the column
stores_clean = store.copy()
stores_clean['state'] = stores_clean['state'].apply(state_abbreviation)
print(stores_clean['state'].unique())

#checking to make changes were implemented
print(stores_clean)
```

## Distribution Center Cleaning

Convert shipment, distance, and surge-capacity fields into consistent numeric representations.

### Copy distribution-center data

Create a working distribution-center DataFrame.

```python
# Define distribution_clean by creating a copy of the raw product DataFrame
distribution_clean = distribution.copy()
```

### Clean distribution capacity fields

Convert numeric fields and standardize surge-capacity values.

```python
#creating a function to standardize surge_capacity to be just a number
distribution_clean = distribution.copy()

#converting shipments and distance to orlando into numeric values first
distribution_clean['avg_daily_shipments'] = pd.to_numeric(distribution_clean['avg_daily_shipments'], errors='coerce')
distribution_clean['distance_to_orlando_mi'] = pd.to_numeric(distribution_clean['distance_to_orlando_mi'], errors='coerce')

def format_surge_capacity(surge_capacity):
    if not isinstance(surge_capacity, str):
        return surge_capacity
    # Omit "~"
    surge_capacity = surge_capacity.replace('~', ' ')
    return surge_capacity

#Applying the function to clean the strings first
distribution_clean['surge_capacity'] = distribution_clean['surge_capacity'].apply(format_surge_capacity)

# Now convert surge_capacity to a numeric value
distribution_clean['surge_capacity'] = pd.to_numeric(distribution_clean['surge_capacity'], errors='coerce')

print(distribution_clean)
```

## Transaction Data Cleaning

Remove duplicates and standardize identifiers, pricing, timestamps, SKUs, names, categories, and quantities.

### Remove duplicate transactions

Copy transactions and drop fully duplicated records.

```python
# Define transactions_clean by creating a copy of the raw product DataFrame and then identifying duplicates and dropping them
transactions_clean = transactions.copy()

print("Before:", len(transactions_clean))

transactions_clean = transactions_clean.drop_duplicates()

print("After:", len(transactions_clean))
```

### Inspect transaction IDs

Apply the notebook’s transaction-ID formatting function and inspect unique IDs.

```python
# creating a function to ensure transaction_id is consistent
def format_transaction_id(transaction_id):
    if not isinstance(transaction_id, str):
        return transaction_id
    # If 'TXN' is already there, keep as is
    if 'TXN' in transaction_id:
        return transaction_id
    return transaction_id

# Applying the function directly to the existing transactions_clean
transactions_clean['transaction_id'] = transactions_clean['transaction_id'].apply(format_transaction_id)
print(transactions_clean['transaction_id'].unique())
```

### Normalize transaction store IDs

Standardize store IDs to match the store dataset.

```python
# function to ensure store code matches code established in store data 'FL-123'
def format_state_code(store_id):
    if not isinstance(store_id, str):
        return store_id

    # Remove leading/trailing whitespaces and convert to uppercase
    store_id = store_id.strip().upper()

    # Replace underscores
    store_id = store_id.replace('_', '-')

    # If the hyphen is already there, keep as is
    if '-' in store_id:
        return store_id

    # Insert the hyphen after the first 2 state characters if not present
    return f"{store_id[:2]}-{store_id[2:]}"

# Applying the function to the existing transactions_clean
transactions_clean['store_id'] = transactions_clean['store_id'].apply(format_state_code)
print(transactions_clean['store_id'].unique())
```

### Convert unit prices

Remove currency symbols and coerce prices to numeric values.

```python
# creating a function to standardize the unit_price
def format_unit_price(unit_price):
    if not isinstance(unit_price, str):
        return unit_price
    # remove $
    unit_price = unit_price.replace('$', '')
    # converting prices to numeric values
    unit_price = pd.to_numeric(unit_price, errors='coerce')
    return unit_price

# Applying the function to the existing transactions_clean
transactions_clean['unit_price'] = transactions_clean['unit_price'].apply(format_unit_price)
print(transactions_clean['unit_price'].unique())
```

### Translate transaction timestamps

Convert mixed timestamp formats to Pandas datetimes.

```python
# Handling the timestamps - creating a standardized date and timestamp format and keeping existing rows that don't have timestamps
# Using pandas standardized datetime
def format_timestamps(df):
    df = df.copy()

    df["timestamp"] = pd.to_datetime(
        df["timestamp"],
        format="mixed",
        errors="coerce"
    )

    return df

transactions_clean = format_timestamps(transactions_clean)
print(transactions_clean)
```

### Map transaction SKUs

Retain original SKUs and apply the canonical SKU mapping.

```python
# applying SKU logic already established to the transactions data
# keeping existing transactions SKU for now
transactions_clean["original_sku"] = (

    transactions_clean["sku"]
)

transactions_clean["canonical_sku"] = (
    transactions_clean["sku"].replace(SKU_MAP)
)
print(transactions_clean)
```

### Normalize transaction product names

Apply product-name formatting consistent with the catalog.

```python
# doing the same process for product names - ensuring they match what was already established in the product
# cleaning the text using same conventions for products_clean
transactions_clean['product_name'] = (
    transactions_clean['product_name']
    .str.lower()
    .str.replace('(', '', regex=False)
    .str.replace(')', '', regex=False)
    .str.replace('_', ' ', regex=False)
    .str.replace('pk', '-pack', regex=False)
    .str.replace('sz', 'size', regex=False)
    .str.replace(',', '', regex=False)
    .str.replace(r'\s+', ' ', regex=True)
    .str.strip()
    .str.title()
    .str.replace('Aa', 'AA', regex=False)
)
#verifying that cleaning produced the desired result
print(transactions_clean['product_name'].unique())
# mapping cleaned values to PRODUCT MAP logic already established
PRODUCT_MAP = {
    'Strawberry Pop-Tart': 'Pop-Tarts Strawberry',
    'Water - 24 Pack': 'Bottled Water 24-Pack',
    'Batteries AA 4-Pack': 'AA Batteries 4-Pack',
    'Board Game - Monopoly': 'Monopoly Board Game',
    'Streaming Device': 'Roku Streaming Stick',
    'Soup - Canned': 'Canned Soup',
    'Portable Phone Charger': 'Portable Charger',
    'Variety Pack - Chips': 'Chips Variety Pack',
    'Toilet Paper 12-Roll': 'Toilet Paper 12-Pack',
    'Baby Diapers - Size 3': 'Diapers Size 3',
    'Infant Formula': 'Baby Formula',
    'Tarp - Heavy Duty': 'Heavy Duty Tarp',
    'Flashlight Led': 'LED Flashlight',
    'Candles Emergency': 'Emergency Candles',
}

# Applying the dictionary replace on products_clean
transactions_clean['product_name'] = transactions_clean['product_name'].replace(PRODUCT_MAP)

# Printing values from products_clean to verify the result
print(transactions_clean['product_name'].unique())

# finally, checking to make sure transactions clean is looking accurate
print(transactions_clean)
```

### Normalize transaction categories

Apply the same category labels used in the catalog.

```python
# applying a similar logic to make sure categories match up with those established in the product catalog
#Logic: stripping unnecessary descriptions + standardizing capitalization conventions
transactions_clean['category'] = (
    transactions_clean['category']
    .str.lower()
    .str.title()
)
# creating a category map
CATEGORY_MAP = {'Breakfast Foods': 'Breakfast',
           'Emergency Supplies': 'Emergency',
           'Alcoholic Beverages': 'Alcohol',
           'Grocery': 'Groceries',
           'Baby': 'Baby Products',
}
transactions_clean['category'] = transactions_clean['category'].replace(CATEGORY_MAP)
print(transactions_clean)
```

### Inspect transaction quantities

Define the quantity-conversion function and display the transactions.

```python
# last step: converting quantity into a numeric value
def format_quantity(quantity):
    if not isinstance(quantity, str):
        return quantity
    # converting quantity to numeric values
    quantity = pd.to_numeric(quantity, errors='coerce')
    return quantity
print(transactions_clean)
```

## Inventory and Sales Database

Download the SQLite database, inspect its tables, and perform the cleaning steps included in the notebook.

### Load and process SQLite data

Download the inventory-and-sales database and run the notebook’s SQLite processing code.

```python
import urllib.request
import os
import sqlite3
import pandas as pd

# Downloading the db from GitHub
db_url = "https://raw.githubusercontent.com/junseo25/ds2002-walmart-hurricane-project/main/data/inventory_and_sales.db"
db_filepath = "inventory_and_sales.db"

try:
    urllib.request.urlretrieve(db_url, db_filepath)
    print("Database downloaded successfully!")
except Exception as e:
    print(f"Error downloading database: {e}")

# connecting the db and read the tables
conn = sqlite3.connect(db_filepath)
daily_sales = pd.read_sql_query("SELECT * FROM daily_sales_summary", conn)
inventory_levels = pd.read_sql_query("SELECT * FROM inventory_levels", conn)
conn.close()

#printing to get a sense for the data
print("inventory_levels:")
print(inventory_levels.head())
print("\ndaily_sales:")
print(daily_sales.head())

# applying the store_id, date, and cateogory standardizations to sales
daily_sales_clean = daily_sales.copy()
daily_sales_clean['store_id'] = daily_sales_clean['store_id'].apply(format_state_code)
daily_sales_clean['date'] = pd.to_datetime(daily_sales_clean['date'], format='mixed', errors='coerce').dt.strftime('%Y-%m-%d')
daily_sales_clean['category'] = daily_sales_clean['category'].str.lower().str.title().replace(CATEGORY_MAP)

# applying the store_id, date, and canonical sku standardizations to inventory (not getting rid of original sku)
inventory_levels_clean = inventory_levels.copy()
inventory_levels_clean['store_id'] = inventory_levels_clean['store_id'].apply(format_state_code)
inventory_levels_clean['date'] = pd.to_datetime(inventory_levels_clean['date'], format='mixed', errors='coerce').dt.strftime('%Y-%m-%d')
inventory_levels_clean['canonical_sku'] = inventory_levels_clean['sku'].replace(SKU_MAP)

# displaying the cleaned dataframes
display(daily_sales_clean.head())
display(inventory_levels_clean.head())
```

