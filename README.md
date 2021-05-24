# School_District_Analysis

Analysis of standardized testing of a school district using Pandas.
---
## Overview
To help a school board and superintendent from a school district in making decisions regarding the school budgets and priorities we were given information from a variety of sources and in a variety of formats of student funding data and student standardized test scores for math and reading,for each high school in the district.

We were given the task with preparing all standardized test data for analysis, report and present to provide insights about performance trends and patterns. These insights will be used to inform discussions and create strategic decisions at the school and district level.

For the first part of our analysis we had to make a change, since we were informed that the csv file shows evidence of academic dishonesty; specifically, reading and math grades for Thomas High School ninth graders appear to have been altered. To be able to continue with our analysis we decided to null the grades for all the ninth graders of this High School. 

For the reading scores we used the code: 
```
student_data_df.loc[(student_data_df["school_name"]=="Thomas High School")&(student_data_df["grade"]=="9th"),("reading_score")]=np.nan

```

For the math scores we used the code: 
```
student_data_df.loc[(student_data_df["school_name"]=="Thomas High School")&(student_data_df["grade"]=="9th"),("math_score")]=np.nan
```

Here is an example of the new data (the names of the students were hidden because of FERPA)

![School_NaN](https://user-images.githubusercontent.com/78781719/119292565-481b8e80-bc16-11eb-8a4a-0bce482c3c46.png)

Since we have changed the data, now we need to update our data frame, or create a new data frame to give the district the correct analysis.

Then we will calculate the number of students in Thomas High School from 10th to 12th grade. 
If we calculate the number of students with no grade (we assigned NaN to the 9th graders) and subtract to the total count of students in Thomas High School, we will have only the count for students from 10th to 12th grade.

```
student_no_grade = school_data_complete_df.loc[(student_data_df["school_name"]=="Thomas High School")&(student_data_df["grade"]=="9th"),("Student ID")].count()
student_count = school_data_complete_df["Student ID"].count()
new_total_student_count = student_count-student_no_grade
```

Now, we calculate the passing scores and percentages for math and reading: 
```
passing_math_count = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)].count()["student_name"]
passing_reading_count = school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)].count()["student_name"]
passing_math_percentage= (passing_math_count/new_total_student_count)*100
passing_reading_percentage=(passing_reading_count/new_total_student_count)*100
```

And the overall passing percentage:
```
passing_math_reading = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)
                                               & (school_data_complete_df["reading_score"] >= 70)]
overall_passing_math_reading_count = passing_math_reading["student_name"].count()
overall_passing_percentage=(overall_passing_math_reading_count/new_total_student_count)*100
```
The District Summary DataFrame is now like this: 
![district_summary_df](https://user-images.githubusercontent.com/78781719/119293677-89ad3900-bc18-11eb-944d-aded08553d68.PNG)

The School Summary DataFrame: 

to find the percentages,

```
percentage__math_passing_10to12 = ( passing_math_THS['Student ID'].count()/total_count_of_10to12)*100
percentage__reading_passing_10to12 =(passing_reading_THS['Student ID'].count()/total_count_of_10to12)*100
percentage_overall_passing_10to12=(passing__math_reading_THS['Student ID'].count()/total_count_of_10to12)*100
```
We replaced the original percentages with the new values using the .loc 

```
per_school_summary_df.loc[("Thomas High School"),"% Passing Math"]= percentage__math_passing_10to12
per_school_summary_df.loc[("Thomas High School"),"% Passing Reading"]= percentage__reading_passing_10to12
per_school_summary_df.loc[("Thomas High School"),"% Overall Passing"]= percentage_overall_passing_10to12
```
![School_summary_df](https://user-images.githubusercontent.com/78781719/119294596-756a3b80-bc1a-11eb-9fc8-dd72befeeab2.PNG)


---
## Results

### How is the District summary affected?

If we compare the original district summary,
![original_district_summary_df](https://user-images.githubusercontent.com/78781719/119298283-51125d00-bc22-11eb-9fc1-a934de9f9da7.PNG)

to the new district summary, with the changes made on Thomas High School, 
![district_summary_df](https://user-images.githubusercontent.com/78781719/119298319-6a1b0e00-bc22-11eb-9cd4-4af04c750ba8.PNG)

we can notice that we have a very small chage in the passing percentage in math, reading and overall, but is not very significant.

### How is the school summary affected?

On the original analysis we had, 
![original_THS_summary](https://user-images.githubusercontent.com/78781719/119298831-6b006f80-bc23-11eb-9f57-792ca67411e5.PNG)

and now 
![new_THS_summary](https://user-images.githubusercontent.com/78781719/119298852-72c01400-bc23-11eb-9cf6-f95223fe4e62.PNG)

Here is where we can notice the real change in data, since we have a big increase in passing percentages for the 3 categories, having a 10% increase in math, the highest increase in reading with a 24% and overall 21%. 

Replacing the ninth graders math and reading scores of Thomas High School is affecting in a positive way realtive to the other schools, since the High School is now on the top 5 performing schools of the districs. 

Thomas High Scholl will only benefit from this change on the school summary, since we do not see any change in any of the other analysis made by school size, school type, or school spending. 




