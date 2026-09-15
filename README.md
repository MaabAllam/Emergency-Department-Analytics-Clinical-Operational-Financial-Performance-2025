# Emergency-Department-Analytics-Clinical-Operational-Financial-Performance-2025
End-to-end Emergency Department analytics project using SQL, Python and Power BI to evaluate clinical, operational, financial and resource performance.
# Emergency Department Analytics
-- Section 1- Patient_Flow_Analysis
-- ------------------------------------
use health_care_project;
-- Question 1:
-- How many Patients visited the Emergency Department?
-- KPI
-- Total ED visits
select count(patient_id) as total_ed_visits
 from emergency_department_dataset;
 -- Business Interpretation:
 -- Measures the Total workload of the emergency Department
 -- Serves as the baselines for all other KPIs
 -- Helps managment evaluate demand and compare performance across different periods
 -- ----------------------------------------------------------------------------------
 -- Question 2:
 -- What is the average waiting time?
 -- KPi
 -- Average Waiting Time
 select * from emergency_department_dataset;
 select  round(avg(wait_time_minutes),2) as avg_waiting_time
 from emergency_department_dataset;
 -- Business Interpretation
 -- Indicate opertional efficiency
 -- A high verage waiting time may reduce patient satisfaction and delay tretment
 -- can be compared against hospital targets or national benchmarks
 -- ---------------------------------------------------------------------------------
 -- Question 3 :
 -- What is the average Length Of Stay?
 -- kPI
 -- Average lOS
 select round(avg(length_of_stay_minutes),2) as Avg_length_of_stay
 from emergency_department_dataset;
 -- Business Interpretaion:
 -- Measures How long patients remain in the ED
 -- A prolonged LOS may indicate workflow bottlenecks,delayed discharges,or bed shortages
 -- --------------------------------------------------------------------------------------------
 -- Question 4:
 -- Which shift recevies the highest number of patients?
 -- KPI
 -- Patient Volume by Shift
 select shift, count(patient_id) as patient_volume
 from emergency_department_dataset
 group by shift
 order by patient_volume desc;
 -- Business Interpretation:
 -- Measures the volume of patient per each shift
 -- it may act as a base for further analysis on each shift needs to counter act the patient volume
 -- helps in undestanding the supply and demand on the basic of shift
 -- Recommendations:
 -- If One Shift Consistently experiences substantially higher patient volumes, hospital managment should evaluate whether staffing
 -- levels, room availabilty, and support services during that shift are sufficient to meet demand
 -- additional analysis should compare patient volume with waiting time and triage acuity before making staffing decisions.
 -- ----------------------------------------------------------------------------------------------------------------------------------
 -- Question 5:
 -- Which day of the week has the highest Emergency Department patient volume?
 -- KPI
 -- Patient Volume per Day
 select day_of_week , count(patient_id ) as patient_volume_per_day
 from emergency_department_dataset
 group by day_of_week
 order by patient_volume_per_day desc;
 -- Business Interpretation :
 -- Measures volume of patients per day
 -- serves as baseline for other KPIs releated to the days of the week
 -- Helps mangement to eavaluate demand and compare performance across differnt days
 -- Recommendations:
 -- if one day show higher volume of patient than other days hospital mangment should evaluate the staff avalabilty room occupancy and the medical services
 -- during this day to meet the demand also additional analysis may took place by comparing the waiting time and triage acuity before making staffing decisions
 --  ------------------------------------------------------------------------------------
 -- Question 6:
 -- At what hour of the day does the Emergency Department receive the highest number of patients?
 -- KPI
 -- Patient Volume by Hour
 select concat(LPAD(Hour(arrival_datetime),2,'0'),':00') as arrival_hour,
 count(patient_id) as Patient_volume
 from emergency_department_dataset
 Group by arrival_hour
 order by Patient_volume DESC;
 -- Business Interpretation:
 -- Patient arrivals are relatively evenly distributed throughout the day,
 -- with a slight increase during the late evening. Although 23:00 records the highest number of arrivals,
 -- the variation across hours is modest, suggesting that patient demand remains relatively stable throughout the day in this dataset.
 -- Recommendation:
 -- Since hourly variation is limited, staffing decisions should not rely solely on arrival volume.
 -- Additional analyses, including waiting time, triage acuity, and length of stay, should be evaluated to identify operational bottlenecks.
 -- ----------------------------------------------------------------------------------------------------------------------------------------------
 -- Question 7 :
 -- Which Shift has the highest average patient waiting time?
 -- KPI
 -- Average Waiting Time by Shift
 select shift,
 count(patient_id) as Patient_Volume
 ,Round(avg(wait_time_minutes),2) as Avg_waiting_time from emergency_department_dataset
 group by shift
 order by Avg_waiting_time DESC;
 -- Business Interpretation:
 -- The average waiting time is slightly differnt in the three shifts during the day
-- Although patient volume varies considerably across shifts,
-- average waiting times remain very similar. This suggests that factors other than patient volume
-- may have a greater influence on waiting times within this dataset.
 -- Recommendtions:
 -- "Based on this analysis alone, I wouldn't recommend additional staffing. Although the Day Shift has the highest patient volume,
 --  its average waiting time is actually the lowest of the three shifts. This suggests that current staffing may already be appropriate. Before making staffing decisions,
 --  I'd investigate triage acuity, diagnostic turnaround time, bed occupancy, and patient length of stay."
-- -------------------------------------------------------------------------------------------------------------
 -- Question 8 :
 -- Which triage acuity level has the highest average waiting time?
 -- KPI
 -- Average Waiting Time by Triage Acuity
 select triage_acuity,
 count(patient_id) as Patient_Volume
 ,Round(avg(wait_time_minutes),2) as Avg_waiting_time from emergency_department_dataset
 group by triage_acuity
 order by Avg_waiting_time DESC;
 -- Business Interpretion:
 -- This analysis shows how long each level is waiting in the ED
 -- this analysis will need more investigation on the bases of this result
 -- Recommendation :
 -- The analysis indicates that higher-acuity patients experience longer average waiting times than lower-acuity patients
 -- Because this differs from expected Emergency Department practice, the result should be validated by confirming the definition of the waiting time
 -- variable and assessing whether the dataset reflects real operational processes or educational sample data
 -- Key Finding:
-- Contrary to expected Emergency Department prioritization, higher-acuity patients in this dataset have longer average waiting times than lower-acuity patients.
-- This unexpected pattern requires validation before operational conclusions are made.
 -- -------------------------------------------------------------------------------------------------------------------------------------
 -- Question 9:
  -- What is the average Length of Stay (LOS) by Shift?
  -- KPI
  -- Average Length of Stay by Shift
   select shift,
 count(patient_id) as Patient_Volume
 ,Round(avg(length_of_stay_minutes),2) as Avg_LOS from emergency_department_dataset
 group by shift
 order by Avg_LOS DESC;
 -- Business Interpretaion:
 -- This may be due to Delays in patient Discharge
 -- or due to delays in Diagnostic Images
 -- staff handover delays
 -- Recommendations:
-- Average Length of Stay is remarkably consistent across all three shifts
--  suggesting that the overall patient journey is relatively stable throughout the day.
--  Based on this analysis alone, there is no strong evidence that one shift experiences substantially longer patient stays than the others.
-- Key Finding
-- Although patient volume varies across shifts, Average Length of Stay remains very similar.
--  Based on this analysis, shift alone does not appear to explain differences in patient LOS.
-- ------------------------------------------------------------------------------------------------------
--  Section Two :
--  Operational Analysis
-- -------------------------------------------------------------------------------------------
-- Question 10 :
-- Which age group has the highest Average Length of Stay(LOS)?
-- KPI
-- Average LOS by age group
select age_group ,count(patient_id),round(avg(length_of_stay_minutes),2) as Avg_lOS
 from emergency_department_dataset
 group by age_group
 order by  Avg_LOS DESC;
-- Business Interpretaion:
-- there are 4 age group in this data set
-- the biggest age group is the young adult (18-39)
-- the average LOS is slightly differ through out the whole ages groups
-- Recommendations:
-- Average Length of Stay is relatively consistent across all age groups.
-- Although Young Adults (18–39 years) have the highest average LOS,
-- the variation between age groups is small,
-- suggesting that age alone is unlikely to be a major driver of patient Length of Stay within this dataset.
-- key findings:
-- Although patient volume differs among age groups,
-- Average Length of Stay remains relatively consistent,
-- suggesting that age alone does not explain differences in patient LOS.
-- -------------------------------------------------------------------------------------------------------------
-- Question 11:
--  What are the most common chief complaints among Emergency Department patients?
-- KPI:
-- Patient Volume by Cheif Complaint
select chief_complaint, count(patient_id) as Patient_volume
 from emergency_department_dataset
 Group by chief_complaint
 order by Patient_volume DESC;
 -- Business Interpretation:
 -- according to this analysis Trauma complaints are the highest patient volume
 -- there should be more clinical and opertional analysis to make sure that medical supply and staff are capable to support this demand
 -- Recommendations:
 -- Trauma represents the highest-volume chief complaint in this Emergency Department dataset.
 -- This suggests that trauma-related cases consume a significant portion of ED resources
 -- and should be considered when planning staffing, equipment availability, and clinical workflows.
 -- key findings
 -- Trauma and Chest Pain account for the largest proportion of Emergency Department visits in this dataset,
 --  indicating that these conditions should be prioritized in future operational and clinical analyses
 -- ------------------------------------------------------------------------------------------------------------
 -- Question 12:
 -- Which chief complaints have the longest Average Length of Stay (LOS)?
 -- KPI:
 -- Avg LOS by chief complaints
 select chief_complaint,
 count(patient_id) as Patient_volume,
 Round(avg(length_of_stay_minutes),2) as Avg_LOS from emergency_department_dataset
 group by chief_complaint
order by Avg_LOS DESC ;
-- Business Interpretaion:
-- Patients presenting with Stroke Symptoms and Chest Pain have the longest average Length of Stay.
-- These conditions often require urgent assessment, advanced imaging,
-- specialist consultation, and possible admission,
-- which may contribute to longer stays in the Emergency Department.
-- Recommendations :
-- Further analysis should evaluate whether the prolonged LOS for Stroke Symptoms
-- and Chest Pain is associated with diagnostic imaging,
-- specialist consultations, admission delays,
-- or bed availability before implementing operational changes.
-- key findings:
-- Stroke Symptoms and Chest Pain have the longest average Length of Stay in this dataset,
-- suggesting these patient groups place a greater demand on Emergency Department
-- resources and should be prioritized for operational review.
-- -----------------------------------------------------------------------------------------------------
-- Observation:
-- "Chest Pain deserves immediate operational attention because it combines a high patient volume with one of the longest average Lengths of Stay.
--  This combination suggests a significant impact on Emergency Department capacity and resource utilization."
-- -----------------------------------------------------------------------------------------------------------------------------------------------------
-- Question 13:
-- which chief complaint have the longest average waiting time?
-- KPI
-- Avg waiting time by chief Complaint
select chief_complaint ,
count(patient_id) as Patient_volume,
 Round(avg(wait_time_minutes),2) as Avg_waiting_time
from emergency_department_dataset
group by chief_complaint
order by Avg_waiting_time DESC;
-- Busniess Interpretation:
-- Stroke Symptoms have the highest average waiting time,
-- which may contribute to their longer average Length of Stay.
-- Additional analyses are needed to determine whether imaging,
-- specialist consultation, or admission delays also influence LOS
-- Recommendation:
-- Future analyses should evaluate diagnostic turnaround time,
 -- bed occupancy, specialist consultation time,
