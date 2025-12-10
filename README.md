# Bayesian Inventory Forecasting Engine

## Project Context

Managing inventory for perishable restaurant ingredients is a balancing act. Ordering too much leads to **food waste ($C_o$)**, while ordering too little results in **lost sales ($C_u$)**.

This repository contains the statistical engine I built for the **Munchbox Inventory Recommendation System**. Instead of relying on single data point, this project uses a **Bayesian Structural Time Series (BSTS)** model to quantify demand uncertainty. It then applies the **Newsvendor Algorithm** to dynamically recommend an "Aggressive" or "Conservative" ordering strategy based on the specific profit margin of each ingredient specified by users.

## Repository Status: Reference Implementation

* **Note on Data Privacy:** This repository demonstrates the logic used in my final project. For confidentiality reasons, the actual sales logs from the pilot restaurant have been excluded and has been replaced by synthetic data, detailed below.
* **Synthetic Data:** I have included **synthetic dummy data** in the `/data` folder (randomly generated Poisson distributions) so you can run the code and see the forecasting visualization in action. If you want to create new synthetic data, feel free to do Step 2 in *How to Run Section*

## Key Capabilities

1.  **Probabilistic Forecasting:** Uses `PyMC` to generate a 90% High Density Interval (HDI) for future demand.
2.  **Vectorized Optimization:** Uses `NumPy` to simulate thousands of financial scenarios instantly to find the optimal order quantity ($Q^*$).
3.  **Visualization:** Uses `Matplotlib` to render "Fan Charts" that visually communicate risk to the user.

## How to Run the Demo

1.  **Install Dependencies:**
    (Recommended: Use Python 3.11 or 3.12 due to PyTensor compatibility)

    ```bash
    pip install -r requirements.txt
    ```

2. **Create New Dummy File (Optional):**

   The dataset used in this demo is procedurally generated. You can recreate different demand scenarios by running.

   ``` bash
   python scripts/generate_data.py
   ```

3.  **Run the Forecaster:**

    ```bash
    python src/forecasting_model.py
    ```

### 4. Interactive Inputs (Human-in-the-Loop)

   The system currently utilizes a **Human-in-the-Loop** design for economic parameters. Since specific unit-loss data (e.g., exact spoilage costs per gram) is not yet available in the restaurant's database, the script prompts the user to input these values manually during runtime.

   **Future Roadmap For Prediction Model:** Future versions will aim to automate the Cost of Waste and Cost of Lost Sales.

   **You will be prompted to enter:**

      1.  **Ingredient Name:** (e.g., `shrimp` or `chicken`) - *Must exist in the dummy BOM.*
      2.  **Cost of Waste ($C_o$):** The loss incurred if one unit spoils (e.g., `0.5` per gram).
         * *Tip: Enter a low number if the item is cheap.*
      3.  **Cost of Lost Sales ($C_u$):** The profit margin lost if one unit is out of stock (e.g., `2.0` per gram).
         * *Tip: Enter a high number if the item is crucial for a popular dish.*
      4.  **Forecast Period:** The number of days to predict (e.g., `2` or `7`).

5. **View Results:**

   The system will generate a "Fan Chart" visualization pop-up and print the final Recommended Purchase strategy to the console.
