![image](https://github.com/user-attachments/assets/3d143227-a967-4e55-ac48-bda4fd0759a8)# Project: Finance Dashboard (Management)

## Table of Content

1. [Introduction](#introduction)
2. [Project Objectives](#project-objectives)
3. [Research Questions](#research-questions)
4. [About the Dataset](#about-the-dataset)
5. [Languages, Utilities, and Environments Used](#languages-utilities-and-environments-used)
6. [Importing the Datasets into Power BI](#importing-the-datasets-into-power-bi)
7. [Data Automation: Cleaning and Transformation](#data-automation-cleaning-and-transformation)
   - [Unpivot Columns](#unpivot-columns)
   - [Rename Columns](#rename-columns)
   - [Add Custom Column](#add-custom-column)
   - [Create New Tables](#create-new-tables)
   - [Create Calendar Table](#create-calendar-table)
8. [Data Modelling](#data-modelling)
9. [Data Analysis using Power BI DAX and Visualizations](#data-analysis-using-power-bi-dax-and-visualizations)
   - [DAX Measures](#dax-measures)
   - [Data Visualizations](#data-visualizations)
10. [Insights from the Data Analysis](#insights-from-the-data-analysis)
11. [Recommendations from the Data Analysis](#recommendations-from-the-data-analysis)
12. [Conclusion](#conclusion)
13. [Glossary of Terms](#glossary-of-terms)



## Introduction
In an increasingly interconnected world, cybersecurity has become one of the most critical challenges facing organizations, governments, and individuals alike. The frequency, scale, and sophistication of cyberattacks have escalated dramatically, causing widespread disruptions and significant financial losses across industries and borders.
This report presents a comprehensive analysis of global cybersecurity threat data between 2015 - 2024, with a focus on identifying the most exploited vulnerabilities, the countries and industries most affected, and the financial toll of these attacks. It also highlights the most commonly deployed defense mechanisms and evaluates their effectiveness in mitigating risk.

## Project Objectives
1. To evaluate the impact of cybersecurity threats across different industries and regions.
2. To identify most exploited vulnerabilities, and most effective defence mechanisms deployed in threat resolution.
3. To assess the financial impact of these threats year-on-year within the most affected regions and industries.
4. To provide recommendations based on the data findings on best practises to mitigate or prevent these attacks.

## Research Questions
The project aims to answer the following research questions:
1. Which cybersecurity vulnerabilities are most frequently exploited across industries, and why?
2. What are the financial and operational impacts of different types of cyberattacks
3. Are there any seasonal or annual trends in the types of attacks being launched?
4. How do attack sources differ by country?
5. Does increased use of AI-based detection correlate with quicker response and containment?
6. Which defense strategies are underutilized despite showing potential

## About the Dataset
The dataset was downloaded from Kaggle. It contains 3000 rows and 10 columns.
[(Link to the dataset)]([https://www.kaggle.com/datasets/atharvasoundankar/global-cybersecurity-threats-2015-2024))

* *Country*: Affected countries
* *Year*: Year of cyber attack
* *Attack type*: Differnt cybersecurity attacks
* *Target Industry*: Affected Industry
* *Financial Loss (in Million $)*: Monetary value of loses incured due to the attack
* *Number of Affected Users*: Numeric value of total persons attacked
* *Attack Source*: Source of security attack
* *Defense Mechanism Used*: Mechanism deployed to resolve threat
* *Incident Resolution Time (in Hours)*: Time taken to resolve threat

## Languages, Utilities, and Environments Used
* Power Query: Data Automation: Cleaning, and Transformation
* Power BI: Data Analysis and Exploration [(link to the Power BI file)](https://drive.google.com/file/d/140wDxkTPf6wn8p11BNEK0SPaE0E61tOl/view?usp=drive_link)

## Importing the Datasets into Power BI
To import the dataset into Power BI, I proceeded as follows:  
* Launched the Microsoft Power BI app
* Created a blank report > Add data to your report > Import Data from Excel
* Clicked on Browse > selected the file from my computer > Open
* In the new window that appears, I clicked on the data to preview it then clicked on Transform Data 
* This launched into the Power Query editor > Next
* Then I proceeded to apply a couple of transformations and cleaning to the datasets as outlined in the next section.
The above steps successfully imported the dataset into my Power Query editor and ready for transformation.

## Data Automation: Cleaning and Transformation
*  The dataset was mostly cleaned from the source, I however followed the following standard practises to ensure the dataset was suitable for the required analysis
**Validated Column properties**
*  Validated column properties such as Data Type, Column Quality, Column Profile, and Column Distribution
*  Checked for blank rows and null values across all columns
*  Checked for duplicate rows and columns
*  Checked for any inconsistency or data entry errors within columns

## Data Analysis using Power BI DAX and visualizations

### Dax Measures
* To analyze the dataset, I wrote a total of 13 dax measures to extract needed information from the tables. Some of these measures are listed below:
1. * Percentage of most affected countries
```
% of Most Affected Country = 
VAR MostAffectedCountry = 
    maxx(TOPN(1, 
        SUMMARIZE(
            'Global_Cybersecurity_Threats_2015-2024', 
            'Global_Cybersecurity_Threats_2015-2024'[Country], 
            "ThreatCount", COUNTROWS('Global_Cybersecurity_Threats_2015-2024')
        ), 
        [ThreatCount], DESC
    ),
    'Global_Cybersecurity_Threats_2015-2024'[Country]
)

VAR Count_MostAffected = 
    CALCULATE(
        COUNTROWS('Global_Cybersecurity_Threats_2015-2024'),
        'Global_Cybersecurity_Threats_2015-2024'[Country] = MostAffectedCountry
    )

VAR All_Count = COUNTROWS('Global_Cybersecurity_Threats_2015-2024')

VAR Perc_Value = FORMAT(DIVIDE(Count_MostAffected, All_Count, 0), "0.00%")

RETURN
    CONCATENATE(Perc_Value, " of Total Cases")
```
2. Most Exploited Vulnerabilities
```
Most Exploited Vulnerability = 
var GroupedTable =
    SUMMARIZE(
        'Global_Cybersecurity_Threats_2015-2024',
        'Global_Cybersecurity_Threats_2015-2024'[Security Vulnerability Type],
        "@Count",COUNTROWS('Global_Cybersecurity_Threats_2015-2024')
    )
var maxCount =
    MAXX(GroupedTable, [@Count])
RETURN
    MAXX(
        FILTER(
            GroupedTable,
            [@Count] = maxCount
        ),
        'Global_Cybersecurity_Threats_2015-2024'[Security Vulnerability Type]
    )
```
3. * Average resolution time
```
Average resolution time2 = 
var time_ =format(AVERAGE('Global_Cybersecurity_Threats_2015-2024'[Incident Resolution Time (in Hours)]),"0.00")
RETURN
    CONCATENATE(time_," hrs")
```
4. * Most used defence mechanism
```
Most Used Defence Mechanism = 
var GroupedTable =
    SUMMARIZE(
        'Global_Cybersecurity_Threats_2015-2024',
        'Global_Cybersecurity_Threats_2015-2024'[Defense Mechanism Used],
        "@Count",COUNTROWS('Global_Cybersecurity_Threats_2015-2024')
    )
var maxCount =
    MAXX(GroupedTable, [@Count])
RETURN
    MAXX(
        FILTER(
            GroupedTable,
            [@Count] = maxCount
        ),
        'Global_Cybersecurity_Threats_2015-2024'[Defense Mechanism Used]
    )
```
5. Total Financial Losses
```
Total Financial Losses = sum('Global_Cybersecurity_Threats_2015-2024'[Financial Loss (in Million $)])*1000000
```


## Data Visualizations
To visualize the data, I used the following native power BI visuals: Matrix, Cards, Doughnut chart, Slicer, Sparkline, and conditional formatting tools. See dashboard snippet below:

![Dashboard snippet](https://github.com/davidutibe/finance_management_dashboard/blob/main/Finance%20Dashboard%20(Management).JPG?raw=true)

## Insights from the Data Analysis
1. **Which cybersecurity vulnerabilities are most frequently exploited across industries, and why?**  
   * *Insights*: Zero day were most prevalent in the IT and Telecoms industry, weak passwords had the most effect on Government and the Education industry ,while social engineering was mostly prevalent in Health Care and Banking.
     * Zero day vulnerability accounted for 26% of total global cases, and accounted for 28% of total cases within the IT industry. This is most likely due to the ikely due to thereliance on cutting-edge technologies within this industry.
2. **What are the financial and operational impacts of different types of cyberattacks?**
  * *Insight*: DDoS and Man-in-the-Middle attacks yield the highest financial impact, potentially due to widespread service disruption and data compromise.
    * Operationally, resolution times hover around 36 hours, with minor variation by attack type.
3. **Are there any seasonal or annual trends in the types of attacks being launched?**
  * *Insights*: Phishing remains consistently high across years.
    * SQL Injection and Malware have been surging since 2021, possibly due to increased digital transformation and legacy database systems being exposed.
4. **How do attack sources differ by country?**
  *  *Insights*: China and Germany face more state-level interference.
  * Insider threats are notably high in countries like Australia and the US.
5. **Does increased use of AI-based detection correlate with quicker response and containment?**
    * *Insights: Average resolution time with AI-based-detection is 36.61 hours, while overall average resolution time is 36.48 hours. AI-based detection does not significantly outperform other methods in resolution time.
This may reflect inefficiencies in post-detection response or limitations in AI deployment.

## Recommendations from the Data Analysis
1. **Strengthen Zero-Day Vulnerability Management Across Key Industries**
   * The IT industry had the highest frequency of Zero-day vulnerabilities (137 cases), followed by Education and Telecommunications. These sectors also showed high exposure to weak passwords and unpatched software.
    To curb this increasing risk, stakeholders can implement automated vulnerability scanning and threat intelligence feeds to detect Zero-day vulnerabilities proactively.
2. **Invest in Advanced Authentication and Credential Management**
    * Weak passwords were especially prevalent in Healthcare (115 cases) and IT (122 cases). These sectors also experienced higher-than-average user impact (over 500K users per incident on average). Stakeholders can 
    introduce passwordless authentication (e.g., biometrics, hardware tokens) in sectors handling sensitive PII like Banking and Healthcare.
3. **Reprioritize Defense Strategy: Elevate AI-Based Detection and Firewall Use**
    * Despite showing competitive performance (Avg. resolution time: 36.61 hrs, slightly above average), AI-based Detection was the least used method (only 583 instances). Firewalls also showed relatively low adoption despite their foundational role.
    Stakeholders can increase AI-based deployment for real-time anomaly detection and pair it with automated containment tools.
4. **Build Country-Specific Threat Intelligence and Insider Threat Mitigation**
   * Australia and China showed high rates of insider attacks, while Brazil and France had a significant share of unknown sources, China and Germany also experienced a high number of Nation-State cases. Stakeholders can Create
   * country-specific playbooks for responding to nation-state tactics, techniques, and procedures.
5. **Address Social Engineering Risks With Behavior-Centric Training**
   * Social Engineering was the top vulnerability in user-centric sectors like Education, Government, and Retail. These sectors reported incident counts above 100 per vector, and user impact ranging between 490 –520,000 on average. 
     Stakeholders can create real-time reporting systems for employees to flag suspicious interactions or commununications.
## Conclusion
  Cybersecurity continues to be a critical area of concern across industries and nations, with evolving threats posing significant financial, operational, and reputational risks. This report leveraged a 10-year dataset to uncover key insights into the nature and impact of cyberattacks, the most commonly exploited vulnerabilities, and the sectors and countries most affected.
  Moving forward, organizations must prioritize proactive threat identification, continuous employee training, and the deployment of automated response systems. By adopting these recommendations, businesses and governments can significantly enhance their cybersecurity posture, reduce risk exposure, and better safeguard sensitive data in an increasingly interconnected world.

## Glossary of Terms

 - **Zero-Day Vulnerability**  
  A security flaw unknown to the software vendor, exploited by attackers before a fix is released.

- **Phishing**  
  A social engineering attack where fraudulent messages are used to trick individuals into revealing sensitive information.

- **Ransomware**  
  Malware that encrypts a victim’s files, demanding payment for decryption.

- **DDoS (Distributed Denial of Service)**  
  An attack where multiple systems flood the bandwidth or resources of a target, causing service disruption.

- **SQL Injection**  
  A code injection technique that exploits security vulnerabilities in a database layer by injecting malicious SQL statements.

- **Man-in-the-Middle (MitM)**  
  An attack where the attacker intercepts communication between two parties to steal or manipulate data.

- **Social Engineering**  
  Manipulation tactics used to trick individuals into divulging confidential information.

- **Unpatched Software**  
  Software lacking the latest security updates, making it vulnerable to known exploits.

- **Weak Passwords**  
  Easily guessable or reused credentials that increase risk of unauthorized access.

- **COGS (Cost of Goods Sold):** 


<br/>
   
**Thank you for taking the time to read through this project!**

**For inquiries, collaboration opportunities, or to engage my services, feel free to reach out via email: mdavidutibe@gmail.com.**

### Author
[David Utibe Michael](https://github.com/davidutibe)