-- and admission workflow to determine which factors contribute most to prolonged waiting time
-- and Length of Stay for patients presenting with Stroke Symptoms.
-- key findings:
-- Stroke Symptoms have the highest average waiting time in the dataset.
-- However, waiting times across chief complaints differ by less than four minutes
-- , suggesting that complaint type alone does not substantially influence waiting time.
-- ------------------------------------------------------------------------------------------------------------
-- show columns from emergency_department_dataset;
-- -------------------------------------------------------------------------------------------------------------
-- -- Question 14:
-- Which arrival mode has the highest average waiting time?
-- KPI
-- Avg waiting time by arrival mode
select arrival_mode ,
count(patient_id) as Patient_Volume,
 round(avg(wait_time_minutes),2) as avg_waiting_time
from emergency_department_dataset
group by arrival_mode
order by avg_waiting_time DESC;
-- Business Interpretion:
-- Average waiting time is relatively similar across all arrival modes.
 -- Ambulance arrivals have the shortest average waiting time,
-- which is consistent with Emergency Department prioritization of potentially higher-acuity patients.
-- Recommendation:
-- Although waiting times differ only slightly across arrival modes,
 -- future analyses should evaluate whether triage acuity, patient volume
-- and arrival mode together influence Emergency Department
 -- waiting time before operational changes are considered.
-- Key findings:
-- Waiting times are relatively consistent across all arrival modes,
-- with Ambulance arrivals experiencing the shortest average waiting time and Referral patients the longest.
-- However, the overall variation is small (approximately two minutes).
-- -----------------------------------------------------------------------------------------------------------------
-- Question 15:
-- Which hospital has the highest average waiting time?
-- KPI
-- Avg waiting time by Hospital_name
show columns from emergency_department_dataset;
select hospital_name,
 count(patient_id) as patient_volume,
 round(avg(wait_time_minutes),2) as Avg_waiting_time
 from emergency_department_dataset
 group by hospital_name
 order by Avg_waiting_time DESC;
 -- -- Business Interpretion:
-- Average waiting time is relatively similar across all hospitals.
 -- St.Mary ED have the shortest average waiting time,
-- Recommendation:
-- Although waiting times differ only slightly across hospitals ,
 -- future analyses should evaluate whether triage acuity,
-- and arrival mode together influence Emergency Department
 -- waiting time before operational changes are considered.
 -- -- Key findings:
-- Waiting times are relatively consistent across all hospitals,
-- with St.Mary ED experiencing the shortest average waiting time and Metro Health  the longest.
-- However, the overall variation is small (approximately two minutes).
-- ------------------------------------------------------------------------------------------------------
-- Question 16:
-- what is the Average Boarding Time by Shift
-- KPI:
-- Average Boarding Time by Shift
select shift, round(avg(boarding_time_minutes),2) as Avg_boarding_time,
count(patient_id) as patient_volume
from emergency_department_dataset
group by shift
order by Avg_boarding_time DESC;
-- -- Business Interpretion:
-- Average boarding time is relatively similar across all the day shifts.
 -- Night shift have the shortest average boarding time,
 -- Recommendation:
-- Although boarding times differ only slightly across shifts ,
 -- future analyses should evaluate whether triage acuity,
-- and arrival mode, bed availabilty  together influence Emergency Department
 -- boarding time before operational changes are considered.
 -- -- Key findings:
-- boarding times are relatively consistent across all shifts,
-- with night shift the shortest average waiting time and Day shift the longest.
-- However, the overall variation is small (approximately one minute).
-- ------------------------------------------------------------------------------------------------------
-- Question 17:
-- Which hospital has the longest average boarding time?
-- kPI
-- Avg boarding time by Hospital
select hospital_name,
round(avg(boarding_time_minutes),2) as Avg_boarding_time,
count(patient_id) as patient_volume
from emergency_department_dataset
group by hospital_name
order by Avg_boarding_time DESC;
-- Business interpretation:
-- Average boarding time is relatively consistent across all hospitals,
--  suggesting that inpatient bed availability and admission processes remain stable throughout the hospitals
--  Although the St.Mary ED has the longest average boarding time, the variation between hospitals
-- is small and may not indicate a meaningful operational difference.
-- Recommendations:
-- Future analyses should evaluate inpatient bed availability,
--  admission workflow efficiency, discharge planning,
-- and hospital occupancy to determine whether these factors contribute
--  to boarding delays before operational changes are implemented
-- key findings:
-- Boarding time varies by approximately one minute across the hospitals
--  suggesting that hospitals boarding timing alone is unlikely to be a major contributor to Emergency Department boarding delays.
-- ---------------------------------------------------------------------------------------------------------------------------------
-- Question 18:
-- Which shift has the longest average service time?
-- KPI:
-- Avg service time by shift
show columns from emergency_department_dataset;
select shift, round(avg(service_time_minutes),2) as Avg_service_time,
count(patient_id) as patient_volume
from emergency_department_dataset
group by shift
order by Avg_service_time DESC;
-- Business Interpretation
-- Consistent clinical practices.
-- Potential night constraints:
-- Fixed process bottlenecks
-- Recommendations:
-- Audit night diagnostics: Investigate if overnight delays in lab results or radiology imaging .
-- Examine patient acuity: Cross-reference this data with a triage scale (like the Emergency Severity Index) to check if night patients are simply sicker.
-- Review discharge barriers: Pinpoint why evening shifts handle high volumes faster, and apply their coordination tactics to day and night crews.
-- Analyze waiting times: Focus process improvements on waiting room queues, as the actual hands-on care time is already highly optimized and stable.
-- Key findings:
-- Highly uniform process
-- Night shift highest
-- Evening shift lowest
-- Minor operational variance
-- ------------------------------------------------------------------------------------------------------------------------------
-- Question 19
-- Which chief complaints have the longest average service time?
-- KPI:
-- Avg Service time by Chief complaints
select chief_complaint, round(avg(service_time_minutes),2) as Avg_service_time,
count(patient_id) as patient_volume
from emergency_department_dataset
group by chief_complaint
order by Avg_service_time DESC;
-- Business Interoretaion:
-- Stroke Symptoms,Chest Pain,Shortness of Breath have the longest average service time
-- which may be due to resource allocation, triage severity,process bottlencks
-- Recommendations:
-- further analysis may took place to work more on the causes that may effect the longest service time like labrortray utilization
-- Medication Administration ,triage acutiy, admission status to determine which factors contribute most prolonged service time for the chief complaints
-- key findings:
-- variation in Service time across the chief complaints
-- stroke symptoms is the longest service time chief complaints
-- further analysis should tok place regarding resource allocation and labrotary utilization
-- --------------------------------------------------------------------------------------------------
-- question 20
-- which region Type has the highest Emergency Department patient volume?
-- KPI
-- Patient Volume by Region Type
select region_type , count(patient_id) as Patient_volume
from emergency_department_dataset
group by region_type
order by Patient_volume desc;
-- Business Interpretion:
 -- Measures the volume of patient per each region
 -- it may act as a base for further analysis on each region needs to counter act the patient volume
 -- helps in undestanding the supply and demand on the basic of region
 -- Recommendations:
 -- If One region Consistently experiences substantially higher patient volumes, hospital managment should evaluate whether staffing
 -- levels, room availabilty, and support services during that shift are sufficient to meet demand
 -- additional analysis should compare patient volume with waiting time and triage acuity before making staffing decisions.
 -- Key Findings:
 -- Urban region shows the highest patient_volume comparing to other regions
 -- ----------------------------------------------------------------------------------------------------------------------------------
 -- Question 21 :
 -- How does Emergency Department performance differ between weekdays and weekends?
 -- KPI:
 -- Emergency Department Performance
 show columns from emergency_department_dataset;
 select  is_weekend,
 count(patient_id) as Patient_volume,
  round(avg(wait_time_minutes),2) as Avg_waiting_time,
 round(avg(length_of_stay_minutes),2) as Avg_length_of_stay
 from emergency_department_dataset
 group by is_weekend
 order by is_weekend DESC;
 -- Business Interpretation:
 --  Although weekdays experience substantially higher patient volumes than weekends,
 -- average waiting time and Length of Stay remain remarkably consistent
--  This suggests that Emergency Department operations
 -- maintain similar performance despite increased weekday demand.
 -- Recommendations:
 -- further analysis on Capacity planning and staffing Bottlenecks
-- also analyse Revenue and resource utilization, patient experience and quality of care
-- optimize weekday high volume flow and stabilize weekend opertional blocks
-- key findings:
-- Weekdays receive significantly more Emergency Department visits than weekends;
--  however, operational performance remains stable,
--  as reflected by nearly identical average waiting times and Lengths of Stay.
-- --------------------------------------------------------------------------------------------------
-- Question 22:
-- How do Emergency Department patient volume, average waiting time, and average Length of Stay change throughout the year?
-- KPI:
-- Monthly Trend Analysis
select month,count(patient_id) as patient_volume,
round(avg(wait_time_minutes),2) as Avg_waiting_time,
 round(avg(length_of_stay_minutes),2) as Avg_length_of_stay
 from emergency_department_dataset
 group by month
 order by month ;
 -- Business Interpretation :
  -- Although months experience substantially varrying in  patient volume ,
 -- average waiting time and Length of Stay remain remarkably consistent
--  This suggests that Emergency Department operations
 -- maintain similar performance despite this volume variation demand.
 -- Recommendations:
 --  further analysis on Capacity planning and staffing Bottlenecks
-- also analyse Revenue and resource utilization, patient experience and quality of care
-- key findings:
--  throught out the year the ED show slight variation on average waiting time and average length of stay
-- patient volume has slight or no effects on the average waiting time and length of stay
-- --------------------------------------------------------------------------------------------------------------
 -- Section Three:
 -- Clinical Analysis:
 -- Section 1- Patient Outcomes
 -- -------------------------------------------------
 -- Question1 :
 -- What percentage of Emergency Department patients are admitted to the hospital?
 -- KPI:
 -- Admisson Rate
 select admitted , count(patient_id) as patient_volume,
 round(count(patient_id) * 100.0 /
 (select count(*) from emergency_department_dataset),2) as admission_rate
 from emergency_department_dataset
 group by admitted;
 -- Business Interpertation:
 --  Approximately 15% of Emergency Department patients required hospital admission, while the majority were treated and discharged
 -- This suggests that most ED visits were managed without requiring inpatient care,although admitted patients are likely to represent
 -- more clinically complex cases requiring additional hospital resourses
 -- Recommendations
 -- Analyze admission rates by triage acuity.
-- Compare admission rates across chief complaints.
-- Evaluate ICU admissions among admitted patients.
-- Assess Length of Stay for admitted versus discharged patients.
-- Review resource utilization for admitted patients.
-- key findings:
-- Approximately one out of every six Emergency Department patients required hospital admission,
-- while the majority were safely discharged.
-- Admission status should be further analyzed alongside triage acuity,
-- ICU utilization, and chief complaints to better understand clinical complexity and resource requirements.
-- --------------------------------------------------------------------------------------------------------------------
-- Question 2 :
-- What is the distribution of Emergency Department patient dispostions?
-- KPI:
-- Patient Disposition Distribution
select disposition,
count(patient_id) as patient_volume
from emergency_department_dataset
group by disposition
order by patient_volume DESC;
-- Business interpretation:
-- -Most Emergency Department patients were discharged after receiving treatment,
--  indicating that the majority of cases were managed without requiring inpatient admission.
--  However, the presence of admitted, transferred,
-- left-without-being-seen, and expired patients highlights
-- different clinical pathways that require
--  further investigation to evaluate patient outcomes, resource utilization, and quality of care.
-- Recommendation:
--   Analyze admitted  by triage acuity and most often chief complaints
-- Review resource utilization for admitted patients.
-- try to analyse the main clinical causes for the dispostion categories regarding the patient volume
-- Key findings:
-- Discharged patients accounted for the majority of Emergency Department visits,
-- while smaller but clinically important groups—including admitted,
-- transferred, left-without-being-seen,
--  and expired patients—represent key areas for further clinical and operational investigation.
-- -----------------------------------------------------------------------------------------------------------------------------
-- Question 3:
-- What percentage of Emergency Department patients required ICU admission?
-- KPI:
--  ICU Admission Rate
SELECT
    CASE
        WHEN icu_flag = 1 THEN 'ICU Admission'
        ELSE 'No ICU Admission'
    END AS ICU_Status,
    COUNT(patient_id) AS patient_volume,
    ROUND(
        COUNT(patient_id) * 100.0 /
        (SELECT COUNT(*) FROM emergency_department_dataset),2
    ) AS icu_admission_rate
