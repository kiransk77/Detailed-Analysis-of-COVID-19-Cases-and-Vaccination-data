Data Mining Assignment 1

---------------------
Table of contents
---------------------
1. Introduction
2. Prerequisites
3. Constraints
4. Usage
5. How to execute
6.Approach write-up
7. Output files
8. Web links


Introduction
---------------
The objective of the assignment is to get COVID-19 related information like cases, vaccination count and so on from the online APIs
also, mine the data in order to comprehend various patterns in case counts and vaccination numbers.

Prerequisites
-----------------
Technical : Python,Numpy,Pandas and the Linux environment to run .sh script files.
In Linux bash shell,   "jupyter-nbconvert " command, (if not installed)

Constraints
--------------
1. In order to bound the analysis to a certain period,Date is limited to 14-08-2021
2. Data files are not pulled directly from api link as sometimes the website isn't functional.
    -   Instead downloaded files are used.
    -  *** These files and program should have the same path. Please add all files to a common folder .
3. All the output files will be generated in the path "<current directory>/Output_Files/Qx " ; where x is 1,2,3...9
   

Usage
--------
This assignment is handy in knowing various trends in the covid cases and vaccination numbers. 
It does the following for all districts,states and overall: 
      - Gives weekly, monthly covid cases data for every district, state and overall.
      - Identifies the week and month for peaks of wave 1 and wave2.
      - Calculates total dose1 and dose2 vaccination numbers.
      - Calculates female to male vaccination and population ratios, also ratio of both ratios.
      - Gives the Covishield to Covaxin vaccination ratio
      - Calculates the ratio of total vacacinations to population. 
      - Estimates a future date by which every state achieves complete vaccination atleast for one dose.

How to execute
-------------------
Individual .sh files for each question are created as list below:(file names doesn't include quotations)
Q1. "neighbor-districts-script.sh"
Q2. "edge-generator.sh"
Q3. "case-generator.sh"
Q4. "peaks-generator.sh"
Q5. "vaccinated-count-generator.sh"
Q6.  "vaccination-population-ratio-generator.sh"
Q7.  "vaccine-type-ratio-generator.sh"
Q8.  "vaccinated-ratio-generator.sh"
Q9.  "complete-vaccination-generator.sh"

Note: "assign1.sh" is for running entire Assignment-1 which executes all the above files
Command:   ./<script-file-name>   
                 Example: ./peaks-generator.sh

Approach write-up
---------------------
      - This assignment demands 10 questions including the requirement of a manual. 
      - Weeks are calculated from Sunday to Saturday 
      - Months are taken as a period from 15th to 14th unlike 1st to 30th/31st in Gregorian Calender
      - In all questions, after date adjustments, weekids and monthids are assigned to dates to group them accordingly for weekly and monthly counts.
      - The fact that covid and vaccination data is cummulative, is handled by taking difference of consecutive columns.
1st question -> To modify and update the given neighbours-districts.json with latest details of districts from vaccination data.
                            - removed the unwanted districts.
                            - created a dictionary for corresponding old and new values, replaced old names with new names.
                            - replaced new names with district keys
                            - merged districts of Delhi.
2nd question -> Node pairs of  Undirected graph for adjacent districts. 
3rd question -> Weekly, monthly, overall case counts for every district, Cycle of Month is 15th-14th  
                          - For a uniform monthid, current date is rolled back by 14 days to fit the cycle in one Gregorian month
                          - Assigned weekids and monthids for each day
                          - Grouped by weekids, took max value, and subtracted consecutive columns for individual week count.
                          - Similarly,months and overall
4th question -> Finding weekids,monthids of first and second wave peaks for districts,states and overall.
                          - Overlapping of week is given in the question.
                          - Assigned odd ids and even ids to different sets, merged both sets. 
                                   - odd ids are Sunday to Saturday cyles and odd ids are Thursday to Wednesday cycles
                          - Applied similar procedure of 3rd question and calculated the max value of respective waves by slicing .
5th question -> Calculating number of people vaccinated with dose1 or dose2 in each district,state and overall.
                          - Used column slicing to take dose1 and dose2 data separately.
                          - Applied melting in the data and converted coulmns into rows.
                          - Grouped different state codes, weekids/monthids etc and calculated the vaccination count
6th question -> Ratio of ratio,i.e., vaccination ratio to population ratio, where ratio is female to male numbers.
                          - As data is cummulative, last columns of female and male vaccinated numbers are taken.
                          - Female and Male population figures are taken from census data
                          - Grouping on district ids, state codes for district and state values respectively.
7th question -> Vaccine ratio, Covishield to Covaxin.
                          - Last columns of Covishield and Covaxin counts are taken.
                          - Grouping on district ids, state codes for respective district and state values.
                          - Ratio is calculated accordingly.
8th question -> total number of persons vaccinated (both 1 and 2 doses) to total population.
                          - Considered only last columns of dose1 and dose2.
                          - Census data gives population counts.
                          - Grouping on districtids, state codes for respective district and state values.
9th question -> For every state,date on which the entire population will get at least one does of vaccination.
                          - rate of vaccination is vaccination of last week i.e week ending on 14-08-2021. 
                          - RATE OF VACCINATION is Difference of 7th and 14th August is divided by 7 
                          - POPULATION LEFT is Difference of district population(From Census) and Dose1 (From COWIN) 
                          - division of populationleft with rateofvaccination gives number of days required 
                          - Sum of Current date and days required gives final date for atleast one dose vaccination.

Output Files
---------------
Q1 - neighbor-districts-modified.json
Q2 - edge-graph.csv
Q3 - cases-week.csv , cases-month.csv, cases-overall.csv

Q4 - district-peaks.csv, state-peaks.csv, overall-peaks.csv

Q5 - district-vaccinated-count-week.csv
      - district-vaccinated-count-month.csv, 
      - district-vaccinated-count-overall.csv
      - state-vaccinated-count-week.csv
      - state-vaccinated-count-month.csv
      - state-vaccinated-count-overall.csv

Q6 - district-vaccination-population-ratio.csv
      - state-vaccination-population-ratio.csv
      - overall-vaccination-population-ratio.csv

Q7 - district-vaccination-type-ratio.csv
      - state-vaccination-type-ratio.csv
      - overall-vaccination-type-ratio.csv

Q8 - district-vaccinated-dose-ratio.csv
      - state-vaccinated-dose-ratio.csv
      - overall-vaccinated-dose-ratio.csv

Q9 - complete-vaccination.csv

Web Links
-------------
VACCINE DATA: http://data.covid19india.org/csv/latest/cowin_vaccine_data_districtwise.csv
DISTRICT CASE DATA: https://data.covid19india.org/csv/latest/districts.csv
CENSUS DATA: http://censusindia.gov.in/pca/DDW_PCA0000_2011_Indiastatedist.xlsx.