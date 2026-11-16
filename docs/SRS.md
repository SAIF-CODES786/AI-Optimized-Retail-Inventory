# Software Requirements Specification (SRS)

> Text version extracted from the supplied SRS PDF. See the project documentation source for diagrams and page layout.

<PARSED TEXT FOR PAGE: 1 / 10>
                             SOFTWARE REQUIREMENTS
                               SPECIFICATION (SRS)
                       AI-Optimized Retail Inventory Management System



                                                     Academic Project Documentation

                                                 Prepared for: ABES Engineering College

                                            Department: Computer Science and Engineering

                                      Reviewed implementation: Python / Google Colab notebook

                                                             Document Version: 1.0

                                                             Date: 16 November 2026




    Prepared from the supplied Untitled7.ipynb notebook and dataset.csv. Implemented functionality is separated from proposed future
                                                                scope.




AI-Optimized Retail Inventory Management System | SRS v1.0                                                                        Page 1
<PARSED TEXT FOR PAGE: 2 / 10>
 Important Notice
 This is an academic notebook-based analytical prototype. Supplied files demonstrate CSV analysis, low-stock flagging, and daily-sales
 regression. They do not demonstrate a deployed dashboard, live database, supplier integration, automated purchase orders, XAI, or
 production inventory optimisation.


 Abstract
 The prototype uses a 500-row, 10-column CSV dataset. The notebook performs data inspection, missing-value checks, category sales
 analysis, low-stock identification, feature encoding, an 80:20 split, and trains Linear Regression, Random Forest, and XGBoost
 regressors. Random Forest has the lowest saved test MAE (14.032) and RMSE (16.299), but test R² is 0.016, indicating weak
 explanatory performance.


 Table of Contents
     1. Introduction
 •
     2. General Description
 •
     3. Functional Requirements
 •
     4. Interface Requirements
 •
     5. Performance Requirements
 •
     6. Design Constraints
 •
     7. Non-Functional Attributes
 •
     8. Uses of the SRS Document
 •
     9. FAQs on SRS Format
 •
     10. Conclusion
 •
     Appendix A: Dataset and Model Results
 •
     Appendix B: Screenshots and Figures
 •
     Appendix C: Items to Confirm
 •




AI-Optimized Retail Inventory Management System | SRS v1.0                                                                         Page 2
<PARSED TEXT FOR PAGE: 3 / 10>
 1. Introduction
 1.1 Purpose
 This SRS defines the current scope, behaviour, interfaces, constraints, assumptions, and quality expectations of the supplied notebook.

 1.2 Scope
       Load and inspect the CSV dataset.
 •
       Summarise dimensions, types, statistics, and missing values.
 •
       Analyse category average daily sales.
 •
       Identify low-stock rows and derive reorder_required.
 •
       Encode product_id and category.
 •
       Split data 80:20 and train three regressors.
 •
       Plot predictions and compare MAE, RMSE, and R².
 •
 Out of scope: dashboard, authentication, database, POS integration, automated supplier orders, cost optimisation, XAI, and deployment.

 1.3 Definitions
     Term                              Meaning

     SRS                               Software Requirements Specification

     SKU                               Product identifier

     MAE                               Mean Absolute Error

     RMSE                              Root Mean Squared Error

     R²                                Coefficient of determination

     Reorder level                     Stock threshold for replenishment review


 1.4 References
       Supplied Untitled7.ipynb and dataset.csv.
 •
       Python, Pandas, NumPy, Matplotlib, Seaborn, scikit-learn, and XGBoost documentation.
 •
       ISO/IEC/IEEE 29148 requirements engineering guidance.
 •
 1.5 Verification status
 Implemented = supported by code/output. Observed = saved output or value calculated from the CSV. Proposed = future
 recommendation. The notebook was inspected but not rerun end-to-end because some variable definitions are absent from the saved
 source.


 2. General Description
 2.1 Product Perspective
 A standalone Python notebook shown in Google Colab. It reads a CSV into memory, analyses data, fits regressors, and displays
 outputs; it is not a client-server application.

 2.2 Users
     User                                 Interaction

     Student / developer                  Runs notebook, supplies CSV, inspects results.

     Faculty evaluator                    Reviews scope and evidence.

     Retail analyst (future)              Potential dashboard user; dashboard not supplied.


 2.3 Dataset Profile
     Property                                                    Observed value

     Rows / original columns                                     500 / 10

     Date range                                                  2024-01-01 to 2024-01-25

     Unique products                                             311

     Unique suppliers                                            30