FROM emergency_department_dataset
GROUP BY icu_flag;
-- Business interpretation:
-- Only 0.59% of Emergency Department patients required Intensive Care Unit (ICU) admission,
-- indicating that the vast majority of patients were managed without requiring critical care. Although
-- ICU admissions represent a very small proportion of total visits,
-- they are among the most clinically complex patients and require
-- significant hospital resources, specialized staff, and continuous monitoring.
-- Recommondetion;
-- Analyze ICU admissions by triage acuity.
-- Identify the chief complaints most frequently associated with ICU admission.
-- Compare ICU admission rates across age groups.
-- Evaluate the Length of Stay of ICU patients.
-- Review resource utilization, including laboratory tests and medications, among ICU patients.
-- Key Findings
-- ICU admissions accounted for only 0.59% of Emergency Department visits, indicating that critical care cases were relatively uncommon.
-- Despite their low frequency,
-- these patients represent the highest level of clinical severity
-- and should be prioritized for further analysis to understand resource utilization and patient outcomes.
-- ------------------------------------------------------------------------------------------------------------------------------
-- Question 4 :
-- How many patients left the Emergency Department without being seen (LWBS)?
-- KPI:
-- Left without being seen(LWBS)Rate(%)
select disposition, count(patient_id) as patient_volume,
 round(count(patient_id) * 100.0 /
 (select  count(*) from emergency_department_dataset),2) as rate_LWBS
 from emergency_department_dataset
 where disposition = 'Left Without Being Seen';
-- Business Interprtion:
-- Approximately 5.65% of Emergency Department patients left without being seen before receiving medical assessment.
-- Although the majority of patients completed their care,
-- this proportion is clinically and operationally significant
-- because patients who leave without evaluation may experience delayed diagnosis,
-- worsening medical conditions, reduced patient satisfaction, and missed treatment opportunities.
-- This KPI also represents a potential loss of hospital revenue and should be monitored as an important quality indicator.
-- Recommendation:
-- Investigate the average waiting time contrubute with LWBS rate
-- Analyze LWBS cases by shift, hour of arrival, and day of the week to identify periods with the highest risk.
-- Compare LWBS patients by triage acuity to determine whether lower-acuity patients are more likely to leave before evaluation.
-- Review chief complaints among LWBS patients to identify whether specific clinical presentations are associated with a higher likelihood of leaving.
-- Key findings:
-- Approximately one out of every eighteen Emergency Department patients left without being seen,
-- making LWBS an important patient safety and operational quality indicator.
--  Further investigation is required to determine whether waiting times, patient flow, or clinical characteristics contribute to these departures.
-- ---------------------------------------------------------------------------------------------------
-- Data Validation Note:
-- During analysis, a discrepancy was identified between the disposition and left_without_being_seen fields.
-- While only 11 patients were flagged as left_without_being_seen = Yes, a total of 565 patients had a disposition of "Left Without Being Seen."
 -- Because the disposition field provided a complete classification of patient outcomes,
-- it was used as the primary source for this analysis.
-- This inconsistency should be reviewed as a potential data quality issue before operational reporting.
-- -----------------------------------------------------------------------------------------------------------------------------------------
-- Question 5:
-- Which triage acuity level has the highest hospital admission rate?
-- KPI:
-- Hospital Admission Rate by Triage Acuity:
select triage_acuity,
 count(patient_id) as patient_volume,
 sum(case
 when admitted = 'Yes' then 1
 else 0
 end) as admitted_patients,
 round(sum(case
 when admitted = 'Yes' then 1
 else 0
 end)
 * 100.0 / count(patient_id),2) as admission_rate
 from emergency_department_dataset
 group by triage_acuity
 order by admission_rate DESC;
 -- Business Interpretain
 --  Hospital admission rates decreased consistently as triage acuity decreased
 -- Patients classified as Resuscitation and Emergent had the highest likelihood of hospital admission
 -- reflecting the greater clinical severity and needs for inpatient management
 -- Incontrast less Urgent and non urgent patients were more frequently treated and discharged from the ED without requiring hospitalization
 -- Recommendations
 -- Analyze the chief complaints among admitted patients to identify the clinical conditions most strongly associated with hospitalization
 -- compare admission rates with ICU admission to understand wether the highest triage acuity also require critical care
 -- evaluate length of stay for admitted patients across different triage levels
 -- key findings
 -- triage acuity demonestrated a strong relationship with hospital admission
 -- Patients with higher clinical severity were substantially more likely to require inpatient care
 -- confirming that triage acuity is an important predictor of hospital resource utilization and admission planning
-- ---------------------------------------------------------------------------------------------------------------------------------------------
-- Question 6:
-- which triage acuity level has the highest ICU admission rate?
-- KPI
-- Hospital  ICU Admission Rate by Triage Acuity:
select triage_acuity,
 count(patient_id) as patient_volume,
 sum( CASE
        WHEN icu_flag = 1 THEN 1
        ELSE 0
    END) AS ICU_admitted_patients,
 round(sum( CASE
        WHEN icu_flag = 1 THEN 1
        ELSE 0
    END)
 * 100.0 / count(patient_id),2) as ICU_admission_rate
 from emergency_department_dataset
 group by triage_acuity
 order by ICU_admission_rate DESC;
 -- Business Interpretain:
 -- within this dataset ICU admissions were limitted to patients classified as Resuscitation and Emergent
 -- indicating that only higher- acuity patients required critical care
 -- Emergent patients demonstrated a slightly higher IcU admission rate than Resuscitation patients
 -- However additional validation identified inconsistencies between icu admission and  hospital admission variables
 -- suggesting that these findings should be interpreted cautiously
 -- Recommendations:
 -- validate the relationship between ICU admission and hospital admission records before opertional reporting
 -- Investigate ICU patients by chief complaint and length of stay
 -- Review clinical documentation to ensure ICU status is accurately captured
 -- Continue monitoring ICU utilization among high-acuity patients for capacity planning
 -- Key findings:
 -- ICU admissions occured exclusively among Resuscitation and Emergent patients in this dataset
 -- supporting the relationship between higher clinical severity and critical care utilization
 -- However inconsistencies between ICU and hospital admission variables indicates that additional data validation
 -- is recomended before drawing opertional conclusions
 -- ---------------------------------------------------------------------------------------------------------------------------
 -- Question 7:
 -- Which chief complaints have the highest hospital admission rate?
 -- KPI:
 -- Hospital admission rate by Chief Complaint
 select chief_complaint,
 count(patient_id) as patient_volume,
 sum(case
 when admitted = 'Yes' then 1
 else 0
 end) as admitted_patients,
 round(sum(case
 when admitted = 'Yes' then 1
 else 0
 end)
 * 100.0 / count(patient_id),2) as admission_rate
 from emergency_department_dataset
 group by chief_complaint
 order by admission_rate DESC;
-- Business Interpretain
 --  Hospital admission rates decreased consistently regardless the patient volume
 -- Patients  with Stroke Symptoms and Headache and Chest pain had the highest likelihood of hospital admission
 -- reflecting the greater clinical severity and needs for inpatient management
 -- Incontrast fever and shortness of breath show less admission rate
 -- Recommendations
 -- Analyze the triage acuity among admitted patients to identify the  most strongly level of acuity  associated with hospitalization
 -- compare admission rates with ICU admission to understand wether the highest chhief complaint also require critical care
 -- evaluate length of stay for admitted patients across different cheif complaints
 -- key findings
 -- chief complaints demonestrated a strong relationship with hospital admission
 -- Patients with higher clinical severity were substantially more likely to require inpatient care stroke symptoms
 -- confirming that chief complaints is an important predictor of hospital resource utilization and admission planning
-- ---------------------------------------------------------------------------------------------------------------------------------------------
-- Question 8 :
--  Which chief complaints have the highest IcU admission rate
-- KPI:
-- ICU Admission Rate by Chief Complaint:
select chief_complaint,
 count(patient_id) as patient_volume,
 sum( CASE
        WHEN icu_flag = 1 THEN 1
        ELSE 0
    END) AS ICU_admitted_patients,
 round(sum( CASE
        WHEN icu_flag = 1 THEN 1
        ELSE 0
    END)
 * 100.0 / count(patient_id),2) as ICU_admission_rate
 from emergency_department_dataset
 group by chief_complaint
 order by ICU_admission_rate DESC;
 -- Business Interpretain:
 -- within this dataset ICU admissions rate is slightly differ across the chief complaints
 -- still stroke symptoms complaints are within the hghest rate among admitted and icu admission rate
 -- we can still need data validation before giving final opertional reports
 -- Recommendations:
 -- validate the relationship between ICU admission and hospital admission records before opertional reporting
 -- Investigate ICU patients by length of stay
 -- Review clinical documentation to ensure ICU status is accurately captured
 -- Continue monitoring ICU utilization among higher rate of chief complaints patients for capacity planning
 -- Key findings:
 -- ICU admissions rate are slightly varry on the base of chief complaints
 -- However  additional data validation  is recomended before drawing opertional conclusions
 -- ----------------------------------------------------------------------------------------------------
-- Question 9:
-- which triage acuity level has the highest 72- hour readmission rate?
-- KPI:
-- 72-Hour Readmission Rate by Triage Acuity
select triage_acuity,
 count(patient_id) as patient_volume,
 sum(case
 when readmit_72h_flag ='Yes' then 1
 else 0
 end) as readmitted_patients,
 round(sum(case
 when readmit_72h_flag =  'Yes' then 1
 else 0
 end)
 * 100.0 / count(patient_id),2) as readmission_rate
 from emergency_department_dataset
 group by triage_acuity
 order by readmission_rate DESC;
 -- Business Interpretain
 --  Hospital readmission rates decreasedregardless the triage acutiy levels
 -- Patients classified as Emergent had the highest likelihood of hospital re admission with 72 hours
 -- reflecting the greater clinical severity and needs for inpatient management
 -- Incontrast  non urgent patients were more less readmission rate
 -- Recommendations
 -- Analyze the chief complaints among admitted patients to identify the clinical conditions most strongly associated with 72_hour_readmission hospitalization
 -- compare admission rates with ICU admission to understand wether the highest triage acuity also require critical care
 -- evaluate length of stay for admitted patients across different triage levels
 -- key findings
 -- triage acuity demonestrated a slight  relationship with hospital 72_hour_readmission
 -- Patients with Emergent triage_acuity were substantially more likely to require readmission  inpatient care
 -- confirming that triage acuity is an important predictor of hospital resource utilization and re_admission planning
-- ---------------------------------------------------------------------------------------------------------------------------------------------
-- Question 10:
-- Which chief complaints have the highest 72-hour readmission rate?
-- KPI:
-- 72- Hour Readmission Rate by Chief Complaint:
select chief_complaint,
 count(patient_id) as patient_volume,
 sum(case
 when readmit_72h_flag ='Yes' then 1
 else 0
 end) as readmitted_patients,
 round(sum(case
 when readmit_72h_flag =  'Yes' then 1
 else 0
 end)
 * 100.0 / count(patient_id),2) as readmission_rate
 from emergency_department_dataset
 group by chief_complaint
 order by readmission_rate DESC;
