### **Project Overview**

This project develops high-speed Machine Learning surrogate models to emulate a 30-stage Benzene and p-Xylene distillation column designed in DWSIM. By replacing iterative thermodynamic solvers with Artificial Neural Networks (ANN) and Gradient Boosting, computational time is reduced from seconds to milliseconds while maintaining simulator-level precision.



#### **1. Prerequisites**

Before beginning, ensure you have the following installed:



* DWSIM: Open-source chemical process simulator (Version 8.0+ recommended).
* Python 3.x: Recommended via Google Colab or local Jupyter environment.
* Python Libraries:
* pandas, numpy: Data manipulation.
* scikit-learn: Preprocessing and regression (SVR, Random Forest).
* xgboost: High-performance gradient boosting.
* tensorflow / keras: Building the ANN architecture.



#### **2. Opening and Running the DWSIM FileLoad File:**



1. **Load File**: Open the file named DWSIM\_Flowsheet\_File.dwxmz within the DWSIM interface.
2. **Review Topology**: The flowsheet connects a single feed to two product streams (Distillate/Bottoms) and two energy streams ($Q\_C$ and $Q\_R$).
3. **Verify Configuration**:

&#x09;Unit: ChemSep Rigorous Distillation.

&#x09;Stages: 30 stages with the Feed introduced at Stage 15.

&#x09;Property Package: Soave-Redlich-Kwong (SRK).

4\. **Sensitivity Study:** Navigate to the Sensitivity Study tool to generate fresh data, sweep the independent variables (Reflux Ratio, Feed Temperature, Feed Composition).



#### **3. Running the Machine Learning Pipeline**



The Python code is structured as a development pipeline:

&#x09;1. Load Data: Import the exported CSV files from DWSIM (2,700 samples).

&#x09;2. Apply Physics Filter: Run the preprocessing script to remove non-converged points and ensure x\_D > x\_B.

&#x09;3. Feature Engineering: Generate the Reflux-to-Bottoms Ratio (RF\\Ratio) to help the AI learn internal column traffic.

&#x09;4. Data Scaling: Execute StandardScaler to normalize features (Reflux, Temp, Energy) to a mean of 0 and unit variance.



#### **4. Training the Models**



The project benchmarks four specific architectures:

&#x09;XGBoost: Trained for high-purity plateau accuracy.

&#x09;Random Forest: Used as a stable baseline for outlier handling.

&#x09;SVR: Trained with an RBF kernel to map the thermodynamic surface.

&#x09;ANN: A custom architecture consisting of 128 to 64 to 32 neurons. 

&#x09;	Optimizer: Adam. 

&#x09;	Loss Function: Mean Squared Error (MSE).

&#x09;	Epochs: 120.



#### **5. Reproducing Results**



To replicate the findings in the fellowship report:



1. **Validation:** Use the 20% held-back test set for evaluation.
2. **Metrics:** Verify performance using R^2, MAE, and RMSE.
3. **Expected Result:** All models should achieve R^2 > 0.85 and  XGBoost should yield high accuracy (R^2 approx 0.985) while the ANN provides the smoothest physical mapping (R^2 approx 0.923).
4. **Auditing:** Run the Residual Plots and Inter-Target Correlation Audit to confirm the model respects the Mass-Energy balance.

