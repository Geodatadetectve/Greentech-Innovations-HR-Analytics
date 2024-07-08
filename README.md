# Greentech-Innovations-HR-Analytics
AMDARI Greentech Innovations HR Analytics
Dynamic Dashboard Analysis for Peak Performance at GreenTech Innovation.
Business Overview/Problem Statement
GreenTech Innovation, a leader in the renewable energy sector with a 15-year history, faces several human resources challenges that need to be addressed using HR analytics. These challenges include:
1.	Employee Performance: There is no comprehensive system to assess and track individual and team performance, making it difficult to identify and reward high-performing employees. This is essential for maintaining a competitive edge.
2.	Employee Engagement: Accurate measurement and understanding of employee engagement are lacking. Without robust data collection and analysis, it is challenging to address issues that could lead to decreased morale and higher attrition rates.

Aim of the Project
GreenTech Innovation aims to enhance its human resources management through data-driven and strategic HR analytics. The company seeks to:
1.	Improve Workforce Decision-Making: Implement a comprehensive HR Dashboard to consolidate HR data, enabling informed decisions, trend identification, and proactive workforce planning.
2.	Enhance Employee Satisfaction and Organizational Performance: Track and analyze employee engagement and career growth to foster a thriving workplace, boosting job satisfaction, loyalty, productivity, and overall organizational performance.

Data Description
The dataset schema include: 
	EmployeeID: Unique identifier for each employee.
	EmployeeName: The full name of the employee.
	Salary: The employee's current salary.
	Position: The job title or role of the employee within the organization.
	State: The state or location where the employee is based.
	DateOfBirth: The birthdate of the employee.
	Gender: The gender of the employee (e.g., Male, Female).
	MaritalStatus: The marital status of the employee (e.g., Single, Married, Divorced).
	HiringDate: The date when the employee was hired.
	TerminationDate: The date when the employee's employment was terminated, if applicable.
	EmploymentStatus: The current employment status of the employee
	Department: The department or division within the organization where the employee works.
	RecruitmentSource: The source through which the employee was recruited
	PerformanceScore: The performance score or rating of the employee, often based on performance evaluations
	EngagementSurvey: A measure of employee engagement or satisfaction, often collected through surveys.
	EmployeeSatisfaction: The level of job satisfaction reported by the employee
	Education Qualification: The highest level of education qualification attained by the employee

Tools: Microsoft Power BI® 

Skills applied:
ETL including data loading, Modelling, Analysis & Visualization.

Some Complex DAX Functions
Most Engaged Employee = 
VAR MostEngagedEmployee = 
FIRSTNONBLANK(
    TOPN(
        1,
        VALUES('HR Dataset'[EmployeeName]),
        [Avg Eng. Score],
    ),
    1
    )

VAR AvgScoreForMostEngaged =
CALCULATE(
    [Avg Eng. Score],
    FILTER('HR Dataset',
    'HR Dataset'[EmployeeName]=MostEngagedEmployee
    )
)    
RETURN
CONCATENATE(
    MostEngagedEmployee,
    ", Engagement Score: "& FORMAT(AvgScoreForMostEngaged, "0.00"))


Least Engaged Employee = VAR LeastEngagedEmployee = 
    CALCULATE(
        LASTNONBLANK(
            TOPN(
                1,
                VALUES('HR Dataset'[EmployeeName]),
                [Avg Eng. Score], 
                ASC
            ),
            1
        )
    )

VAR AvgScoreForLeastEngaged =
    CALCULATE(
        [Avg Eng. Score],
        FILTER('HR Dataset', 'HR Dataset'[EmployeeName] = LeastEngagedEmployee)
    )    

RETURN
    LeastEngagedEmployee & ", Engagement Score: " & FORMAT(AvgScoreForLeastEngaged, "0.00")

CorrCoefficient = 
VAR n = COUNTROWS('HR Dataset')
VAR SmX = SUM('HR Dataset'[EmployeeSatisfaction])
VAR SumY = SUM('HR Dataset'[Eng Survey])
VAR SumX2 = SUMX('HR Dataset', 'HR Dataset'[EmployeeSatisfaction] * 'HR Dataset'[EmployeeSatisfaction])
VAR SumY2 = SUMX('HR Dataset', 'HR Dataset'[Eng Survey] * 'HR Dataset'[Eng Survey])
VAR SumXY = SUMX('HR Dataset', 'HR Dataset'[EmployeeSatisfaction] * 'HR Dataset'[Eng Survey])

RETURN
DIVIDE(
    n * SumXY - SmX * SumY,
    SQRT((n * SumX2 - SmX * SmX) * (n * SumY2 - SumY * SumY))
)


CorrelationCoefficient = 
VAR MeanX = AVERAGE('HR Dataset'[EmployeeSatisfaction])
VAR MeanY = AVERAGE('HR Dataset'[Eng Survey])
VAR n = COUNTROWS('HR Dataset')