-- Business Interpretation:
-- The readmission rate after 72 hour is varied only slightly across chief complaints
-- patients with shortness of breath has the highest readmission rate while patients with Stroke Symptoms has the lowest readmission rate
-- additional clinical and opertional factors are likely contributing to patients returning to the ED within 72 hours
-- Recommendations:
-- Review discharge planning and patient education for patients with higher readmission rates
-- compare readmission rates with Length of Stay and Icu admission to identify high-risk patient groups
--  Evaluate follow-up care and outpatient referral processes to reduce avoidable Emergency Department revisits
-- Key Findings:
-- patients with Shortness Of Breath demonstrated the highest 72_hour readmission rate within this datasets
-- However readmission rate varied only slightly across the chief complaints
-- suggesting that factors beyond initial clinical severity may contribute to ED revisits
-- ------------------------------------------------------------------------------------------------------------------------------
-- Section Four:
-- Financial Analysis
-- Question One:
-- What is the total estimated charge generated by Emergency Department encounter ?
-- KPI
-- Total Estimated ED Charges
show columns from emergency_department_dataset;
select
round(sum(estimated_charge_usd),2) as total_estimated_charges,
count(patient_id) as patient_volume,
round(avg(estimated_charge_usd),2) as avg_charges_per_patient
from emergency_department_dataset;
-- Business Interpretation:
-- The Emergency Department made about $78.68 million in estimated charges from 10,000 visits, averaging $7,867.71 per visit.
-- This sets a baseline to study financial differences across hospitals, payers, and patient needs.
-- These figures are estimated charges, not actual collected money or confirmed revenue.
-- Recommendation:
-- Hospital management should use the total estimated charges as a baseline and investigate how financial activity varies by:
-- Hospital,Payer type,Chief complaint,Triage acuity,Shift,Patient utilization patterns
-- Further analysis can help identify which patient groups and operational areas contribute most to the ED's estimated financial activity.
-- Key Finding:
-- - The ED generated approximately $78.68 million in estimated charges across 10,000 encounters,
-- with an average estimated charge of $7,867.71 per encounter.
-- this provides a baseline for evaluating financial performance and resource utilization across the Emergency Department.
-- ----------------------------------------------------------------------------------------------------------------------------------
-- Question Two:
-- Which hospital has the highest total estimated charges?
-- KPI
-- Total Estimated Charges by Hospital
select hospital_name,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by hospital_name
order by total_estimated_charges DESC;
-- Business Interpretation:
-- -- Central General generated the highest total estimated Emergency Department charges,
-- contributing approximately $22.31 million across 2,828 patient encounters.
-- The higher total estimated charges appear to be primarily driven by
-- higher patient volume rather than substantially higher charges per patient.
-- Riverside Medical reported a lower total estimated charge,
-- it recorded the highest average estimated charge per patient,
-- suggesting that patient encounters at Riverside Medical
-- may involve slightly greater resource utilization or higher-cost services.
-- Recommendation:
-- Compare total estimated charges with patient volume to distinguish whether financial differences are driven by patient demand or higher average charges.
-- Analyze estimated charges by chief complaint and triage acuity within each hospital to identify clinical drivers of financial activity.
-- Compare laboratory utilization, medication administration, and Length of Stay across hospitals to better understand differences in average estimated charges.
-- Continue monitoring estimated charges together with operational and clinical KPIs to support resource allocation and financial planning.
-- Compare total estimated charges with patient volume to distinguish whether financial differences are driven by patient demand or higher average charges.
-- Analyze estimated charges by chief complaint and triage acuity within each hospital to identify clinical drivers of financial activity.
-- Compare laboratory utilization, medication administration, and Length of Stay across hospitals to better understand differences in average estimated charges.
-- Continue monitoring estimated charges together with operational and clinical KPIs to support resource allocation and financial planning.
-- Key Findings:
-- Central General generated the highest total estimated Emergency Department charges,
--  largely due to its higher patient volume. Despite differences in total estimated charges,
--  the average estimated charge per patient remained relatively consistent across hospitals,
--  suggesting comparable financial patterns across the healthcare system.
-- -------------------------------------------------------------------------------------------------------------------------------------
-- Question 3:
-- Which Payer type contributes the highest total estimated charges?
-- KPI
-- Estimated Charges by Payer Type:
select
payer_type,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by payer_type
order by total_estimated_charges DESC;
--  -- Business Interpretation
-- Private payer patients generated the highest total estimated charges,
-- at approximately $33.15 million. This is primarily associated with their substantially higher patient volume of 4,179 encounters.
-- interestingly, Other payer types recorded the highest average estimated charge per patient
-- at approximately $8,071.95, despite representing the smallest patient volume.
-- Recommendations
-- Monitor payer mix and patient volume as part of Emergency Department financial planning.
-- Investigate why the Other payer category has the highest average estimated charge despite its low volume.
-- Compare payer types with Length of Stay, triage acuity, chief complaint, laboratory utilization, and medication administration to identify possible drivers of higher charges.
-- Further analyze payer-specific patterns to support revenue-cycle and resource-planning decisions.
-- Key Findings
-- Private payer encounters generated the highest total estimated charges, largely due to their higher patient volume.
-- However, the Other payer category recorded the highest average estimated charge per patient,
-- indicating that payer volume and individual encounter charges should be evaluated separately when assessing Emergency Department financial performance
-- -----------------------------------------------------------------------------------------------------------------------------------------------------------
-- Question 4:
-- Which Cheif complaint generate the highest total estimated charges?
-- KPI:
-- Estimated Charges by Cheif Complaint
select
chief_complaint,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by chief_complaint
order by total_estimated_charges DESC;
-- Business Interpretation
-- Trauma generated the highest total estimated charges,
-- approximately $12.58 million, primarily because it had the highest patient volume at 1,596 encounters.
-- Recommendations
-- Investigate resource utilization associated with high-cost clinical presentations,
-- particularly Stroke Symptoms and Laceration.
-- Compare estimated charges with:
-- Length of Stay,Service Time,Laboratory Orders
-- Medications Administered
-- ICU Admission
-- Hospital Admission
-- Key Findings
-- Trauma generated the highest total estimated charges at approximately $12.58 million,
-- largely due to its high patient volume.
-- However, Stroke Symptoms had the highest average estimated charge per encounter at $8,143.20,
-- suggesting potentially greater resource intensity per patient.
--  Further analysis is required to determine the clinical and operational factors contributing to these differences.
-- -----------------------------------------------------------------------------------------------------------------------
-- Question 5:
-- Which triage acuity level generates the highest total estimated charges?
-- KPI:
-- Estimated CHARGES BY Triage acuity
select
triage_acuity,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by triage_acuity
order by total_estimated_charges DESC;
-- Business Recommendation:
-- The relationship between triage acuity and estimated charges suggests that higher-acuity encounters may require greater resource utilization.
-- Further analysis of procedures, laboratory utilization, medications, ICU utilization,
-- and length of stay would be required to identify the specific drivers of higher charges.
-- Recommendations
-- Investigate high-acuity resource utilization
-- Monitor high-volume Urgent cases
-- Evaluate resource intensity
-- Support capacity planning
-- Key Findings
-- Urgent patients generated the highest total estimated charges
-- at approximately $35.26 million because of their high patient volume.
-- --------------------------------------------------------------------------------------------------------
-- Question 6:
-- Which Emergency Department shift generates the highest total estimated charges?
-- KPI:
-- Estimated CHARGES BY Shift
select
shift,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by shift
order by total_estimated_charges DESC;
--  Business Interpretation
--  This finding suggests that financial differences between shifts are driven primarily
-- by patient volume rather than significant variations in resource utilization per patient.
-- Recommendation
-- Continue monitoring patient volume across all shifts to support staffing and financial planning
-- Use patient volume as a key indicator when planning workforce allocation and operational resources.
-- Key Findings
-- The Day shift generated the highest total estimated charges because it managed the largest patient volume.
--   suggesting that patient demand, rather than differences in individual encounters, is the primary driver of financial activity.
-- ----------------------------------------------------------------------------------------------------------------------------------------
-- Question 7
-- How does laboratory utilization affect estimated charges in the Emergency Department?
-- KPI:
--  Laboratory utilization affect charges
show columns from emergency_department_dataset;
select
 labs_ordered,
 count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by labs_ordered
order by labs_ordered DESC;
-- Business Interpretation
-- Estimated charges generally increase as laboratory utilization increases.
-- Patients with no laboratory orders had an average estimated charge of approximately $2,894,
-- while patients with higher laboratory utilization recorded substantially higher average estimated charges
-- Recommendations
-- Investigate the relationship between laboratory utilization and triage acuity.
-- Compare laboratory utilization with Length of Stay and service time.
-- Evaluate whether high laboratory utilization is concentrated among admitted or ICU patients.
-- Key Findings
-- Higher laboratory utilization is associated with substantially higher average estimated charges in this dataset.
-- Patients with no laboratory orders had the lowest average estimated charges, while higher laboratory-utilization groups
-- generally demonstrated considerably higher charges.
--  Further clinical analysis is required to determine whether this relationship reflects increased resource utilization
-- ----------------------------------------------------------------------------------------------------------------------------------------------------
-- Question 8
-- How does Medication Utilization affect estimated charges in the Emergency Department?
-- KPI:
-- Medication utilization affects charges
select
    medications_administered,
    count(patient_id) as patient_volume,
    round(avg(estimated_charge_usd),2) as avg_estimated_charge,
    round(sum(estimated_charge_usd),2) as total_estimated_charge
from emergency_department_dataset
group by medications_administered
order by medications_administered;
-- Business Interpretation
-- Average estimated charges generally increase as the number of medications administered increases.
-- Patients who received no medications had an average estimated charge of approximately $3,834,
-- while patients receiving higher numbers of medications generally recorded substantially higher estimated charges
-- Recommendations
-- Analyze medication utilization by triage acuity to determine whether higher-acuity patients require more medications.
-- Compare medication utilization with hospital admission and ICU admission.
-- Examine the relationship between medication utilization and Length of Stay.
-- Key Findings
-- Higher medication utilization is generally associated with higher average estimated charges.
--  Patients receiving no medications had the lowest average estimated charges,
-- while patients receiving multiple medications demonstrated substantially higher charges.
-- This relationship may reflect increased clinical complexity and resource utilization among patients requiring more intensive treatment.
-- ----------------------------------------------------------------------------------------------------------------------------------------------------
-- Question 9:
-- How does Admission Status affect estimated charges in the Emergency Department?
-- KPI
-- Average Estimated Charge by Admission Status
select
    admitted,
    count(patient_id) as patient_volume,
    round(avg(estimated_charge_usd),2) as avg_estimated_charge,
    round(sum(estimated_charge_usd),2) as total_estimated_charge
