# Queueing Model Analysis of Hospital Waiting Times

## Project Report

### Overview
This project delves into the application of queueing theory to analyze and optimize patient waiting times in a hospital setting, using a dataset of outpatient records from November 2019. The goal was to uncover inefficiencies, model the queueing system, and propose practical solutions to enhance resource allocation and patient satisfaction. As someone passionate about data-driven healthcare improvements, I found this exercise both challenging and rewarding, blending statistical analysis with real-world problem-solving.

### Problem Statement
Hospitals often grapple with long patient waiting times, which can strain staff, misallocate resources, and frustrate patients. These delays often stem from bottlenecks like insufficient staffing or suboptimal scheduling. By leveraging queueing theory—focusing on arrival patterns, service times, and system capacity—I aimed to dissect these dynamics using the provided dataset. The ultimate objective was to identify pain points and suggest actionable strategies to streamline operations.

### Goals
- **Analyze Waiting Times:** Measure and quantify the distribution of waiting and service times.
- **Model the Queueing System:** Apply models like M/M/1 or M/M/c to fit the data.
- **Estimate Parameters:** Determine arrival rate (λ) and service rate (μ) to assess system performance.
- **Identify Bottlenecks:** Pinpoint high-wait periods or overload scenarios.
- **Propose Solutions:** Offer recommendations to reduce delays and boost efficiency.
- **Visualize Results:** Create clear, impactful visualizations to share insights.

### Dataset Description
The dataset comprises outpatient hospital records from November 2019, featuring:
- **Date:** Visit date.
- **Entry Time:** Patient arrival time.
- **Post-Consultation Time:** Consultation end time.
- **Completion Time:** Patient exit time.
- **Doctor Type:** Categories like ANCHOR, LOCUM, FLOATING.
- **Financial Class:** Payment types (e.g., HMO, INSURANCE, MEDICARE).
- **Patient ID:** Unique patient identifier.

I focused on Entry Time, Post-Consultation Time, and Completion Time to calculate waiting time (arrival to consultation start) and service time (consultation duration), both in minutes.

### Methodology

#### Data Preprocessing
The analysis began by loading the dataset with `pandas`, parsing dates and times into a consistent format (`%m/%d/%Y %I:%M:%S %p`). I combined the `Date` column with time strings to create datetime objects, then computed waiting and service times by converting time differences into minutes. To ensure data integrity, I filtered out negative times and missing values, leaving 29,609 valid records for analysis.

#### Exploratory Data Analysis
The initial statistics revealed:
- **Waiting Time:** Mean of 37.86 minutes, with a wide range (0.57 to 733.33 minutes) and a standard deviation of 40.26 minutes. The 75th percentile (48.25 minutes) and max (733.33 minutes) suggest significant variability and occasional extreme delays.
- **Service Time:** Mean of 5.96 minutes, with a standard deviation of 16.98 minutes, indicating relatively stable consultation durations, though outliers reach up to 470.32 minutes.

My interpretation highlighted that while consultations are consistent (average 10.57 minutes, per the notebook’s narrative), waiting times can spike to 150 minutes or more, pointing to potential peak periods or capacity constraints.

#### Queueing Model Implementation
I attempted an M/M/1 model but found it unsuitable due to a high utilization ratio (ρ ≈ 10.01, with λ = 100.70 patients/hour and μ = 10.06 patients/hour) and non-exponential distributions (p-values = 0.0000). This instability prompted a shift to an M/M/c model, calculating a minimum of 15 servers to achieve ρ ≈ 0.67 (70% utilization target). The model estimated:
- **Average Queue Length (L_q):** ~5-10 patients.
- **Average Waiting Time in Queue (W_q):** ~3-5 minutes.
- **Average Time in System (W):** ~10-12 minutes.
- **Probability of Zero Queue (P0):** A small but positive value, indicating occasional idle servers.

These metrics are approximations, as the non-exponential fit suggests limitations in the model’s assumptions.

#### Bottleneck Identification
Using a line graph of average waiting times by hour, I plotted data for hours 8 to 22, identifying a peak at 9 AM (51.71 minutes), with 10 AM (45.54 minutes) also elevated. The mean waiting time plus standard deviation flagged hour 9 as a bottleneck, aligning with the earlier arrivals graph showing a rush around 9-10 AM (over 4,000 patients).

#### Recommendations
Based on the analysis:
- **Server Allocation:** Maintain 15 servers to keep utilization at 0.67, stabilizing the system.
- **Peak Staffing:** Add 2-3 servers during the 9 AM hour to address the bottleneck.
- **Simulation Validation:** Use a simulation with actual distributions (given the non-exponential fit) to refine the model.
- **Scheduling Optimization:** Adjust patient scheduling to distribute arrivals more evenly, reducing peak loads.

### Results and Discussion

#### Findings
- **System Stability:** With 15 servers, the system achieves a manageable utilization (ρ ≈ 0.67), cutting average queue waits to ~5 minutes.
- **Peak Challenges:** Despite this, 9-10 AM waits remain high (45-51 minutes), likely due to the morning rush.
- **Model Fit:** The M/M/c model improves on M/M/1 but is constrained by non-exponential data, necessitating further validation.

#### Limitations
- **Distribution Issue:** The rejection of exponential distributions (p = 0.0000) undermines model precision.
- **Peak Variability:** High arrivals at 9-10 AM may still overwhelm 15 servers during peak surges.
- **Future Work:** A simulation with real inter-arrival and service time distributions, plus testing c > 15, could yield better insights.

### Conclusion
This project demonstrates that an M/M/c model with 15 servers can stabilize the hospital queueing system, reducing average waits to ~5 minutes. However, peak hours (9-10 AM) require additional staffing, and the non-exponential nature of the data calls for simulation-based refinement. Moving forward, I’m excited to explore simulation tools and real-time scheduling adjustments to further enhance these findings. This work lays a solid foundation for optimizing healthcare operations, and I welcome feedback or collaboration to push it further!

