## I have anlysed an HR dataset about absenteeism and health. I have develop a database, SQL queries to answer the requirments and built a dashboard. I have used Microsoft SQL Server Management Studio.
## Requirment : Determine how to provide a bonus and incentive to healthy employees.

# SQL Queries:
   --creating a join table
Select * from Absenteeism_at_work a
left join compensation b
On a.ID = b.ID
left join Reasons r
on a.Reason_for_absence = r.Number

---find the healthiest

Select * from Absenteeism_at_work 
Where Social_drinker = 0 and Social_smoker = 0 
and Body_mass_index <25 and 
Absenteeism_time_in_hours < (Select AVG(Absenteeism_time_in_hours) from Absenteeism_at_work )

---compensation rate increase for non-smokers/budget $983,221 so.68 increase per hour/$141.4 per year

Select COUNT(*) as nonsmokers from Absenteeism_at_work
Where Social_smoker = 0

--optimizing the query

Select
a.ID,
r.Reason,
Month_of_absence,
Body_mass_index,
Case When Body_mass_index <18.5 then 'underweight'
     When Body_mass_index between 18.5 and 24.9 then 'Healthy Weight'
	 When Body_mass_index between 25 and 30 then 'Overweight'
	 When Body_mass_index >18.5 then 'Obese'
	 Else 'Unknown' End as BMI_category,
CASE WHEN Month_of_absence IN (12,1,2) Then 'Winter'
     WHEN Month_of_absence IN (3,4,5) Then 'Spring'
	 WHEN Month_of_absence IN (6,7,8) Then 'Summer'
	 WHEN Month_of_absence IN (9,10,11) Then 'Fall'
	 Else 'Unknown' END as Season_Names,
Month_of_absence,
Day_of_the_week,
Transportation_expense,
Education,
Son,
Social_drinker,
Social_smoker,
Pet,
Disciplinary_failure,
Age,
Work_load_Average_day,
Absenteeism_time_in_hours
from Absenteeism_at_work a
left join compensation b
On a.ID = b.ID
left join Reasons r
on a.Reason_for_absence = r.Number