from emergency_department_dataset
group by admitted
order by avg_estimated_charge DESC;
-- Business Interpretation
-- Patients who were admitted to the hospital had a higher average estimated charge per encounter,
--  approximately $10,289.76, compared with $7,435.93 among patients who were not admitted.
-- Recommendations
-- Compare admitted and non-admitted patients by triage acuity, Length of Stay, laboratory utilization, medication utilization, and ICU admission.
-- Identify the resource-utilization patterns associated with higher estimated charges among admitted patients.
-- Monitor high-volume non-admitted encounters because their large volume contributes substantially to overall ED financial activity.
-- Use admission status alongside clinical and operational KPIs when evaluating resource planning and financial performance.
-- Key Findings
-- Admitted patients demonstrated higher financial intensity,
--  with an average estimated charge of $10,289.76 compared with $7,435.93 for non-admitted patients.
-- However, non-admitted encounters generated the majority of total estimated charges because of their substantially higher patient volume.
-- ------------------------------------------------------------------------------------------------------------------------------------------------
-- Section 1- Patient_Flow_Analysis
-- ------------------------------------
use health_care_project;
-- Question 1:
-- How many Patients visited the Emergency Department?
-- KPI
-- Total ED visits
select count(patient_id) as total_ed_visits
 from emergency_department_dataset;
 -- Business Interpretation:
 -- Measures the Total workload of the emergency Department
 -- Serves as the baselines for all other KPIs
 -- Helps managment evaluate demand and compare performance across different periods
 -- ----------------------------------------------------------------------------------
 -- Question 2:
 -- What is the average waiting time?
 -- KPi
 -- Average Waiting Time
 select * from emergency_department_dataset;
 select  round(avg(wait_time_minutes),2) as avg_waiting_time
 from emergency_department_dataset;
 -- Business Interpretation
 -- Indicate opertional efficiency
 -- A high verage waiting time may reduce patient satisfaction and delay tretment
 -- can be compared against hospital targets or national benchmarks
 -- ---------------------------------------------------------------------------------
 -- Question 3 :
 -- What is the average Length Of Stay?
 -- kPI
 -- Average lOS
 select round(avg(length_of_stay_minutes),2) as Avg_length_of_stay
 from emergency_department_dataset;
 -- Business Interpretaion:
 -- Measures How long patients remain in the ED
 -- A prolonged LOS may indicate workflow bottlenecks,delayed discharges,or bed shortages
 -- --------------------------------------------------------------------------------------------
 -- Question 4:
 -- Which shift recevies the highest number of patients?
 -- KPI
 -- Patient Volume by Shift
 select shift, count(patient_id) as patient_volume
 from emergency_department_dataset
 group by shift
 order by patient_volume desc;
 -- Business Interpretation:
 -- Measures the volume of patient per each shift
 -- it may act as a base for further analysis on each shift needs to counter act the patient volume
 -- helps in undestanding the supply and demand on the basic of shift
 -- Recommendations:
 -- If One Shift Consistently experiences substantially higher patient volumes, hospital managment should evaluate whether staffing
 -- levels, room availabilty, and support services during that shift are sufficient to meet demand
 -- additional analysis should compare patient volume with waiting time and triage acuity before making staffing decisions.
 -- ----------------------------------------------------------------------------------------------------------------------------------
 -- Question 5:
 -- Which day of the week has the highest Emergency Department patient volume?
 -- KPI
 -- Patient Volume per Day
 select day_of_week , count(patient_id ) as patient_volume_per_day
 from emergency_department_dataset
 group by day_of_week
 order by patient_volume_per_day desc;
 -- Business Interpretation :
 -- Measures volume of patients per day
 -- serves as baseline for other KPIs releated to the days of the week
 -- Helps mangement to eavaluate demand and compare performance across differnt days
 -- Recommendations:
 -- if one day show higher volume of patient than other days hospital mangment should evaluate the staff avalabilty room occupancy and the medical services
 -- during this day to meet the demand also additional analysis may took place by comparing the waiting time and triage acuity before making staffing decisions
 --  ------------------------------------------------------------------------------------
 -- Question 6:
 -- At what hour of the day does the Emergency Department receive the highest number of patients?
 -- KPI
 -- Patient Volume by Hour
 select concat(LPAD(Hour(arrival_datetime),2,'0'),':00') as arrival_hour,
 count(patient_id) as Patient_volume
 from emergency_department_dataset
 Group by arrival_hour
 order by Patient_volume DESC;
 -- Business Interpretation:
 -- Patient arrivals are relatively evenly distributed throughout the day,
 -- with a slight increase during the late evening. Although 23:00 records the highest number of arrivals,
 -- the variation across hours is modest, suggesting that patient demand remains relatively stable throughout the day in this dataset.
 -- Recommendation:
 -- Since hourly variation is limited, staffing decisions should not rely solely on arrival volume.
 -- Additional analyses, including waiting time, triage acuity, and length of stay, should be evaluated to identify operational bottlenecks.
 -- ----------------------------------------------------------------------------------------------------------------------------------------------
 -- Question 7 :
 -- Which Shift has the highest average patient waiting time?
 -- KPI
 -- Average Waiting Time by Shift
 select shift,
 count(patient_id) as Patient_Volume
 ,Round(avg(wait_time_minutes),2) as Avg_waiting_time from emergency_department_dataset
 group by shift
 order by Avg_waiting_time DESC;
 -- Business Interpretation:
 -- The average waiting time is slightly differnt in the three shifts during the day
-- Although patient volume varies considerably across shifts,
-- average waiting times remain very similar. This suggests that factors other than patient volume
-- may have a greater influence on waiting times within this dataset.
 -- Recommendtions:
 -- "Based on this analysis alone, I wouldn't recommend additional staffing. Although the Day Shift has the highest patient volume,
 --  its average waiting time is actually the lowest of the three shifts. This suggests that current staffing may already be appropriate. Before making staffing decisions,
 --  I'd investigate triage acuity, diagnostic turnaround time, bed occupancy, and patient length of stay."
-- -------------------------------------------------------------------------------------------------------------
 -- Question 8 :
 -- Which triage acuity level has the highest average waiting time?
 -- KPI
 -- Average Waiting Time by Triage Acuity
 select triage_acuity,
 count(patient_id) as Patient_Volume
 ,Round(avg(wait_time_minutes),2) as Avg_waiting_time from emergency_department_dataset
 group by triage_acuity
 order by Avg_waiting_time DESC;
 -- Business Interpretion:
 -- This analysis shows how long each level is waiting in the ED
 -- this analysis will need more investigation on the bases of this result
 -- Recommendation :
 -- The analysis indicates that higher-acuity patients experience longer average waiting times than lower-acuity patients
 -- Because this differs from expected Emergency Department practice, the result should be validated by confirming the definition of the waiting time
 -- variable and assessing whether the dataset reflects real operational processes or educational sample data
 -- Key Finding:
-- Contrary to expected Emergency Department prioritization, higher-acuity patients in this dataset have longer average waiting times than lower-acuity patients.
-- This unexpected pattern requires validation before operational conclusions are made.
 -- -------------------------------------------------------------------------------------------------------------------------------------
 -- Question 9:
  -- What is the average Length of Stay (LOS) by Shift?
  -- KPI
  -- Average Length of Stay by Shift
   select shift,
 count(patient_id) as Patient_Volume
 ,Round(avg(length_of_stay_minutes),2) as Avg_LOS from emergency_department_dataset
 group by shift
 order by Avg_LOS DESC;
 -- Business Interpretaion:
 -- This may be due to Delays in patient Discharge
 -- or due to delays in Diagnostic Images
 -- staff handover delays
 -- Recommendations:
-- Average Length of Stay is remarkably consistent across all three shifts
--  suggesting that the overall patient journey is relatively stable throughout the day.
--  Based on this analysis alone, there is no strong evidence that one shift experiences substantially longer patient stays than the others.
-- Key Finding
-- Although patient volume varies across shifts, Average Length of Stay remains very similar.
--  Based on this analysis, shift alone does not appear to explain differences in patient LOS.
-- ------------------------------------------------------------------------------------------------------
--  Section Two :
--  Operational Analysis
-- -------------------------------------------------------------------------------------------
-- Question 10 :
-- Which age group has the highest Average Length of Stay(LOS)?
-- KPI
-- Average LOS by age group
select age_group ,count(patient_id),round(avg(length_of_stay_minutes),2) as Avg_lOS
 from emergency_department_dataset
 group by age_group
 order by  Avg_LOS DESC;
-- Business Interpretaion:
-- there are 4 age group in this data set
-- the biggest age group is the young adult (18-39)
-- the average LOS is slightly differ through out the whole ages groups
-- Recommendations:
-- Average Length of Stay is relatively consistent across all age groups.
-- Although Young Adults (18–39 years) have the highest average LOS,
-- the variation between age groups is small,
-- suggesting that age alone is unlikely to be a major driver of patient Length of Stay within this dataset.
-- key findings:
-- Although patient volume differs among age groups,
-- Average Length of Stay remains relatively consistent,
-- suggesting that age alone does not explain differences in patient LOS.
-- -------------------------------------------------------------------------------------------------------------
-- Question 11:
--  What are the most common chief complaints among Emergency Department patients?
-- KPI:
-- Patient Volume by Cheif Complaint
select chief_complaint, count(patient_id) as Patient_volume
 from emergency_department_dataset
 Group by chief_complaint
 order by Patient_volume DESC;
 -- Business Interpretation:
 -- according to this analysis Trauma complaints are the highest patient volume
 -- there should be more clinical and opertional analysis to make sure that medical supply and staff are capable to support this demand
 -- Recommendations:
 -- Trauma represents the highest-volume chief complaint in this Emergency Department dataset.
 -- This suggests that trauma-related cases consume a significant portion of ED resources
 -- and should be considered when planning staffing, equipment availability, and clinical workflows.
 -- key findings
 -- Trauma and Chest Pain account for the largest proportion of Emergency Department visits in this dataset,
 --  indicating that these conditions should be prioritized in future operational and clinical analyses
 -- ------------------------------------------------------------------------------------------------------------
 -- Question 12:
 -- Which chief complaints have the longest Average Length of Stay (LOS)?
 -- KPI:
 -- Avg LOS by chief complaints
 select chief_complaint,
 count(patient_id) as Patient_volume,
 Round(avg(length_of_stay_minutes),2) as Avg_LOS from emergency_department_dataset
 group by chief_complaint
order by Avg_LOS DESC ;
-- Business Interpretaion:
-- Patients presenting with Stroke Symptoms and Chest Pain have the longest average Length of Stay.
-- These conditions often require urgent assessment, advanced imaging,
-- specialist consultation, and possible admission,
-- which may contribute to longer stays in the Emergency Department.
-- Recommendations :
-- Further analysis should evaluate whether the prolonged LOS for Stroke Symptoms
-- and Chest Pain is associated with diagnostic imaging,
-- specialist consultations, admission delays,
-- or bed availability before implementing operational changes.
-- key findings:
-- Stroke Symptoms and Chest Pain have the longest average Length of Stay in this dataset,
-- suggesting these patient groups place a greater demand on Emergency Department
-- resources and should be prioritized for operational review.
-- -----------------------------------------------------------------------------------------------------
-- Observation:
-- "Chest Pain deserves immediate operational attention because it combines a high patient volume with one of the longest average Lengths of Stay.
--  This combination suggests a significant impact on Emergency Department capacity and resource utilization."
-- -----------------------------------------------------------------------------------------------------------------------------------------------------
-- Question 13:
-- which chief complaint have the longest average waiting time?
-- KPI
-- Avg waiting time by chief Complaint
select chief_complaint ,
count(patient_id) as Patient_volume,
 Round(avg(wait_time_minutes),2) as Avg_waiting_time
from emergency_department_dataset
group by chief_complaint
order by Avg_waiting_time DESC;
-- Busniess Interpretation:
-- Stroke Symptoms have the highest average waiting time,
-- which may contribute to their longer average Length of Stay.
-- Additional analyses are needed to determine whether imaging,
-- specialist consultation, or admission delays also influence LOS
-- Recommendation:
-- Future analyses should evaluate diagnostic turnaround time,
 -- bed occupancy, specialist consultation time,
-- and admission workflow to determine which factors contribute most to prolonged waiting time
-- and Length of Stay for patients presenting with Stroke Symptoms.
-- key findings:
-- Stroke Symptoms have the highest average waiting time in the dataset.
-- However, waiting times across chief complaints differ by less than four minutes
-- , suggesting that complaint type alone does not substantially influence waiting time.
-- ------------------------------------------------------------------------------------------------------------
-- show columns from emergency_department_dataset;
-- -------------------------------------------------------------------------------------------------------------
-- -- Question 14:
-- Which arrival mode has the highest average waiting time?
-- KPI
-- Avg waiting time by arrival mode
select arrival_mode ,
count(patient_id) as Patient_Volume,
 round(avg(wait_time_minutes),2) as avg_waiting_time