AI-Optimized Retail Inventory Management System | SRS v1.0                                                                          Page 3
<PARSED TEXT FOR PAGE: 4 / 10>
     Property                                                          Observed value

     Missing values                                                    0 in all original columns

     Season                                                            Winter only

     Rows below reorder level                                          63

     Rows at or below reorder level                                    64


 2.4 Limitations
      Dataset spans only 25 days; all records are Winter.
 •
      311 products occur in 500 records, with sparse product histories.
 •
      Best test R² is 0.016; two models have negative test R².
 •
      Saved source omits the features definition and visible creation of date fields shown in outputs.
 •
      No dashboard, database, supplier workflow, XAI, or automatic optimisation is demonstrated.
 •
 2.5 System Architecture
 CSV input → data inspection/preparation → feature encoding and split → model training → metrics and plots.




                                                         Figure 1: Current notebook workflow architecture

 2.6 Use Cases




                                                               Figure 2: Current notebook use cases

     Use case                         Description

     UC-01                            Load dataset

     UC-02                            Inspect schema, statistics, and missing values

     UC-03                            Identify low-stock rows and reorder flag




AI-Optimized Retail Inventory Management System | SRS v1.0                                                    Page 4
<PARSED TEXT FOR PAGE: 5 / 10>
    Use case                          Description

    UC-04                             Visualise category sales

    UC-05                             Train three regression models

    UC-06                             Evaluate metrics and plot predictions


 2.7 Data Flow




                                                                   Figure 3: High-level data flow


 3. Functional Requirements
    ID           Requirement                                                                        Status

    FR-01        Load CSV into DataFrame                                                            Implemented

    FR-02        Preview records                                                                    Implemented

    FR-03        Report shape, types, statistics                                                    Implemented

    FR-04        Count missing values                                                               Implemented; none observed

    FR-05        Count unique products                                                              Implemented; 311

    FR-06        Plot category average sales                                                        Implemented

    FR-07        Filter stock below reorder level                                                   Implemented; 63 rows

    FR-08        Derive reorder_required for stock ≤ threshold                                      Implemented

    FR-09        One-hot encode product_id and category                                             Output exists; feature
                                                                                                    definition absent

    FR-10        80:20 split with seed 42                                                           Implemented; 400/100

    FR-11        Train three regression models                                                      Implemented

    FR-12        Plot actual vs predicted                                                           Implemented

    FR-13        Calculate MAE, RMSE, R²                                                            Implemented

    FR-14        Generate supplier orders                                                           Proposed

    FR-15        Web dashboard and login                                                            Proposed


 3.1 Input Fields
    Field                                   Description

    date                                    Record date

    product_id                              Product identifier

    category                                Product category

    season                                  Season label

    stock_qty                               Available stock

    reorder_level                           Reorder threshold




AI-Optimized Retail Inventory Management System | SRS v1.0                                                                   Page 5
<PARSED TEXT FOR PAGE: 6 / 10>
     Field                             Description

     daily_sales                       Daily sales target

     unit_cost                         Unit cost

     supplier_id                       Supplier identifier

     lead_days                         Supplier lead time


 3.2 Model Configuration
 80:20 train-test split, random_state=42. Linear Regression uses defaults. Random Forest uses 100 estimators. XGBoost uses 100
 estimators, learning_rate=0.1, max_depth=3, random_state=42, and squared-error objective.

 3.3 Future Requirements
       Restore missing feature definitions and rerun cleanly.
 •
       Collect longer multi-season product history.
 •
       Use chronological validation.
 •
       Implement reorder quantity using forecast demand, lead time, and safety stock.
 •
       Add dashboard, storage, supplier workflow, and explainability after validation.
 •
 4. Interface Requirements
 4.1 User Interface
 Notebook code cells, printed DataFrame summaries, model evaluation table, and Matplotlib/Seaborn plots. No separate web interface is
 included.

 4.2 Software Interfaces
     Interface                         Specification

     CSV                               Pandas read_csv() uses /content/dataset.csv

     DataFrame                         In-memory Pandas DataFrame

     ML                                scikit-learn and XGBoost

     Charts                            Matplotlib and Seaborn


 4.3 Hardware and Communication
 A standard computer and browser-based notebook runtime are sufficient. No project API or supplier communication interface is shown.


 5. Performance Requirements
 No latency, throughput, concurrency, or memory targets are defined. These are saved model-evaluation outputs, not production
 guarantees.
      Model                        Train MAE                Test MAE      Train RMSE     Test RMSE    Train R²      Test R²

      Linear Regression            5.578                    17.452        9.059          21.439       0.680         -0.702

      Random Forest                5.417                    14.032        6.482          16.299       0.836         0.016

      XGBoost                      11.108                   14.381        13.057         16.679       0.335         -0.030


 Random Forest has the lowest test MAE and RMSE, but test R²=0.016 indicates weak explanatory power. Linear Regression and
 XGBoost have negative test R².


 6. Design Constraints
       CSV schema and column names must match.
 •
       Processing is in-memory; no persistent application database is shown.
 •
       One-hot encoding produces 322 features in saved output.
 •
       The feature-list definition and some date-field creation code are missing from saved source.
 •
       Only one season and 25 days of data are represented.
 •
 7. Non-Functional Attributes


