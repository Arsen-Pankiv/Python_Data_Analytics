# Python_Data_Analytics

## Acessing and Cleaning Data

In this project, I began by loading a **CSV dataset into a Pandas DataFrame** and converting the date column to datetime format to facilitate time-based operations. I explored techniques to extract specific characters or substrings from string columns using **.str[]** and direct indexing, including extracting substrings from individual cells. For numeric columns like mean_salary, I converted values to strings to enable digit-level extraction.   


![image_5](/screenshots_p/image_5.png)
Handling columns containing string representations of lists, such as **skills_list**, required the use of **ast.literal_eval applied** via a **lambda function** to safely convert these strings into actual Python lists, which allowed me to access individual skills within each row.    
I addressed missing data by calculating **median values** for key columns such as recycling_pct, population_size, and num_of_jobs and then used fillna **to impute** these medians, ensuring a complete dataset. 
<img src="screenshots_p/image_1.png" alt="image_9" width="250" height="250" />


**Duplicate rows** were identified and removed both globally with drop_duplicates() and specifically within certain columns like recycling_pct, which helped eliminate redundant or repeated data entries.


## Applying Functions

Following data cleaning, I applied **various functions** to enhance the dataset. For instance, I defined a projected_salary function that increased median salaries by **3%**, which I then applied across the column using **.apply()**, creating a new column **median_salr_inflated**. 

![image_3](/screenshots_p/image_3.png)
<img src="screenshots_p/image_4.png" alt="image_9" width="300" height="300" />

Additionally, I demonstrated the use of lambda functions to perform similar inline transformations concisely. To implement conditional logic at the row level, I created a function selection21 that applies a salary increase only if the number of jobs exceeds 120,000, otherwise returning 1. This function was applied **row-wise using df.apply(..., axis=1)**, resulting in a new column row_lambda*. These steps illustrate how combining Pandas' vectorized operations with custom Python functions enables powerful, flexible data transformations and insights extraction.

<img src="screenshots_p/image_2.png" alt="image_9" width="300" height="250" />


## Index Management,Pivot Table, Explode Function
### Section 1 - Index Management

I first assigned a custom name, **"value_index"**, to the DataFrame’s index to better identify it. Then, I demonstrated how to locate rows with a specific value in the "area" column (e.g., "brent"). Since the resulting subset rf_brent may have a non-sequential index, I used **reset_index(inplace=True)** to reset the index back to the default integer sequence while preserving the old index as a column. 
![image_6](/screenshots_p/image_6.png)

To restore the original index as the DataFrame’s index, I then applied set_index("value_index", inplace=True). This workflow shows how I managed, reset, and reassigned indexes effectively to maintain clarity and order in the DataFrame.

![image_7](/screenshots_p/image_7.png)
### Section 2 - Pivot Table (pivot_table, group_by)

First, I counted the number of **job listings per job title** using two approaches: rf.groupby("job_title").size() and rf.pivot_table(index="job_title", aggfunc="size"). This gave me an overview of the distribution of job titles in the dataset. 

<img src="screenshots_p/image_9.png" alt="image_9" width="300" height="250" />


Next, I created a pivot table to analyze the average mean salary for each job title across different areas using **rf.pivot_table(values="mean_salary", index="area", columns="job_title", aggfunc="mean")**.   

<img src="screenshots_p/image_10.png" alt="image_9" width="500" height="400" />

Then I  identified the top **2 most frequent areas** using rf["area"].value_counts().head(2).index. Then, I created a pivot table with mean salaries by job title and area using rf.pivot_table(). I filtered this table to keep only those top areas with title_salary_ctr.loc[top61]

![image_11](/screenshots_p/image_11.png)

 Finally, I plotted the filtered data as a grouped bar chart to compare average salaries by job title within these key areas. 

 ![image_12](/screenshots_p/image_12.png)

### Section 3 - Explode Function

I first converted the **"skills_list"** column from strings to actual Python lists using **ast.literal_eval with apply()**. Then, I applied the **explode() method** on the "skills_list" column to transform each list of skills into separate rows, making each skill its own row in the DataFrame **(rf_exploded = rf.explode("skills_list"))**. 

<img src="screenshots_p/image_13.png" alt="image_9" width="500" height="400" />

This made it possible to count **the frequency of each skill across different job titles** using groupby (["job_title", "skills_list"]).size(). The resulting Series was converted into a **DataFrame** with .reset_index(name="skills_count") to label the count column. 
<img src="screenshots_p/image_16.png" alt="image_9" width="500" height="150" />


<img src="screenshots_p/image_14.png" alt="image_9" width="500" height="400" />

I sorted the DataFrame by **"skills_count" in descending order**, filtered the **top 5 skills** for the job title **"Statistician**," and finally plotted these counts with a horizontal bar chart using plot (kind="barh", x="skills_list", y="skills_count") to visualize the most in-demand skills for that role.    


<img src="screenshots_p/image_15.png" alt="image_9" width="500" height="400" />

## Concatting and Merging
### Section 1
### Section 2

## Matplotlib 

## Seaborn 

## Final Projects

### Project 1
### Project 2
### Project 3
### Project 4
### Project 5
### Project 6


