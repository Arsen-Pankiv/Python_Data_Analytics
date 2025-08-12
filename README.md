# Python_Data_Analytics

## Acessing and Cleaning Data

In this project, we began by loading a **CSV dataset into a Pandas DataFrame** and converting the date column to datetime format to facilitate time-based operations. We explored techniques to extract specific characters or substrings from string columns using **.str[]** and direct indexing, including extracting substrings from individual cells. For numeric columns like mean_salary, we converted values to strings to enable digit-level extraction.   

<img src="screenshots_p/image_5.png" alt="image_5" width="500" height="100" />

Handling columns containing string representations of lists, such as **skills_list**, required the use of **ast.literal_eval applied** via a **lambda function** to safely convert these strings into actual Python lists, which allowed us to access individual skills within each row.    
We addressed missing data by calculating **median values** for key columns such as recycling_pct, population_size, and num_of_jobs and then used fillna **to impute** these medians, ensuring a complete dataset. 
<img src="screenshots_p/image_1.png" alt="image_9" width="250" height="250" />


**Duplicate rows** were identified and removed both globally with drop_duplicates() and specifically within certain columns like recycling_pct, which helped eliminate redundant or repeated data entries.


## Applying Functions

Following data cleaning, we applied **various functions** to enhance the dataset. For instance, we defined a projected_salary function that increased median salaries by **3%**, which we then applied across the column using **.apply()**, creating a new column **median_salr_inflated**. 

![image_3](/screenshots_p/image_3.png)
<img src="screenshots_p/image_4.png" alt="image_9" width="300" height="300" />

Additionally, we demonstrated the use of lambda functions to perform similar inline transformations concisely. To implement conditional logic at the row level, we created a function selection21 that applies a salary increase only if the number of jobs exceeds 120,000, otherwise returning 1. This function was applied **row-wise using df.apply(..., axis=1)**, resulting in a new column row_lambda*. These steps illustrate how combining Pandas' vectorized operations with custom Python functions enables powerful, flexible data transformations and insights extraction.

<img src="screenshots_p/image_2.png" alt="image_9" width="300" height="250" />


## Index Management,Pivot Table, Explode Function
### Section 1 - Index Management

We first assigned a custom name, **"value_index"**, to the DataFrame’s index to better identify it. Then, we demonstrated how to locate rows with a specific value in the "area" column (e.g., "brent"). Since the resulting subset rf_brent may have a non-sequential index, we used **reset_index(inplace=True)** to reset the index back to the default integer sequence while preserving the old index as a column. 
![image_6](/screenshots_p/image_6.png)

To restore the original index as the DataFrame’s index, we then applied set_index("value_index", inplace=True). This workflow shows how we managed, reset, and reassigned indexes effectively to maintain clarity and order in the DataFrame.

![image_7](/screenshots_p/image_7.png)
### Section 2 - Pivot Table (pivot_table, group_by)

First, we counted the number of **job listings per job title** using two approaches: rf.groupby("job_title").size() and rf.pivot_table(index="job_title", aggfunc="size"). This gave us an overview of the distribution of job titles in the dataset. 

<img src="screenshots_p/image_9.png" alt="image_9" width="300" height="250" />


Next, we created a pivot table to analyze the average mean salary for each job title across different areas using **rf.pivot_table(values="mean_salary", index="area", columns="job_title", aggfunc="mean")**.   

<img src="screenshots_p/image_10.png" alt="image_9" width="500" height="400" />

Then we  identified the top **2 most frequent areas** using rf["area"].value_counts().head(2).index. Then, we created a pivot table with mean salaries by job title and area using rf.pivot_table(). We filtered this table to keep only those top areas with title_salary_ctr.loc[top61]

<img src="screenshots_p/image_11.png" alt="image_11" width="400" height="200" />

 Finally, we plotted the filtered data as a grouped bar chart to compare average salaries by job title within these key areas. 

 ![image_12](/screenshots_p/image_12.png)

### Section 3 - Explode Function

Wefirst converted the **"skills_list"** column from strings to actual Python lists using **ast.literal_eval with apply()**. Then, we applied the **explode() method** on the "skills_list" column to transform each list of skills into separate rows, making each skill its own row in the DataFrame **(rf_exploded = rf.explode("skills_list"))**. 

<img src="screenshots_p/image_13.png" alt="image_9" width="500" height="400" />

