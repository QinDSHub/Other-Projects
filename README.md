## Notes on Project Materials

The PPTs included in these two projects were originally created for presentations at the time and have been preserved since then.  
While they may now appear somewhat preliminary, they capture a valuable process focused on underlying technical design and system thinking.

They provide a full-picture view of the end-to-end modeling workflow, including:
- Problem definition  
- Data exploration  
- Data cleaning  
- Model architecture design  
- Model development  
- Model offline evaluation  

These materials may be particularly helpful for anyone seeking a deeper understanding of the complete modeling pipeline and the technical reasoning behind it. You can also gain a solid overview by reviewing the README sections below for each project.

For a complete overview of my projects, feel free to visit my LinkedIn:  
### https://www.linkedin.com/in/qinluu  

I’m always happy to connect, share, and discuss.

---

## Demand Forecasting for Rémy Products on JD Marketplace

📂 Project Overview

This project was initiated as a high-stakes feasibility study for a VIP client in the luxury spirits sector (Rémy Cointreau). The objective was to infer monthly sales volumes on the JD.com platform using unstructured consumer comment metadata as a proxy.
Despite significant data constraints and high noise levels, this "impossible task" resulted in a mathematical framework that the client successfully adopted to inform their marketing strategies and inventory management with R2 score at 0.86.

________________________________________
📂 Technical Challenges & Complexity

Predicting sales from comment data presents several non-trivial hurdles:

•	Sparse & Noisy Data: Fragmented datasets due to system migrations, with some products having only 1–3 data points per month.

•	Dynamic Entity Mapping: Product "Keys" on JD.com are non-static; a single ID might represent Red Wine for one quarter and be repurposed for White Wine the next.

•	Long-Tail Distribution: Highly imbalanced data across the alcoholic beverage category.

•	Signal Uncertainty: Determining if a correlation exists between the timing of a comment and the timing of a transaction.

________________________________________
📂 Methodological Innovation: Mathematical Decomposition

To solve the lack of direct sales data, we developed a logic to derive Sales Volume ($V$) from Incremental Comments ($C$).

☑ The Core Hypothesis
The relationship was modeled as a weighted sum of current and lagged comment activities:
V_april = w1 * C1 + w2 * C2 + w3 * C3
Where $w_n$ represents the conversion ratio of users who purchased in month $n$ but commented in April.

☑ Formula Optimization & Simplification
Through continuous derivation and assumption testing, I simplified the complex multi-lag model into a more robust Dual-Variable Model:
V_april = alpha1 * C1 + alpha2 * C2
Testing proved that this Formula (B) significantly outperformed simpler single-variable models by capturing the "lagged" nature of consumer feedback while maintaining model stability.

☑ Dynamic Parameter Search
Instead of static weights, I leveraged a parameter search approach using limited ground-truth sales samples to identify the optimal $\alpha_1$ and $\alpha_2$ for each specific month. This accounted for seasonal variations in consumer behavior (e.g., Chinese New Year or 6.18 shopping festivals).
________________________________________

📂 Business Impact & Client Adoption

While initially framed as an exploratory experiment, the model's ability to capture sales trends from public-facing metadata provided the client with a unique competitive advantage:

•	Strategic Pivot: Enabled the client to adjust marketing spend based on the "engagement-to-sales" lag identified by the model.

•	Inventory Heuristics: Provided a reference point for stock replenishment in regions where direct sales data was delayed.

•	Success out of "Impossibility": The client ultimately moved this from a "test case" to a production-referenced tool for internal decision-making.

________________________________________
📂 Key Takeaways

☑ Mathematical Intuition: Ability to decompose business problems into solvable linear equations.

☑ Client Management: Managed expectations for a "practically infeasible" project while delivering a "practically useful" outcome.

☑ Data Scarcity Strategy: Expert at extracting signal from noisy, small, and long-tail datasets.

________________________________________

## Hybrid Forecasting System for Next Vehicle Service Date & Odometer

📂 Project Objective

The goal of this project is to predict the next service visit date and odometer reading for the entire customer base. By limiting the prediction deviation to within 30 days, the system empowers the business department to execute high-precision, data-driven marketing and personalized parts recommendations.

________________________________________
📂 Key Innovations

☑ Advanced Data Engineering (Anti-Leakage)

•	Wide-Format Transformation: Successfully pivoted original long-format transactional data into a wide-format structure. This crucial step prevents data leakage (temporal look-ahead bias), ensuring the model’s performance is robust and representative of real-world production.

☑ Hybrid "Model-Rule" Architecture

•	80/20 Strategy: 70% of high-quality, cleaned data is processed through a Gradient Boosting (LightGBM) algorithm.

•	Statistical rule-based method: The remaining 30% (including cold-start users and noisy data) is handled by a custom-built statistical rule-based model. 

•	This ensures 100% coverage of the customer base even when data is sparse.

☑ Granular Batch-Wise Learning

•	Both the algorithmic and statistical paths utilize a granular, batch-wise training approach, allowing the system to capture subtle shifts in user behavior patterns across different time windows.

☑ Multi-Metric Weighted Fusion

•	Implemented an innovative multi-metric weighted fusion approach within the ML model. This ensemble-like logic allows for superior performance using a single-model architecture, simplifying deployment while maintaining high accuracy.

________________________________________
📂 Technical Challenges & Solutions

•	Low-Loyalty Behavior: Captured patterns from low-frequency users by extracting latent features from sparse transaction histories.

•	Data Quality & Noise: Addressed high volumes of missing and noisy data through rigorous feature engineering and the hybrid fallback mechanism.

•	Model Robustness: Solved the risk of inflated performance by enforcing strict temporal splits and wide-format feature isolation.

________________________________________
📂 Tech Stack & Implementation

•	Core Model: LightGBM (LGB)

•	Data Processing: PySpark (Optimized for production scalability and high-speed ETL in Data Lake environment)

•	Programming: Python (Pandas, NumPy, Scikit-learn)

•	Logic: Hybrid Machine Learning + Statistical Rule Engine

________________________________________
📂 Results & Business Impact

The system was successfully deployed into a live production environment with the following accuracy metrics (defined as prediction error < 30 days):

LGB Model (Core) @ 40.07%

Statistical Rule Model @ 30.49%

Business Outcome: Enabled the marketing department to transition from "mass-blasting" to "predictive-trigger" notifications, significantly improving customer retention and car-part upsell conversion rates.
________________________________________

📂 Summary of Contributions

☑ Business Logic Deep-Dive: Translated complex automotive maintenance cycles into a quantifiable data problem.

☑ Wide-Format Design: Engineered a robust data schema to prevent leakage.

☑ Scalable Deployment: Utilized PySpark to ensure the pipeline is production-ready for millions of records.

☑ Ensemble Innovation: Created a weighted fusion logic that maximizes the predictive power of single-model deployments.
