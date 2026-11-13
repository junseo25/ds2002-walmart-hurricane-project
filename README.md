# Walmart Hurricane Analytics

A DS2002 team project exploring how Hurricane Helene affected Walmart product demand and supply. The notebook combines transaction and product data, store and distribution-center locations, SQLite sales and inventory tables, and historical weather. It cleans and checks the data, answers five business questions, creates presentation charts, and ends with a recommendation and reflection.

## Repository contents

- [`notebooks/notebook.ipynb`](notebooks/notebook.ipynb): the main working notebook. It includes data loading and cleaning, validation checks, weather retrieval and joins, Q1–Q5, charts, the recommendation, and reflections.
- [`notebooks/scratch/`](notebooks/scratch/): supporting notebook work and experiments.
- [`data/`](data/): the project’s source files:
  - `walmart_transactions.csv`: transaction records
  - `product_catalog.csv`: product and SKU information
  - `store_locations.csv`: store locations
  - `distribution_centers.csv`: distribution-center locations
  - `inventory_and_sales.db`: SQLite database with sales and inventory tables
- [`TEAM_CHARTER.md`](TEAM_CHARTER.md): team goals, roles, responsibilities, and working agreements.

## Run the analysis

### Google Colab (recommended)

1. Open [the main notebook in Google Colab](https://colab.research.google.com/github/junseo25/ds2002-walmart-hurricane-project/blob/main/notebooks/notebook.ipynb).
2. Choose **Runtime → Run all**. If you have run some cells already, choose **Runtime → Restart session and run all** so the notebook executes in order.
3. Wait for the data and weather cells to finish. The notebook reads the project files from this repository and requests historical weather data from Open-Meteo, so the runtime needs internet access.

The notebook uses the Open-Meteo Archive API for historical weather and does not require an API key. Its analysis dates are configured in the notebook.

### Run locally

1. Clone the repository and enter its folder:

   ```bash
   git clone https://github.com/junseo25/ds2002-walmart-hurricane-project.git
   cd ds2002-walmart-hurricane-project
   ```

2. Install Python and the notebook’s packages:

   ```bash
   python -m pip install jupyter pandas numpy matplotlib requests
   ```

3. Start Jupyter and open `notebooks/notebook.ipynb`:

   ```bash
   jupyter lab
   ```

Run the cells from top to bottom. Internet access is required because the notebook loads the data from the repository’s raw GitHub files and calls Open-Meteo for weather.

## What the notebook investigates

1. **Category demand surge** — compares sales across the analysis periods.
2. **Pop-Tarts consolidation** — standardizes related product identifiers for analysis.
3. **Weather relationships and lag** — joins weather observations with sales and examines timing.
4. **Store geography** — compares patterns across store locations.
5. **Board-game investigation** — weighs the observed sales signal against data quality and inventory risks before making a recommendation.

The notebook documents the cleaning decisions and validation checks alongside the analysis so readers can see where the data may limit conclusions.
