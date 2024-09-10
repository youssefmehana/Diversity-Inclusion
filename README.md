# Diversity-Inclusion
This project is part of a Virtual Internship on Power BI with PwC Switzerland, offered through the Data Analytics Virtual Case Experience by Forage.
# Problem Statement

Our telecom client has been striving to improve gender balance at the executive management level as part of their broader commitment to diversity and inclusion. Despite their efforts, they have not made any significant progress in achieving this goal. The company recognizes that fostering diversity and inclusion is critical to navigating the complexities of today's business environment and has reached out to us for support. At PwC Switzerland, we understand that creating an inclusive workforce with diverse talents is not only essential for business success but also presents practical challenges that require targeted strategies and solutions. The client is seeking our expertise to help overcome these barriers and realize the full potential of their diversity and inclusion efforts.
# Data Preparation

| **Column Name**                      | **Description**                                                                 |
|---------------------------------------|---------------------------------------------------------------------------------|
| Employee ID                          | Represents the unique identifier of the employee                                |
| Gender                               | Describes the gender of the employee                                            |
| Job Level after FY20 promotions       | Describes the employee's job level after promotions in fiscal year 2020         |
| New hire FY20?                       | Indicates whether the employee was a new hire in fiscal year 2020               |
| FY20 Performance Rating              | Represents the employee's performance rating for fiscal year 2020               |
| Promotion in FY21?                   | Indicates whether the employee received a promotion in fiscal year 2021         |
| FY20 leaver?                         | Indicates whether the employee left the company in fiscal year 2020             |
| Department @01.07.2020               | Describes the department the employee was part of as of July 1, 2020            |
| Leaver FY                            | Indicates if the employee was a leaver in the fiscal year                       |
| Job Level after FY21 promotions       | Describes the employee's job level after promotions in fiscal year 2021         |
| Last Department in FY20              | The department the employee was part of in fiscal year 2020                     |
| FTE group                            | Describes the full-time equivalent group of the employee                        |
| Time type                            | Describes the employee's time status (e.g., full-time, part-time)               |
| Department & JL group PRA status     | Status of department and job level group PRA (Performance, Recognition, Award)  |
| Department & JL group for PRA        | Group assigned to the department and job level for PRA                          |
| Job Level group PRA status           | Status of the job level group for PRA                                           |
| Job Level group for PRA              | Group assigned to the job level for PRA                                         |
| Time in Job Level @01.07.2020        | Number of years the employee has been in their job level as of July 1, 2020     |
| Job Level before FY20 promotions     | Describes the employee's job level before promotions in fiscal year 2020        |
| Promotion in FY20?                   | Indicates whether the employee received a promotion in fiscal year 2020         |
| FY19 Performance Rating              | Represents the employee's performance rating for fiscal year 2019               |
| Age group                            | Categorizes the employee's age group                                            |
| Region group: nationality 1          | Describes the region associated with the employee's nationality                 |
| Last hire date                       | The date when the employee was last hired                                       |


**Data Cleansing**

**Years since last hire (Grouped) Column**: A new column named **Years since last hire (Grouped)** was created using the following formula to group employees based on the number of years since their last hire:

```DAX
Years since last hire(Grouped) = SWITCH(
    TRUE(),
    [Years since last hire] < 1, "< 1 year",
    [Years since last hire] >= 1 && [Years since last hire] < 2, "1-2 years",
    [Years since last hire] >= 2 && [Years since last hire] < 3, "2-3 years",
    [Years since last hire] >= 3 && [Years since last hire] < 4, "3-4 years",
    [Years since last hire] >= 4 && [Years since last hire] < 5, "4-5 years",
    [Years since last hire] >= 5 && [Years since last hire] < 6, "5-6 years",
    [Years since last hire] >= 6 && [Years since last hire] < 7, "6-7 years",
    [Years since last hire] >= 7 && [Years since last hire] < 8, "7-8 years",
    [Years since last hire] >= 8 && [Years since last hire] < 9, "8-9 years",
    [Years since last hire] = 9, "9 years"
)
```

Further cleansing was done as follows:

Unnecessary columns were removed

# Data analysis

Here's a structured data analysis similar to your provided example, but adapted for employee-related measures in the `'Pharma Group AG'` dataset:

---

**Data Analysis**

The following DAX measures are used to visualize key metrics within the `'Pharma Group AG'` dataset:

---

### **Promotion Rate (FY21):**
This measure calculates the percentage of employees promoted in FY21:
```DAX
PromotionRateFY21 = 
DIVIDE(
    COUNTROWS(
        FILTER('Pharma Group AG', 'Pharma Group AG'[Promotion in FY21?] = "Yes")
    ),
    COUNTROWS('Pharma Group AG')
)
```

---

### **Percentage of Hires - Men (FY20):**
This measure calculates the percentage of male employees hired in FY20:
```DAX
%HiresMenFY20 = 
DIVIDE(
    COUNTROWS(
        FILTER('Pharma Group AG', 'Pharma Group AG'[New hire FY20?] = "Y" && 'Pharma Group AG'[Gender] = "Male")
    ),
    COUNT('Pharma Group AG'[New hire FY20?])
)
```

---

### **Percentage of Hires - Women (FY20):**
This measure calculates the percentage of female employees hired in FY20:
```DAX
%HiresWomenFY20 = 
DIVIDE(
    COUNTROWS(
        FILTER('Pharma Group AG', 'Pharma Group AG'[New hire FY20?] = "Y" && 'Pharma Group AG'[Gender] = "Female")
    ),
    COUNT('Pharma Group AG'[New hire FY20?])
)
```

---

### **Turnover Rate (FY20):**
This measure calculates the turnover rate in FY20:
```DAX
TurnoverRateFY20 = 
DIVIDE(
    COUNTROWS(
        FILTER('Pharma Group AG', 'Pharma Group AG'[FY20 leaver?] = "Yes")
    ),
    COUNT('Pharma Group AG'[FY20 leaver?])
)
```

---

### **Average Performance Rating - Men (FY20):**
This measure calculates the average performance rating of male employees for FY20:
```DAX
AvgPerformanceRatingMenFY20 = 
CALCULATE(
    AVERAGE('Pharma Group AG'[FY20 Performance Rating]),
    'Pharma Group AG'[Gender] = "Male"
)
```

---

### **Average Performance Rating - Women (FY20):**
This measure calculates the average performance rating of female employees for FY20:
```DAX
AvgPerformanceRatingWomenFY20 = 
CALCULATE(
    AVERAGE('Pharma Group AG'[FY20 Performance Rating]),
    'Pharma Group AG'[Gender] = "Female"
)
```

---

### **Number of Leavers (FY20):**
This measure calculates the number of employees who left the company in FY20:
```DAX
NumberOfLeaversFY20 = 
COUNTROWS(
    FILTER('Pharma Group AG', 'Pharma Group AG'[FY20 leaver?] = "Yes")
)
```

---

### **Number of Male Employees:**
This measure calculates the total number of male employees in the dataset:
```DAX
NumberOfMen = 
COUNTROWS(
    FILTER('Pharma Group AG', 'Pharma Group AG'[Gender] = "Male")
)
```

---

### **Number of Female Employees:**
This measure calculates the total number of female employees in the dataset:
```DAX
NumberOfWomen = 
COUNTROWS(
    FILTER('Pharma Group AG', 'Pharma Group AG'[Gender] = "Female")
)
```
