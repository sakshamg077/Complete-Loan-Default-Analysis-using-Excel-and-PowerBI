# Loan Default Risk Analysis | Excel, Power Query \& Power BI

## Project Overview

This project analyzes loan default patterns across 148,670 loan records using Excel, Power Query, Pivot Tables, and Power BI. The goal was to identify which borrower and loan characteristics are associated with higher default rates, and to build an interactive dashboard that makes those patterns easy to explore.

The dataset came with several data quality issues that needed to be resolved before any analysis could happen. A few of those issues turned out to be significant findings in themselves, particularly around which fields could and could not be reliably used for default-risk analysis.



## Dataset

* **Total Records:** 148,670
* **Domain:** Banking / Financial Risk
* **Target Variable:** Status (1 = Default, 0 = Non-Default)
* **Overall Default Rate:** 24.64% (36,639 defaults)
* **Source:** https://www.kaggle.com/datasets/yasserh/loan-default-dataset



## Tools Used

* Microsoft Excel
* Power Query
* Pivot Tables and Pivot Charts
* Power BI Desktop
* DAX (for calculated measures in Power BI)



## Data Cleaning and Preprocessing

All cleaning was done in Power Query before any analysis. This included handling missing values, decoding abbreviated categorical values into readable labels, and creating DTI and LTV bucket columns to enable band-level analysis.



## Data Anomalies

Several unusual patterns were found during cleaning and analysis. Each one was investigated before deciding how to handle it.

### 1\. Interest Rate Data Lifecycle Finding

A cross-tab in Power Query (screenshot attached) revealed that 99.45% of all defaulted loans have no recorded interest rate, while every single non-defaulted loan has one. This pattern likely suggests that rate\_of\_interest is captured only after a loan reaches a later stage in the origination process that defaulted loans may never have completed. Both rate\_of\_interest and interest\_rate\_spread were excluded from default-risk analysis to avoid drawing conclusions from a field whose missingness is so closely tied to the outcome itself.

### 2\. Equifax Credit Bureau Anomaly

Loans with Equifax listed as the credit bureau showed a 99.99% default rate (15,297 out of 15,298 loans), compared to roughly 16% for all other bureaus. This likely points to a data encoding issue rather than a real pattern. Excluded from credit bureau analysis.

### 3\. Manufactured Home Cluster

33 loans classified as both "Indirect" security type and "Manufactured Home" showed a 100% default rate. The sample is far too small to draw any meaningful conclusion. Treated as a statistical outlier and excluded.

### 4\. Unknown Age Group

200 loans with unknown age also showed a 100% default rate. Cross-referencing confirmed these are the same 200 loans from the interest rate finding above. Excluded from age group analysis.

### 5\. DTI Zero Values

A large number of DTI values were recorded as null in the raw data. After correcting for this, these rows were placed in an "Unknown" bucket and excluded from DTI band analysis.

### 6\. LTV Below 60% Anomaly

The <60% LTV group showed the highest default rate at 41.18%, which is counterintuitive since lower LTV is supposed to indicate lower risk. Drilling down by loan type offered a possible explanation: commercial loans are disproportionately concentrated in this band and carry a 78.16% default rate within it. Once segmented by loan type, the LTV pattern aligns more conventionally.



## Key Insights

The full analysis across 11 questions is documented in the attached Excel pivot file. Key findings are summarized below.

**Loan Structure is the strongest risk signal**
Lump sum repayment loans defaulted at 77.66% vs 23.41% for standard repayment, the largest single gap in the entire dataset. Negative amortization loans defaulted at nearly double the rate of fully amortizing loans (44.60% vs 22.38%). Borrowers with structurally riskier loan terms defaulted at dramatically higher rates regardless of other characteristics.

**Commercial loans are consistently riskier than personal loans**
The 34.54% vs 23.04% gap between commercial and personal loans holds up across every segment tested including LTV bands and DTI bands. Loan purpose appears to be a stronger risk driver than most borrower-level characteristics.

**Investment properties carry more risk than primary residences**
Investment Residence borrowers defaulted at 29.99%, compared to 24.30% for Primary Residence. This is consistent with standard lending theory since investment property owners have less personal stake in avoiding default.

**The 25-44 age group is the safest segment**
Default rates show a clear U-shape by age. The 25-44 group had the lowest rates around 20-22%, while borrowers under 25 and over 74 defaulted at 28-30%. Both ends of the age spectrum carry meaningfully higher risk.

**Credit score was not a differentiating factor here**
Default rates stayed flat across all credit score bands at roughly 24-25%, which is unusual since credit score is typically one of the stronger predictors in mortgage lending. One possible reason is that very low credit score borrowers were filtered out at origination, leaving a narrower pool where score variation no longer separates risk clearly. This is a hypothesis rather than a confirmed finding.

**Higher DTI generally means higher default for personal loans**
For personal loans, default rates rise as DTI increases, which is consistent with lending theory. For commercial loans, default rates are elevated across all DTI bands regardless of level, reinforcing that loan purpose matters more than DTI for that segment.

**Regional differences exist but are not dramatic**
North-East (30.45%) and Central (27.54%) showed higher default rates than North (22.51%), but the overall spread is modest compared to the structural factors above.

**Smaller loans defaulted more**
Loans under 100k had a 37.10% default rate vs 21.90% for loans above 400k. This likely reflects the borrower segment at each loan size rather than loan amount being a direct risk driver.



## Power BI Dashboard

A two-page interactive dashboard was built in Power BI Desktop. All default rate calculations use DAX measures, so every chart updates dynamically when cross-filters are applied. Clicking any bar, slice, or data point filters the rest of the page automatically.

**Page 1: Overview**
High-level KPI cards alongside default rate breakdowns by borrower demographics and geography.

**Page 2: Risk Factors**
Focused on loan structure and financial characteristics, covering DTI, LTV, repayment structure, lump sum payment, and loan convention.



## Project Files

* Project files (Excel analysis + Power BI dashboard): (https://drive.google.com/drive/folders/1pSgUsm6dQmPPXKUVLVoLmCfeUroKboHV?usp=drive\_link)
* Interest rate anomaly screenshot (Power Query Group By output)



## Author

**Saksham Gaur**

Linkedin- www.linkedin.com/in/sakshamgaur077