VAR COVAR =
    SUMX(
        'HR Dataset',
        ('HR Dataset'[EmployeeSatisfaction] - MeanX) * ('HR Dataset'[Eng Survey] - MeanY)
    ) / (n - 1)

VAR StdDevX = 
    SQRT(
        SUMX(
            'HR Dataset',
            POWER('HR Dataset'[EmployeeSatisfaction] - MeanX, 2)
        ) / (n - 1)
    )

VAR StdDevY = 
    SQRT(
        SUMX(
            'HR Dataset',
            POWER('HR Dataset'[Eng Survey] - MeanY, 2)
        ) / (n - 1)
    )

VAR COR = IF(StdDevX * StdDevY <> 0, COVAR / (StdDevX * StdDevY), BLANK())

RETURN
    COR



Employee Statistics and Insights at GreenTech Innovations
Employee Demographics and Distribution: GreenTech Innovations employs a total of 315 individuals, with the largest department being Production, which accounts for 210 employees, representing a significant 20,900% more than the Executive office, which has the lowest count at just 1 employee. Most employees are in Massachusetts, totaling 277 individuals. The workforce composition includes 178 females and 137 males. In terms of marital status, the majority are single (46.35%), followed by married individuals, while divorced employees constitute a smaller proportion (13.65%). The age distribution shows that 37.14% of employees are between 40-48 years old, and 30.79% are aged between 31-39.

Employee Recruitment and Termination: Indeed is the largest source of recruitment, accounting for 28.25% of all hires across nine different sources, ranging in total employees from 1 to 89. Regarding employment status, 67% of the workforce are active employees, while 33% have either voluntarily left or were terminated.

Educational Qualifications and Engagement: Most employees hold Diploma degrees (191). Employees with Master's degrees (MSc) exhibit the highest average engagement score at 4.23, which is 4.92% higher than those with High School Diplomas, who have the lowest score at 4.03. Engagement scores vary across all four educational categories from 4.03 to 4.23.

Gender and Engagement: Female employees demonstrate slightly higher engagement levels compared to males, with less than a 2% difference.

Recruitment Source and Engagement: Online web applications yield the highest average engagement score at 5.0, which is 26.78% higher than Indeed, the lowest scorer at 3.94. Engagement scores across all recruitment sources range from 3.94 to 5.0, indicating significant variability.

Performance Metrics: The average performance score for females stands at 3.99, slightly surpassing males at 3.88. June in 2013 contributed approximately 1.94% to the average performance score. Single employees show a performance score of 3.95, which is higher than that of married (3.89) and divorced (4.07) employees.
Performance by Education and Age: Employees with MSc degrees also achieve the highest average performance score at 4.23, which is 9.94% higher than those with Diplomas, who have the lowest score at 3.85. Employees aged 49 and above, followed closely by those aged 40-48, show the highest performance. Additionally, employees with 7 and 10 years of service show the highest performance scores.

Departmental Performance: The Administrative offices department demonstrates superior performance compared to other departments. Production and Sales teams have higher employee counts compared to the PIP team.

Correlations: There is a low correlation (r=21%) between employee engagement and satisfaction, while employee performance and satisfaction show a moderate to high correlation (r=64%).



RECOMMENDATION

Based on the insights and data from GreenTech Innovations, here are five key recommendations:
	Diversify Recruitment Sources: While Indeed is a significant source of recruitment, consider diversifying recruitment channels to mitigate dependency and potentially improve engagement. Exploring other effective sources identified could help attract diverse talent pools and enhance overall engagement scores.
	Invest in Employee Engagement Initiatives: Given the varied engagement scores across education levels and recruitment sources, prioritize targeted engagement initiatives. Focus on enhancing engagement strategies for employees with High School Diplomas and those recruited via Indeed, aiming to bridge the gap observed with higher-scoring groups.
	Tailor Development Programs by Age Group: Recognizing the performance peaks among older employees (aged 49 and above), tailor professional development programs to use their expertise and maintain high performance levels. Similarly, invest in career development opportunities for employees in the 40-48 age bracket to sustain their engagement and performance.
	Enhance Gender-specific Engagement Strategies: While female employees show marginally higher engagement levels, consider implementing gender-specific engagement strategies to further boost satisfaction and performance. Conducting gender-focused engagement surveys and adjusting policies accordingly could help address any potential disparities.
	Strengthen Departmental Performance Management: Given the Administrative offices' outperformance, analyze and replicate successful practices across other departments. Implement robust performance management frameworks tailored to each department's unique dynamics to foster consistency in achieving organizational goals.

These recommendations aim to capitalize on strengths, address disparities, and foster a more engaged and high-performing workforce at GreenTech Innovations.


Data Source: https://amdari.io
Data Analyst: Kamaldeen O Omosanya (https://github.com/Geodatadetectve?tab=repositories)