AI-Optimized Retail Inventory Management System | SRS v1.0                                                                       Page 6
<PARSED TEXT FOR PAGE: 7 / 10>
     Attribute                      Current status / requirement

     Reliability                    Restore missing source definitions for clean execution.

     Usability                      Label notebook cells and outputs sequentially.

     Maintainability                Separate preprocessing, training, evaluation.

     Portability                    Make CSV path configurable.

     Security                       No authentication is implemented.

     Scalability                    No large-scale or multi-user testing.

     Explainability                 No XAI module.

     Availability                   No hosted service or uptime target.


 7.1 Test Evidence
 Saved outputs show data inspection, missing-value checks, low-stock filtering, model training, plots, and evaluation metrics. No separate
 automated test suite or coverage report was supplied.


 8. Uses of the SRS Document
       Developers use requirements to plan implementation.
 •
       Testers derive repeatable checks from requirements.
 •
       Faculty compare scope with evidence.
 •
       Maintainers use limitations to reproduce the notebook.
 •
 9. FAQs
 What is an SRS? A structured description of scope, behaviour, interfaces, constraints, and quality requirements.

 Is this a complete retail platform? No; it is the supplied notebook-based analytical prototype.

 Does it place orders? No; it flags stock levels but does not create purchase orders.

 Is the model highly accurate? Not established; the best test R² is 0.016.


 10. Conclusion
 The notebook establishes a preliminary retail-data analysis and daily-sales regression workflow. Random Forest has the best test MAE
 and RMSE of the three tested models, but its test R² is close to zero. Treat it as an academic baseline until missing source definitions
 are restored, longer multi-season data is collected, time-based validation is performed, and inventory recommendation features are
 implemented.


 Appendix A: Dataset and Model Results
     Field                                Mean                                  Min                              Max

     stock_qty                            257.35                                0                                500

     reorder_level                        60.73                                 20                               100

     daily_sales                          32.05                                 5                                60

     unit_cost                            2436.57                               104.22                           4991.74

     lead_days                            12.31                                 3                                21


     Category                                        Records                                  Mean daily sales

     Apparel                                         130                                      29.39

     Electronics                                     128                                      32.1

     Grocery                                         112                                      35.16

     HomeDecor                                       130                                      31.96



 Appendix B: Authentic Notebook Figures
 Figures B1–B5 are plot images embedded in the supplied notebook outputs.




AI-Optimized Retail Inventory Management System | SRS v1.0                                                                           Page 7
<PARSED TEXT FOR PAGE: 8 / 10>
                                                    Figure B1: Category-wise average daily sales




                                                             Figure B2: Correlation heatmap




AI-Optimized Retail Inventory Management System | SRS v1.0                                         Page 8
<PARSED TEXT FOR PAGE: 9 / 10>
                                                 Figure B3: Actual vs predicted — Linear Regression




                                                  Figure B4: Actual vs predicted — Random Forest




                                                     Figure B5: Actual vs predicted — XGBoost


 Appendix C: Items to Confirm

AI-Optimized Retail Inventory Management System | SRS v1.0                                            Page 9
<PARSED TEXT FOR PAGE: 10 / 10>
    Item                               Observation / action

    Notebook reproducibility           Restore missing feature definitions and run from a clean runtime.

    CSV path                           Notebook expects /content/dataset.csv; add upload instructions.

    Model validation                   Use chronological holdout and more historical data.

    Inventory optimisation             Implement reorder quantity, safety stock, and supplier workflow.

    Application interface              Confirm notebook-only or full application submission.

    Cover details                      Add student name, roll number, guide name, course, and section.




AI-Optimized Retail Inventory Management System | SRS v1.0                                                 Page 10