This made it possible to count **the frequency of each skill across different job titles** using groupby (["job_title", "skills_list"]).size(). The resulting Series was converted into a **DataFrame** with .reset_index(name="skills_count") to label the count column. 
<img src="screenshots_p/image_16.png" alt="image_9" width="500" height="150" />


<img src="screenshots_p/image_14.png" alt="image_9" width="500" height="400" />

We sorted the DataFrame by **"skills_count" in descending order**, filtered the **top 5 skills** for the job title **"Statistician**," and finally plotted these counts with a horizontal bar chart using plot (kind="barh", x="skills_list", y="skills_count") to visualize the most in-demand skills for that role.    


<img src="screenshots_p/image_15.png" alt="image_9" width="500" height="400" />

## Concat and Merge DataFrames
### Section 1 - Concat Method
#### Part 1
We loaded two CSV files into DataFrames (m1 and m2) and combined them vertically using **pd.concat()**, effectively stacking the rows from one under the other. We also demonstrated ignore_index=True to reset the index in the merged DataFrame.
**m1:**

<img src="screenshots_p/image_17.png" alt="image_9" width="500" height="300" />

**m2:**

<img src="screenshots_p/image_18.png" alt="image_9" width="500" height="300" />

**Concated:**   
<img src="screenshots_p/image_19.png" alt="image_9" width="500" height="400" />

### Part 2

We started by converting column from string to **datetime objects** using pd.to_datetime(), enabling time-based operations.

We then created a new column **month_var** by extracting the **month abbreviation** from the date column with **.dt.strftime("%b").**

Next, we retrieved all distinct months present in the dataset using **.unique().**

<img src="screenshots_p/image_20.png" alt="image_20" width="400" height="150" />

*A dictionary comprehension was used twice:*

**First**, to map each month abbreviation to itself ({month: month for month in months}).

**Then**, to map each month abbreviation to a filtered DataFrame containing only rows for that month ({month: rf[rf["month_var"] == month] for month in months}).

This second dictionary was stored in **mn_frames**, allowing easy access to month-specific subsets (e.g., mn_frames["Jan"]):

<img src="screenshots_p/image_21.png" alt="image_21" width="400" height="150" />



We then concatenated multiple monthly DataFrames **("Jan", "Feb", and "Apr")** using pd.concat(..., ignore_index=True) and stored the result in concated.
From this combined DataFrame, we calculated the **frequency of each month** using .value_counts() on the month_var column and stored it in value23.

<img src="screenshots_p/image_22.png" alt="image_22" width="400" height="300" />

### Section 2 - Merge Method

We started by importing pandas and matplotlib.pyplot, then read two CSV files into DataFrames **m1** and **m2** using pd.read_csv().

**m1**:

<img src="screenshots_p/image_23.png" alt="image_23" width="600" height="150" />

**m2**:

<img src="screenshots_p/image_24.png" alt="image_24" width="600" height="150" />


We then merged them with **.merge(on="area")**, storing the result in merged1.

We performed an inner join on the area column, meaning that only rows with matching area values in both DataFrames were kept.
If m1 has multiple rows with the same area value, this merge produces multiple matching rows depending on how many identical values exist in m2.

Next, we removed unnecessary columns **("area", "job_title", "industry_type")** from merged1 using .drop(columns=...), resulting in merged2 — a DataFrame containing only numeric columns suitable for aggregation.


**merged2:**

<img src="screenshots_p/image_25.png" alt="image_25" width="700" height="150" />

We then computed two key variables:

**sum1 →** total sum of each numeric column in merged2 using .sum().

**sum2 →** top 5 column names with the largest sums, obtained by chaining .sum().head().index.to_list() (though here .head() simply takes the first 5 columns, not the highest sums).

We then selected only the columns in sum2 from merged2 using **merged2[sum2]:**

<img src="screenshots_p/image_26.png" alt="image_26" width="600" height="200" />

This subset was plotted as a line chart with **merged2[sum2].plot(kind="line")**


<img src="screenshots_p/image_27.png" alt="image_27" width="400" height="200" />


## Matplotlib 
### Histograms