from emergency_department_dataset
group by arrival_mode
order by avg_waiting_time DESC;
-- Business Interpretion:
-- Average waiting time is relatively similar across all arrival modes.
 -- Ambulance arrivals have the shortest average waiting time,
-- which is consistent with Emergency Department prioritization of potentially higher-acuity patients.
-- Recommendation:
-- Although waiting times differ only slightly across arrival modes,
 -- future analyses should evaluate whether triage acuity, patient volume
-- and arrival mode together influence Emergency Department
 -- waiting time before operational changes are considered.
-- Key findings:
-- Waiting times are relatively consistent across all arrival modes,
-- with Ambulance arrivals experiencing the shortest average waiting time and Referral patients the longest.
-- However, the overall variation is small (approximately two minutes).
-- -----------------------------------------------------------------------------------------------------------------
-- Question 15:
-- Which hospital has the highest average waiting time?
-- KPI
-- Avg waiting time by Hospital_name
show columns from emergency_department_dataset;
select hospital_name,
 count(patient_id) as patient_volume,
 round(avg(wait_time_minutes),2) as Avg_waiting_time
 from emergency_department_dataset
 group by hospital_name
 order by Avg_waiting_time DESC;
 -- -- Business Interpretion:
-- Average waiting time is relatively similar across all hospitals.
 -- St.Mary ED have the shortest average waiting time,
-- Recommendation:
-- Although waiting times differ only slightly across hospitals ,
 -- future analyses should evaluate whether triage acuity,
-- and arrival mode together influence Emergency Department
 -- waiting time before operational changes are considered.
 -- -- Key findings:
-- Waiting times are relatively consistent across all hospitals,
-- with St.Mary ED experiencing the shortest average waiting time and Metro Health  the longest.
-- However, the overall variation is small (approximately two minutes).
-- ------------------------------------------------------------------------------------------------------
-- Question 16:
-- what is the Average Boarding Time by Shift
-- KPI:
-- Average Boarding Time by Shift
select shift, round(avg(boarding_time_minutes),2) as Avg_boarding_time,
count(patient_id) as patient_volume
from emergency_department_dataset
group by shift
order by Avg_boarding_time DESC;
-- -- Business Interpretion:
-- Average boarding time is relatively similar across all the day shifts.
 -- Night shift have the shortest average boarding time,
 -- Recommendation:
-- Although boarding times differ only slightly across shifts ,
 -- future analyses should evaluate whether triage acuity,
-- and arrival mode, bed availabilty  together influence Emergency Department
 -- boarding time before operational changes are considered.
 -- -- Key findings:
-- boarding times are relatively consistent across all shifts,
-- with night shift the shortest average waiting time and Day shift the longest.
-- However, the overall variation is small (approximately one minute).
-- ------------------------------------------------------------------------------------------------------
-- Question 17:
-- Which hospital has the longest average boarding time?
-- kPI
-- Avg boarding time by Hospital
select hospital_name,
round(avg(boarding_time_minutes),2) as Avg_boarding_time,
count(patient_id) as patient_volume
from emergency_department_dataset
group by hospital_name
order by Avg_boarding_time DESC;
-- Business interpretation:
-- Average boarding time is relatively consistent across all hospitals,
--  suggesting that inpatient bed availability and admission processes remain stable throughout the hospitals
--  Although the St.Mary ED has the longest average boarding time, the variation between hospitals
-- is small and may not indicate a meaningful operational difference.
-- Recommendations:
-- Future analyses should evaluate inpatient bed availability,
--  admission workflow efficiency, discharge planning,
-- and hospital occupancy to determine whether these factors contribute
--  to boarding delays before operational changes are implemented
-- key findings:
-- Boarding time varies by approximately one minute across the hospitals
--  suggesting that hospitals boarding timing alone is unlikely to be a major contributor to Emergency Department boarding delays.
-- ---------------------------------------------------------------------------------------------------------------------------------
-- Question 18:
-- Which shift has the longest average service time?
-- KPI:
-- Avg service time by shift
show columns from emergency_department_dataset;
select shift, round(avg(service_time_minutes),2) as Avg_service_time,
count(patient_id) as patient_volume
from emergency_department_dataset
group by shift
order by Avg_service_time DESC;
-- Business Interpretation
-- Consistent clinical practices.
-- Potential night constraints:
-- Fixed process bottlenecks
-- Recommendations:
-- Audit night diagnostics: Investigate if overnight delays in lab results or radiology imaging .
-- Examine patient acuity: Cross-reference this data with a triage scale (like the Emergency Severity Index) to check if night patients are simply sicker.
-- Review discharge barriers: Pinpoint why evening shifts handle high volumes faster, and apply their coordination tactics to day and night crews.
-- Analyze waiting times: Focus process improvements on waiting room queues, as the actual hands-on care time is already highly optimized and stable.
-- Key findings:
-- Highly uniform process
-- Night shift highest
-- Evening shift lowest
-- Minor operational variance
-- ------------------------------------------------------------------------------------------------------------------------------
-- Question 19
-- Which chief complaints have the longest average service time?
-- KPI:
-- Avg Service time by Chief complaints
select chief_complaint, round(avg(service_time_minutes),2) as Avg_service_time,
count(patient_id) as patient_volume
from emergency_department_dataset
group by chief_complaint
order by Avg_service_time DESC;
-- Business Interoretaion:
-- Stroke Symptoms,Chest Pain,Shortness of Breath have the longest average service time
-- which may be due to resource allocation, triage severity,process bottlencks
-- Recommendations:
-- further analysis may took place to work more on the causes that may effect the longest service time like labrortray utilization
-- Medication Administration ,triage acutiy, admission status to determine which factors contribute most prolonged service time for the chief complaints
-- key findings:
-- variation in Service time across the chief complaints
-- stroke symptoms is the longest service time chief complaints
-- further analysis should tok place regarding resource allocation and labrotary utilization
-- --------------------------------------------------------------------------------------------------
-- question 20
-- which region Type has the highest Emergency Department patient volume?
-- KPI
-- Patient Volume by Region Type
select region_type , count(patient_id) as Patient_volume
from emergency_department_dataset
group by region_type
order by Patient_volume desc;
-- Business Interpretion:
 -- Measures the volume of patient per each region
 -- it may act as a base for further analysis on each region needs to counter act the patient volume
 -- helps in undestanding the supply and demand on the basic of region
 -- Recommendations:
 -- If One region Consistently experiences substantially higher patient volumes, hospital managment should evaluate whether staffing
 -- levels, room availabilty, and support services during that shift are sufficient to meet demand
 -- additional analysis should compare patient volume with waiting time and triage acuity before making staffing decisions.
 -- Key Findings:
 -- Urban region shows the highest patient_volume comparing to other regions
 -- ----------------------------------------------------------------------------------------------------------------------------------
 -- Question 21 :
 -- How does Emergency Department performance differ between weekdays and weekends?
 -- KPI:
 -- Emergency Department Performance
 show columns from emergency_department_dataset;
 select  is_weekend,
 count(patient_id) as Patient_volume,
  round(avg(wait_time_minutes),2) as Avg_waiting_time,
 round(avg(length_of_stay_minutes),2) as Avg_length_of_stay
 from emergency_department_dataset
 group by is_weekend
 order by is_weekend DESC;
 -- Business Interpretation:
 --  Although weekdays experience substantially higher patient volumes than weekends,
 -- average waiting time and Length of Stay remain remarkably consistent
--  This suggests that Emergency Department operations
 -- maintain similar performance despite increased weekday demand.
 -- Recommendations:
 -- further analysis on Capacity planning and staffing Bottlenecks
-- also analyse Revenue and resource utilization, patient experience and quality of care
-- optimize weekday high volume flow and stabilize weekend opertional blocks
-- key findings:
-- Weekdays receive significantly more Emergency Department visits than weekends;
--  however, operational performance remains stable,
--  as reflected by nearly identical average waiting times and Lengths of Stay.
-- --------------------------------------------------------------------------------------------------
-- Question 22:
-- How do Emergency Department patient volume, average waiting time, and average Length of Stay change throughout the year?
-- KPI:
-- Monthly Trend Analysis
select month,count(patient_id) as patient_volume,
round(avg(wait_time_minutes),2) as Avg_waiting_time,
 round(avg(length_of_stay_minutes),2) as Avg_length_of_stay
 from emergency_department_dataset
 group by month
 order by month ;
 -- Business Interpretation :
  -- Although months experience substantially varrying in  patient volume ,
 -- average waiting time and Length of Stay remain remarkably consistent
--  This suggests that Emergency Department operations
 -- maintain similar performance despite this volume variation demand.
 -- Recommendations:
 --  further analysis on Capacity planning and staffing Bottlenecks
-- also analyse Revenue and resource utilization, patient experience and quality of care
-- key findings:
--  throught out the year the ED show slight variation on average waiting time and average length of stay
-- patient volume has slight or no effects on the average waiting time and length of stay
-- --------------------------------------------------------------------------------------------------------------
 -- Section Three:
 -- Clinical Analysis:
 -- Section 1- Patient Outcomes
 -- -------------------------------------------------
 -- Question1 :
 -- What percentage of Emergency Department patients are admitted to the hospital?
 -- KPI:
 -- Admisson Rate
 select admitted , count(patient_id) as patient_volume,
 round(count(patient_id) * 100.0 /
 (select count(*) from emergency_department_dataset),2) as admission_rate
 from emergency_department_dataset
 group by admitted;
 -- Business Interpertation:
 --  Approximately 15% of Emergency Department patients required hospital admission, while the majority were treated and discharged
 -- This suggests that most ED visits were managed without requiring inpatient care,although admitted patients are likely to represent
 -- more clinically complex cases requiring additional hospital resourses
 -- Recommendations
 -- Analyze admission rates by triage acuity.
-- Compare admission rates across chief complaints.
-- Evaluate ICU admissions among admitted patients.
-- Assess Length of Stay for admitted versus discharged patients.
-- Review resource utilization for admitted patients.
-- key findings:
-- Approximately one out of every six Emergency Department patients required hospital admission,
-- while the majority were safely discharged.
-- Admission status should be further analyzed alongside triage acuity,
-- ICU utilization, and chief complaints to better understand clinical complexity and resource requirements.
-- --------------------------------------------------------------------------------------------------------------------
-- Question 2 :
-- What is the distribution of Emergency Department patient dispostions?
-- KPI:
-- Patient Disposition Distribution
select disposition,
count(patient_id) as patient_volume
from emergency_department_dataset
group by disposition
order by patient_volume DESC;
-- Business interpretation:
-- -Most Emergency Department patients were discharged after receiving treatment,
--  indicating that the majority of cases were managed without requiring inpatient admission.
--  However, the presence of admitted, transferred,
-- left-without-being-seen, and expired patients highlights
-- different clinical pathways that require
--  further investigation to evaluate patient outcomes, resource utilization, and quality of care.
-- Recommendation:
--   Analyze admitted  by triage acuity and most often chief complaints
-- Review resource utilization for admitted patients.
-- try to analyse the main clinical causes for the dispostion categories regarding the patient volume
-- Key findings:
-- Discharged patients accounted for the majority of Emergency Department visits,
-- while smaller but clinically important groups—including admitted,
-- transferred, left-without-being-seen,
--  and expired patients—represent key areas for further clinical and operational investigation.
-- -----------------------------------------------------------------------------------------------------------------------------
-- Question 3:
-- What percentage of Emergency Department patients required ICU admission?
-- KPI:
--  ICU Admission Rate
SELECT
    CASE
        WHEN icu_flag = 1 THEN 'ICU Admission'
        ELSE 'No ICU Admission'
    END AS ICU_Status,
    COUNT(patient_id) AS patient_volume,
    ROUND(
        COUNT(patient_id) * 100.0 /
        (SELECT COUNT(*) FROM emergency_department_dataset),2
    ) AS icu_admission_rate
