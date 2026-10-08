# Executive-healthcare-Analytics-Readmission-Risk
# Project Overview 
An interactive Power BI healthcare dashboard tracking 9,999 patients to analyze 30-day readmission risks, organ-specific chronic conditions, follow-up compliance, and $16.7M in financial penalties. Built using DAX, Star Schema, and dynamic slicers.

# Tech Stack
  * Business Intelligence: Power BI Desktop
  * Data Modeling & Calculations: DAX (Data Analysis Expressions), Star Schema Architecture
  * Data Preprocessing: Power Query M Code

# Key Business Problems Solved
* **Readmission Penalty Control:** Stratifying high-risk patients to proactively manage discharge planning and prevent high hospital readmission financial penalties.
* **Operational Efficiency:** Tracking Average Length of Stay (LOS) across emergency, elective, and urgent admission types.
* **Disease & Organ System Monitoring:** Analyzing chronic condition distributions across organ systems (Heart, Lungs, Kidneys, Metabolic) for personalized patient care strategies.

# Data Architecture & Modeling
* **Data Modeling:** Star Schema architecture connecting the clinical fact dataset (`hospital_readmission_risk`) with a custom `Dim_Calendar` dimension table.
* **Data Transformation:** Performed automated data cleaning, handling null values, standardizing dates, and dynamic feature formatting in Power Query.
* **Advanced DAX Calculations:** Built custom operational measures organized in a clean `_All Measures` table.


# Key DAX Measures Implemented
DEFINE
    MEASURE 'hospital_readmission_risk_10000'[Total Patients] = 
        COUNTROWS('hospital_readmission_risk_10000')
        
    MEASURE 'hospital_readmission_risk_10000'[Avg Length of Stay] = 
        AVERAGE('hospital_readmission_risk_10000'[length_of_stay])

    MEASURE 'hospital_readmission_risk_10000'[High Risk Patients] = 
        CALCULATE(
            COUNTROWS('hospital_readmission_risk_10000'),
            'hospital_readmission_risk_10000'[readmission_risk] = "High"
        )

    MEASURE 'hospital_readmission_risk_10000'[High Risk Readmission Rate %] = 
        DIVIDE([High Risk Patients], [Total Patients], 0)

    MEASURE 'hospital_readmission_risk_10000'[Estimated Financial Penalty] = 
        [High Risk Patients] * 5000
    
    MEASURE 'hospital_readmission_risk_10000'[Avg BMI] = 
        AVERAGE('hospital_readmission_risk_10000'[bmi])
    
    MEASURE 'hospital_readmission_risk_10000'[Avg Medications] = 
        AVERAGE('hospital_readmission_risk_10000'[medications_count])
    
    MEASURE 'hospital_readmission_risk_10000'[Organ_Health_Status] = 
        VAR SelectedDisease = SELECTEDVALUE('hospital_readmission_risk_10000'[chronic_conditions], "Select a Condition")
        RETURN
        SWITCH(
            TRUE(),
            SelectedDisease = "Heart Disease", "🫀 Heart Status: High Risk | Monitor Blood Pressure & Hemoglobin",
            SelectedDisease = "Asthma", "🫁 Lungs Status: Moderate Risk | Check Oxygen & Follow-up Compliance",
            SelectedDisease = "Diabetes", "🩸 Metabolic Status: High Risk | Glucose Tracking Required",
            "🩺 Organ Status: Normal / Multi-organ Monitoring"
        ) EVALUATE
    ROW (
        "Total Patients", [Total Patients],
        "Avg Length of Stay", [Avg Length of Stay].
        "High Risk Patients", [High Risk Patients],
        "High Risk Readmission Rate %", [High Risk Readmission Rate %],
        "Estimated Financial Penalty", [Estimated Financial Penalty],
        "Avg BMI", [Avg BMI],
        "Avg Medications", [Avg Medications],
        "Organ Health Status", [Organ_Health_Status]
    )

# Dashboard Explaination
# Page 1: Executive Overview & Risk Metrics.
       1 :  Key Performance Indicators (KPIs):
           * Total Patients: 9,999  
           * Avg Length of Stay: 15.05 days   
           * High Risk Patients: 3,333   
           * High Risk Readmission Rate: 33.3%   
           * Estimated Financial Penalty: $16.7M   
           * Avg BMI: 28.55   
           * Avg Medications: 4.43.   
       2 : Visual Breakdown & Insights:
           * Total Patients by Readmission Risk and Gender: Readmission risk shows an even distribution across High, Medium, and Low risk tiers (~16.5% – 16.8% per risk/gender group).    
           * High Risk Patients by Chronic Conditions: The high-risk patient volume is led by patients with "None" (no recorded chronic conditions) and "COPD, followed by Diabetes, Hypertension, and Heart Disease.   
           * Total Patients by Month: Monthly volume displays a steady decline from September through October, dropping sharply in November.   
           * Total Patients by Admission Type & Avg Stay: Elective (3,358), Emergency (3,351), and Urgent (3,290) admissions reflect an nearly equal patient split and an identical average length of stay (~15 days). 

 
# Page 2: Demographics, Insurance & Operational Risk Factors.
     # 1. Insurance Breakdown : 
         * Uninsured: 33.55% (3K)  
         * Private: 33.47% (3K)   
         * Public: 32.97% (3K)
        (Shows an equal distribution across all three coverage types).  
     # 2. Total Patients vs High Risk Patients Trend: 
        * Monthly volume tracking reveals that the proportion of high-risk patients remains consistent across all months.
     # 3. Readmission Risk by Gender and Chronic Conditions: 
        * Chronic condition distributions (COPD, Diabetes, Heart Disease, Hypertension, None) are balanced across both Male and Female cohorts.   
     # 4. Decomposition Tree (Smoking Status & Alcohol Use): 
        * Out of 9,999 chronic condition occurrences, Smoking Status splits evenly among Never (3,383), Current (3,380), and Former (3,236) smokers.   
        * Within the Never smoker group, Alcohol Use breaks down into Moderate (1,164), High (1,146), and None (1,073).   

# Page 3: Temporal Trends & Follow-up Compliance.      
     # 1. Total Patients by Month and Year (Waterfall Chart): 
        * Illustrates month-over-month patient volume fluctuations for 2025, highlighting increases in March, May, July, and October (green bars) alongside monthly decreases (red bars).
    # 2. Follow-up Compliance by Chronic Conditions: 
        * Compliance rates are highest among patients with Diabetes and those with "None" (~2,030+ patients). * Compliance drops significantly for Heart Disease patients (~1,920 patients).
    # 3. Total Patients by Previous Admissions & Follow-up Compliance: 
        * Good Compliance (Blue line): Peaks for patients with 0, 3, or 8 previous admissions.   
        * Poor Compliance (Teal line): Increases steadily from 0 to 5 previous admissions before fluctuating.
 #  How to View & Explore
1. Clone or download the `.pbix` file from this repository.
2. Open `Healthcare_Analytics_Dashboard.pbix` in **Power BI Desktop**.
3. Use the page tabs and cross-report sync slicers to interact with disease categories and organ risk stratification.                                                                       