We began by loading the dataset with pd.read_csv() and converting the "date" column to a **datetime** format using pd.to_datetime() to enable time-based operations. The **"skills_list"** column, which contained string representations of lists, was cleaned by applying ast.literal_eval() within .apply() so that each value became a proper Python list. 

 I then filtered the DataFrame to keep only rows where **"job_title"** equaled **"Statistician"**. Using the "mean_salary" column, I created a histogram with .plot(kind="hist", bins=10, edgecolor="black") to visualize the salary distribution.
<img src="screenshots_p/image_28.png" alt="image_28" width="450" height="200" />

The x-axis range was limited with **plt.xlim(0, 45000)**, and tick labels were formatted into thousands with a **“£K”** style using plt.FuncFormatter. Lastly, I set a descriptive title and customized axis labels to make the chart clearer.

<img src="screenshots_p/image_29.png" alt="image_29" width="400" height="300" />

### Charts


**Firstly**, we used rf["job_title"].value_counts() and rf["area"].value_counts().head(5) to count job titles and the top five areas.

<img src="screenshots_p/image_30.png" alt="image_30" width="450" height="200" />

Then, we created a figure with two subplots using **plt.subplots(1, 2)**, where fig represents the whole figure and ax is an array of individual plot axes. We plotted job titles on **ax[1** and areas on **ax[0]** with .plot(kind="bar", ax=...), and finally **adjusted spacing** with fig.tight_layout() for a cleaner layout.

<img src="screenshots_p/image_31.png" alt="image_31" width="400" height="300" />


### Pie charts

#### Section 1 

**Firstly**, we loaded and cleaned the dataset by converting date to datetime and parsing skills_list with ast.literal_eval(). Then, we used **value_counts()** on job_title to count **each role**, plotted the results as a **pie chart** with startangle=90 and percentage labels (autopct="%1.1f%%"), set a title, removed the y-axis label, and displayed the chart.

<img src="screenshots_p/image_32.png" alt="image_32" width="400" height="300" />

#### Section 2

We began by selecting the **three** relevant columns *(job_title, job__health_insurance, and work_from_home)* into a new DataFrame **rf_three**. Then, ywe created a subplot layout with one row and three columns using **plt.subplots(1,3)**, giving us three separate ax objects to work with. We defined a dictionary **dict1** to pair each column name with a descriptive chart title.

<img src="screenshots_p/image_33.png" alt="image_33" width="600" height="200" />

Using **a for loop** with enumerate(), we iterated over column–title pairs, used value_counts() to get **category counts**, and plotted them as pie charts with ax[i].pie(). We set a 90° start angle, displayed **percentages** with autopct, reduced label **font size** via textprops={'fontsize': 6}, added **category names** as labels, and set individual **subplot titles** with ax[i].set_title().

### Scatter Plot

#### Section 1 

We first created an **artificial dataset** as a Python dictionary and converted it into a pandas **DataFrame** named rf using pd.DataFrame().

<img src="screenshots_p/image_34.png" alt="image_34" width="300" height="300" />


Then, we plotted **a scatter plot** with rf.plot(kind="scatter", x="skill_count", y="job_skills") to visualize **the relationship between skill counts and their corresponding job skills**.

<img src="screenshots_p/image_35.png" alt="image_35" width="500" height="300" />

#### Section 2 

We **first** loaded the CSV file into the rf DataFrame.
Then, the date column was converted to a **datetime format** with pd.to_datetime(), and skills_list strings were converted to **Python lists** using ast.literal_eval() inside .apply().
We filtered the dataset to only include rows where job_title equals **"Statistician"** and used **.explode()** on skills_list so each skill had its own row.
Using **.groupby()** with .agg(), we created a new DataFrame that had **new_column** for the count of each skill and **stat_salary** for its median salary.

<img src="screenshots_p/image_36.png" alt="image_36" width="300" height="300" />

This **grouped DataFrame** was sorted in descending order of skill count with .sort_values().
We plotted **a scatter plot** with .plot(kind="scatter"), using new_column as the x-axis and stat_salary as the y-axis, then set axis labels and a title.

<img src="screenshots_p/image_37.png" alt="image_37" width="700" height="200" />

**Lastly**, we looped through the DataFrame index with **enumerate()** and used plt.text() to place each skill’s name next to its corresponding point in the chart.


<img src="screenshots_p/image_38.png" alt="image_37" width="500" height="350" />



### Box Plot

### Advanced Customization 

## Seaborn 

### Histogram 

### Box Plot