FROM emergency_department_dataset
GROUP BY icu_flag;
-- Business interpretation:
-- Only 0.59% of Emergency Department patients required Intensive Care Unit (ICU) admission,
-- indicating that the vast majority of patients were managed without requiring critical care. Although
-- ICU admissions represent a very small proportion of total visits,
-- they are among the most clinically complex patients and require
-- significant hospital resources, specialized staff, and continuous monitoring.
-- Recommondetion;
-- Analyze ICU admissions by triage acuity.
-- Identify the chief complaints most frequently associated with ICU admission.
-- Compare ICU admission rates across age groups.
-- Evaluate the Length of Stay of ICU patients.
-- Review resource utilization, including laboratory tests and medications, among ICU patients.
-- Key Findings
-- ICU admissions accounted for only 0.59% of Emergency Department visits, indicating that critical care cases were relatively uncommon.
-- Despite their low frequency,
-- these patients represent the highest level of clinical severity
-- and should be prioritized for further analysis to understand resource utilization and patient outcomes.
-- ------------------------------------------------------------------------------------------------------------------------------
-- Question 4 :
-- How many patients left the Emergency Department without being seen (LWBS)?
-- KPI:
-- Left without being seen(LWBS)Rate(%)
select disposition, count(patient_id) as patient_volume,
 round(count(patient_id) * 100.0 /
 (select  count(*) from emergency_department_dataset),2) as rate_LWBS
 from emergency_department_dataset
 where disposition = 'Left Without Being Seen';
-- Business Interprtion:
-- Approximately 5.65% of Emergency Department patients left without being seen before receiving medical assessment.
-- Although the majority of patients completed their care,
-- this proportion is clinically and operationally significant
-- because patients who leave without evaluation may experience delayed diagnosis,
-- worsening medical conditions, reduced patient satisfaction, and missed treatment opportunities.
-- This KPI also represents a potential loss of hospital revenue and should be monitored as an important quality indicator.
-- Recommendation:
-- Investigate the average waiting time contrubute with LWBS rate
-- Analyze LWBS cases by shift, hour of arrival, and day of the week to identify periods with the highest risk.
-- Compare LWBS patients by triage acuity to determine whether lower-acuity patients are more likely to leave before evaluation.
-- Review chief complaints among LWBS patients to identify whether specific clinical presentations are associated with a higher likelihood of leaving.
-- Key findings:
-- Approximately one out of every eighteen Emergency Department patients left without being seen,
-- making LWBS an important patient safety and operational quality indicator.
--  Further investigation is required to determine whether waiting times, patient flow, or clinical characteristics contribute to these departures.
-- ---------------------------------------------------------------------------------------------------
-- Data Validation Note:
-- During analysis, a discrepancy was identified between the disposition and left_without_being_seen fields.
-- While only 11 patients were flagged as left_without_being_seen = Yes, a total of 565 patients had a disposition of "Left Without Being Seen."
 -- Because the disposition field provided a complete classification of patient outcomes,
-- it was used as the primary source for this analysis.
-- This inconsistency should be reviewed as a potential data quality issue before operational reporting.
-- -----------------------------------------------------------------------------------------------------------------------------------------
-- Question 5:
-- Which triage acuity level has the highest hospital admission rate?
-- KPI:
-- Hospital Admission Rate by Triage Acuity:
select triage_acuity,
 count(patient_id) as patient_volume,
 sum(case
 when admitted = 'Yes' then 1
 else 0
 end) as admitted_patients,
 round(sum(case
 when admitted = 'Yes' then 1
 else 0
 end)
 * 100.0 / count(patient_id),2) as admission_rate
 from emergency_department_dataset
 group by triage_acuity
 order by admission_rate DESC;
 -- Business Interpretain
 --  Hospital admission rates decreased consistently as triage acuity decreased
 -- Patients classified as Resuscitation and Emergent had the highest likelihood of hospital admission
 -- reflecting the greater clinical severity and needs for inpatient management
 -- Incontrast less Urgent and non urgent patients were more frequently treated and discharged from the ED without requiring hospitalization
 -- Recommendations
 -- Analyze the chief complaints among admitted patients to identify the clinical conditions most strongly associated with hospitalization
 -- compare admission rates with ICU admission to understand wether the highest triage acuity also require critical care
 -- evaluate length of stay for admitted patients across different triage levels
 -- key findings
 -- triage acuity demonestrated a strong relationship with hospital admission
 -- Patients with higher clinical severity were substantially more likely to require inpatient care
 -- confirming that triage acuity is an important predictor of hospital resource utilization and admission planning
-- ---------------------------------------------------------------------------------------------------------------------------------------------
-- Question 6:
-- which triage acuity level has the highest ICU admission rate?
-- KPI
-- Hospital  ICU Admission Rate by Triage Acuity:
select triage_acuity,
 count(patient_id) as patient_volume,
 sum( CASE
        WHEN icu_flag = 1 THEN 1
        ELSE 0
    END) AS ICU_admitted_patients,
 round(sum( CASE
        WHEN icu_flag = 1 THEN 1
        ELSE 0
    END)
 * 100.0 / count(patient_id),2) as ICU_admission_rate
 from emergency_department_dataset
 group by triage_acuity
 order by ICU_admission_rate DESC;
 -- Business Interpretain:
 -- within this dataset ICU admissions were limitted to patients classified as Resuscitation and Emergent
 -- indicating that only higher- acuity patients required critical care
 -- Emergent patients demonstrated a slightly higher IcU admission rate than Resuscitation patients
 -- However additional validation identified inconsistencies between icu admission and  hospital admission variables
 -- suggesting that these findings should be interpreted cautiously
 -- Recommendations:
 -- validate the relationship between ICU admission and hospital admission records before opertional reporting
 -- Investigate ICU patients by chief complaint and length of stay
 -- Review clinical documentation to ensure ICU status is accurately captured
 -- Continue monitoring ICU utilization among high-acuity patients for capacity planning
 -- Key findings:
 -- ICU admissions occured exclusively among Resuscitation and Emergent patients in this dataset
 -- supporting the relationship between higher clinical severity and critical care utilization
 -- However inconsistencies between ICU and hospital admission variables indicates that additional data validation
 -- is recomended before drawing opertional conclusions
 -- ---------------------------------------------------------------------------------------------------------------------------
 -- Question 7:
 -- Which chief complaints have the highest hospital admission rate?
 -- KPI:
 -- Hospital admission rate by Chief Complaint
 select chief_complaint,
 count(patient_id) as patient_volume,
 sum(case
 when admitted = 'Yes' then 1
 else 0
 end) as admitted_patients,
 round(sum(case
 when admitted = 'Yes' then 1
 else 0
 end)
 * 100.0 / count(patient_id),2) as admission_rate
 from emergency_department_dataset
 group by chief_complaint
 order by admission_rate DESC;
-- Business Interpretain
 --  Hospital admission rates decreased consistently regardless the patient volume
 -- Patients  with Stroke Symptoms and Headache and Chest pain had the highest likelihood of hospital admission
 -- reflecting the greater clinical severity and needs for inpatient management
 -- Incontrast fever and shortness of breath show less admission rate
 -- Recommendations
 -- Analyze the triage acuity among admitted patients to identify the  most strongly level of acuity  associated with hospitalization
 -- compare admission rates with ICU admission to understand wether the highest chhief complaint also require critical care
 -- evaluate length of stay for admitted patients across different cheif complaints
 -- key findings
 -- chief complaints demonestrated a strong relationship with hospital admission
 -- Patients with higher clinical severity were substantially more likely to require inpatient care stroke symptoms
 -- confirming that chief complaints is an important predictor of hospital resource utilization and admission planning
-- ---------------------------------------------------------------------------------------------------------------------------------------------
-- Question 8 :
--  Which chief complaints have the highest IcU admission rate
-- KPI:
-- ICU Admission Rate by Chief Complaint:
select chief_complaint,
 count(patient_id) as patient_volume,
 sum( CASE
        WHEN icu_flag = 1 THEN 1
        ELSE 0
    END) AS ICU_admitted_patients,
 round(sum( CASE
        WHEN icu_flag = 1 THEN 1
        ELSE 0
    END)
 * 100.0 / count(patient_id),2) as ICU_admission_rate
 from emergency_department_dataset
 group by chief_complaint
 order by ICU_admission_rate DESC;
 -- Business Interpretain:
 -- within this dataset ICU admissions rate is slightly differ across the chief complaints
 -- still stroke symptoms complaints are within the hghest rate among admitted and icu admission rate
 -- we can still need data validation before giving final opertional reports
 -- Recommendations:
 -- validate the relationship between ICU admission and hospital admission records before opertional reporting
 -- Investigate ICU patients by length of stay
 -- Review clinical documentation to ensure ICU status is accurately captured
 -- Continue monitoring ICU utilization among higher rate of chief complaints patients for capacity planning
 -- Key findings:
 -- ICU admissions rate are slightly varry on the base of chief complaints
 -- However  additional data validation  is recomended before drawing opertional conclusions
 -- ----------------------------------------------------------------------------------------------------
-- Question 9:
-- which triage acuity level has the highest 72- hour readmission rate?
-- KPI:
-- 72-Hour Readmission Rate by Triage Acuity
select triage_acuity,
 count(patient_id) as patient_volume,
 sum(case
 when readmit_72h_flag ='Yes' then 1
 else 0
 end) as readmitted_patients,
 round(sum(case
 when readmit_72h_flag =  'Yes' then 1
 else 0
 end)
 * 100.0 / count(patient_id),2) as readmission_rate
 from emergency_department_dataset
 group by triage_acuity
 order by readmission_rate DESC;
 -- Business Interpretain
 --  Hospital readmission rates decreasedregardless the triage acutiy levels
 -- Patients classified as Emergent had the highest likelihood of hospital re admission with 72 hours
 -- reflecting the greater clinical severity and needs for inpatient management
 -- Incontrast  non urgent patients were more less readmission rate
 -- Recommendations
 -- Analyze the chief complaints among admitted patients to identify the clinical conditions most strongly associated with 72_hour_readmission hospitalization
 -- compare admission rates with ICU admission to understand wether the highest triage acuity also require critical care
 -- evaluate length of stay for admitted patients across different triage levels
 -- key findings
 -- triage acuity demonestrated a slight  relationship with hospital 72_hour_readmission
 -- Patients with Emergent triage_acuity were substantially more likely to require readmission  inpatient care
 -- confirming that triage acuity is an important predictor of hospital resource utilization and re_admission planning
-- ---------------------------------------------------------------------------------------------------------------------------------------------
-- Question 10:
-- Which chief complaints have the highest 72-hour readmission rate?
-- KPI:
-- 72- Hour Readmission Rate by Chief Complaint:
select chief_complaint,
 count(patient_id) as patient_volume,
 sum(case
 when readmit_72h_flag ='Yes' then 1
 else 0
 end) as readmitted_patients,
 round(sum(case
 when readmit_72h_flag =  'Yes' then 1
 else 0
 end)
 * 100.0 / count(patient_id),2) as readmission_rate
 from emergency_department_dataset
 group by chief_complaint
 order by readmission_rate DESC;
