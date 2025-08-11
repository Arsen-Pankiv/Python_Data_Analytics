# Python_Data_Analytics


<img src="screenshots/image_22.png" alt="image_9" width="400" height="500" />



## Acessing and Cleaning Data

In this project, I began by loading a **CSV dataset into a Pandas DataFrame** and converting the date column to datetime format to facilitate time-based operations. I explored techniques to extract specific characters or substrings from string columns using **.str[]** and direct indexing, including extracting substrings from individual cells. For numeric columns like mean_salary, I converted values to strings to enable digit-level extraction.   


![image_5](/screenshots_p/image_5.png)
Handling columns containing string representations of lists, such as **skills_list**, required the use of **ast.literal_eval applied** via a **lambda function** to safely convert these strings into actual Python lists, which allowed me to access individual skills within each row.    
I addressed missing data by calculating **median values** for key columns such as recycling_pct, population_size, and num_of_jobs and then used fillna **to impute** these medians, ensuring a complete dataset. 

![image_1](/screenshots_p/image_1.png)

**Duplicate rows** were identified and removed both globally with drop_duplicates() and specifically within certain columns like recycling_pct, which helped eliminate redundant or repeated data entries.


## Applying Functions

Following data cleaning, I applied **various functions** to enhance the dataset. For instance, I defined a projected_salary function that increased median salaries by **3%**, which I then applied across the column using **.apply()**, creating a new column **median_salr_inflated**. 

![image_3](/screenshots_p/image_3.png)
![image_4](/screenshots_p/image_4.png)

Additionally, I demonstrated the use of lambda functions to perform similar inline transformations concisely. To implement conditional logic at the row level, I created a function selection21 that applies a salary increase only if the number of jobs exceeds 120,000, otherwise returning 1. This function was applied **row-wise using df.apply(..., axis=1)**, resulting in a new column row_lambda*. These steps illustrate how combining Pandas' vectorized operations with custom Python functions enables powerful, flexible data transformations and insights extraction.

![image_2](/screenshots_p/image_2.png)

## Index Management,Pivot Table, Explode Function
### Section 1
### Section 2
### Section 3

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