-- Business Interpretation:
-- The readmission rate after 72 hour is varied only slightly across chief complaints
-- patients with shortness of breath has the highest readmission rate while patients with Stroke Symptoms has the lowest readmission rate
-- additional clinical and opertional factors are likely contributing to patients returning to the ED within 72 hours
-- Recommendations:
-- Review discharge planning and patient education for patients with higher readmission rates
-- compare readmission rates with Length of Stay and Icu admission to identify high-risk patient groups
--  Evaluate follow-up care and outpatient referral processes to reduce avoidable Emergency Department revisits
-- Key Findings:
-- patients with Shortness Of Breath demonstrated the highest 72_hour readmission rate within this datasets
-- However readmission rate varied only slightly across the chief complaints
-- suggesting that factors beyond initial clinical severity may contribute to ED revisits
-- ------------------------------------------------------------------------------------------------------------------------------
-- Section Four:
-- Financial Analysis
-- Question One:
-- What is the total estimated charge generated by Emergency Department encounter ?
-- KPI
-- Total Estimated ED Charges
show columns from emergency_department_dataset;
select
round(sum(estimated_charge_usd),2) as total_estimated_charges,
count(patient_id) as patient_volume,
round(avg(estimated_charge_usd),2) as avg_charges_per_patient
from emergency_department_dataset;
-- Business Interpretation:
-- The Emergency Department made about $78.68 million in estimated charges from 10,000 visits, averaging $7,867.71 per visit.
-- This sets a baseline to study financial differences across hospitals, payers, and patient needs.
-- These figures are estimated charges, not actual collected money or confirmed revenue.
-- Recommendation:
-- Hospital management should use the total estimated charges as a baseline and investigate how financial activity varies by:
-- Hospital,Payer type,Chief complaint,Triage acuity,Shift,Patient utilization patterns
-- Further analysis can help identify which patient groups and operational areas contribute most to the ED's estimated financial activity.
-- Key Finding:
-- - The ED generated approximately $78.68 million in estimated charges across 10,000 encounters,
-- with an average estimated charge of $7,867.71 per encounter.
-- this provides a baseline for evaluating financial performance and resource utilization across the Emergency Department.
-- ----------------------------------------------------------------------------------------------------------------------------------
-- Question Two:
-- Which hospital has the highest total estimated charges?
-- KPI
-- Total Estimated Charges by Hospital
select hospital_name,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by hospital_name
order by total_estimated_charges DESC;
-- Business Interpretation:
-- -- Central General generated the highest total estimated Emergency Department charges,
-- contributing approximately $22.31 million across 2,828 patient encounters.
-- The higher total estimated charges appear to be primarily driven by
-- higher patient volume rather than substantially higher charges per patient.
-- Riverside Medical reported a lower total estimated charge,
-- it recorded the highest average estimated charge per patient,
-- suggesting that patient encounters at Riverside Medical
-- may involve slightly greater resource utilization or higher-cost services.
-- Recommendation:
-- Compare total estimated charges with patient volume to distinguish whether financial differences are driven by patient demand or higher average charges.
-- Analyze estimated charges by chief complaint and triage acuity within each hospital to identify clinical drivers of financial activity.
-- Compare laboratory utilization, medication administration, and Length of Stay across hospitals to better understand differences in average estimated charges.
-- Continue monitoring estimated charges together with operational and clinical KPIs to support resource allocation and financial planning.
-- Compare total estimated charges with patient volume to distinguish whether financial differences are driven by patient demand or higher average charges.
-- Analyze estimated charges by chief complaint and triage acuity within each hospital to identify clinical drivers of financial activity.
-- Compare laboratory utilization, medication administration, and Length of Stay across hospitals to better understand differences in average estimated charges.
-- Continue monitoring estimated charges together with operational and clinical KPIs to support resource allocation and financial planning.
-- Key Findings:
-- Central General generated the highest total estimated Emergency Department charges,
--  largely due to its higher patient volume. Despite differences in total estimated charges,
--  the average estimated charge per patient remained relatively consistent across hospitals,
--  suggesting comparable financial patterns across the healthcare system.
-- -------------------------------------------------------------------------------------------------------------------------------------
-- Question 3:
-- Which Payer type contributes the highest total estimated charges?
-- KPI
-- Estimated Charges by Payer Type:
select
payer_type,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by payer_type
order by total_estimated_charges DESC;
--  -- Business Interpretation
-- Private payer patients generated the highest total estimated charges,
-- at approximately $33.15 million. This is primarily associated with their substantially higher patient volume of 4,179 encounters.
-- interestingly, Other payer types recorded the highest average estimated charge per patient
-- at approximately $8,071.95, despite representing the smallest patient volume.
-- Recommendations
-- Monitor payer mix and patient volume as part of Emergency Department financial planning.
-- Investigate why the Other payer category has the highest average estimated charge despite its low volume.
-- Compare payer types with Length of Stay, triage acuity, chief complaint, laboratory utilization, and medication administration to identify possible drivers of higher charges.
-- Further analyze payer-specific patterns to support revenue-cycle and resource-planning decisions.
-- Key Findings
-- Private payer encounters generated the highest total estimated charges, largely due to their higher patient volume.
-- However, the Other payer category recorded the highest average estimated charge per patient,
-- indicating that payer volume and individual encounter charges should be evaluated separately when assessing Emergency Department financial performance
-- -----------------------------------------------------------------------------------------------------------------------------------------------------------
-- Question 4:
-- Which Cheif complaint generate the highest total estimated charges?
-- KPI:
-- Estimated Charges by Cheif Complaint
select
chief_complaint,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by chief_complaint
order by total_estimated_charges DESC;
-- Business Interpretation
-- Trauma generated the highest total estimated charges,
-- approximately $12.58 million, primarily because it had the highest patient volume at 1,596 encounters.
-- Recommendations
-- Investigate resource utilization associated with high-cost clinical presentations,
-- particularly Stroke Symptoms and Laceration.
-- Compare estimated charges with:
-- Length of Stay,Service Time,Laboratory Orders
-- Medications Administered
-- ICU Admission
-- Hospital Admission
-- Key Findings
-- Trauma generated the highest total estimated charges at approximately $12.58 million,
-- largely due to its high patient volume.
-- However, Stroke Symptoms had the highest average estimated charge per encounter at $8,143.20,
-- suggesting potentially greater resource intensity per patient.
--  Further analysis is required to determine the clinical and operational factors contributing to these differences.
-- -----------------------------------------------------------------------------------------------------------------------
-- Question 5:
-- Which triage acuity level generates the highest total estimated charges?
-- KPI:
-- Estimated CHARGES BY Triage acuity
select
triage_acuity,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by triage_acuity
order by total_estimated_charges DESC;
-- Business Recommendation:
-- The relationship between triage acuity and estimated charges suggests that higher-acuity encounters may require greater resource utilization.
-- Further analysis of procedures, laboratory utilization, medications, ICU utilization,
-- and length of stay would be required to identify the specific drivers of higher charges.
-- Recommendations
-- Investigate high-acuity resource utilization
-- Monitor high-volume Urgent cases
-- Evaluate resource intensity
-- Support capacity planning
-- Key Findings
-- Urgent patients generated the highest total estimated charges
-- at approximately $35.26 million because of their high patient volume.
-- --------------------------------------------------------------------------------------------------------
-- Question 6:
-- Which Emergency Department shift generates the highest total estimated charges?
-- KPI:
-- Estimated CHARGES BY Shift
select
shift,
count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by shift
order by total_estimated_charges DESC;
--  Business Interpretation
--  This finding suggests that financial differences between shifts are driven primarily
-- by patient volume rather than significant variations in resource utilization per patient.
-- Recommendation
-- Continue monitoring patient volume across all shifts to support staffing and financial planning
-- Use patient volume as a key indicator when planning workforce allocation and operational resources.
-- Key Findings
-- The Day shift generated the highest total estimated charges because it managed the largest patient volume.
--   suggesting that patient demand, rather than differences in individual encounters, is the primary driver of financial activity.
-- ----------------------------------------------------------------------------------------------------------------------------------------
-- Question 7
-- How does laboratory utilization affect estimated charges in the Emergency Department?
-- KPI:
--  Laboratory utilization affect charges
show columns from emergency_department_dataset;
select
 labs_ordered,
 count(patient_id) as patient_volume,
round(sum(estimated_charge_usd),2) as total_estimated_charges,
round(avg(estimated_charge_usd),2) as avg_charge_per_patient
from emergency_department_dataset
group by labs_ordered
order by labs_ordered DESC;
-- Business Interpretation
-- Estimated charges generally increase as laboratory utilization increases.
-- Patients with no laboratory orders had an average estimated charge of approximately $2,894,
-- while patients with higher laboratory utilization recorded substantially higher average estimated charges
-- Recommendations
-- Investigate the relationship between laboratory utilization and triage acuity.
-- Compare laboratory utilization with Length of Stay and service time.
-- Evaluate whether high laboratory utilization is concentrated among admitted or ICU patients.
-- Key Findings
-- Higher laboratory utilization is associated with substantially higher average estimated charges in this dataset.
-- Patients with no laboratory orders had the lowest average estimated charges, while higher laboratory-utilization groups
-- generally demonstrated considerably higher charges.
--  Further clinical analysis is required to determine whether this relationship reflects increased resource utilization
-- ----------------------------------------------------------------------------------------------------------------------------------------------------
-- Question 8
-- How does Medication Utilization affect estimated charges in the Emergency Department?
-- KPI:
-- Medication utilization affects charges
select
    medications_administered,
    count(patient_id) as patient_volume,
    round(avg(estimated_charge_usd),2) as avg_estimated_charge,
    round(sum(estimated_charge_usd),2) as total_estimated_charge
from emergency_department_dataset
group by medications_administered
order by medications_administered;
-- Business Interpretation
-- Average estimated charges generally increase as the number of medications administered increases.
-- Patients who received no medications had an average estimated charge of approximately $3,834,
-- while patients receiving higher numbers of medications generally recorded substantially higher estimated charges
-- Recommendations
-- Analyze medication utilization by triage acuity to determine whether higher-acuity patients require more medications.
-- Compare medication utilization with hospital admission and ICU admission.
-- Examine the relationship between medication utilization and Length of Stay.
-- Key Findings
-- Higher medication utilization is generally associated with higher average estimated charges.
--  Patients receiving no medications had the lowest average estimated charges,
-- while patients receiving multiple medications demonstrated substantially higher charges.
-- This relationship may reflect increased clinical complexity and resource utilization among patients requiring more intensive treatment.
-- ----------------------------------------------------------------------------------------------------------------------------------------------------
-- Question 9:
-- How does Admission Status affect estimated charges in the Emergency Department?
-- KPI
-- Average Estimated Charge by Admission Status
select
    admitted,
    count(patient_id) as patient_volume,
    round(avg(estimated_charge_usd),2) as avg_estimated_charge,
    round(sum(estimated_charge_usd),2) as total_estimated_charge
from emergency_department_dataset
group by admitted
order by avg_estimated_charge DESC;
-- Business Interpretation
-- Patients who were admitted to the hospital had a higher average estimated charge per encounter,
--  approximately $10,289.76, compared with $7,435.93 among patients who were not admitted.
-- Recommendations
-- Compare admitted and non-admitted patients by triage acuity, Length of Stay, laboratory utilization, medication utilization, and ICU admission.
-- Identify the resource-utilization patterns associated with higher estimated charges among admitted patients.
-- Monitor high-volume non-admitted encounters because their large volume contributes substantially to overall ED financial activity.
-- Use admission status alongside clinical and operational KPIs when evaluating resource planning and financial performance.
-- Key Findings
-- Admitted patients demonstrated higher financial intensity,
--  with an average estimated charge of $10,289.76 compared with $7,435.93 for non-admitted patients.
-- However, non-admitted encounters generated the majority of total estimated charges because of their substantially higher patient volume.
-- ------------------------------------------------------------------------------------------------------------------------------------------------


