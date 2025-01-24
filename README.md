# **Framework**
Hey there, welcome to my research on the data employment market, which focusses on data analyst positions in France. This project was started out of a desire to better navigate and understand the employment market. It focusses on the highest-paying and most in-demand talents to assist data analysts in finding the best career prospects.

The data used in my analysis comes from [Luke Barousse's Python Course](https://lukebarousse.com/python), which includes specific information on job titles, salaries, locations, and needed skills. Through a series of Python scripts, I investigate significant concepts such as the most sought-after talents, compensation patterns, and the link between the demand and salary regarding data analytics.

# **The Questions**
The following are the questions I hope to answer in my project.

* What skills are most in-demand for the top three most common data roles?
* What are the current in-demand skills for data analysts?
* How much do Data Analysts earn based on their occupations and skills?
* What are the best skills for data analysts to learn? (High Demand and High Paying)

# **Tools I Applied**
For my deep dive into the data analyst job market, I leveraged the capabilities of many essential technologies.

- **Python** is the foundation of my analysis, allowing me to analyse data and discover key insights.I used the following Python libraries:
     - **Pandas Library:** This tool was used to analyse the data.
     - **Matplotlib library:** Used to visualise the data.
     - **Seaborn Library:** Enabled me to develop more complex visualisations.
- **Jupyter Notebooks** was the tool I used to execute my Python programs, allowing me to quickly include my notes and analysis.
- **Visual Studio Code** is my go-to for running Python scripts.
- **Git and GitHub** are essential for version control and sharing my Python code and analysis, as well as for collaboration and project management.

# **Collection of Data and Cleanup**
This section describes the methods used to gather the data for analysis while assuring precision and usability.

## Import and clean up data.
I began by importing the relevant libraries and loading the dataset, then do initial data cleaning procedures to guarantee data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

# Filtering France Jobs
To focus my study on the French employment market, I add filters to the dataset, constraining it to occupations based in France.
```python
df_DA_FR = df[df['job_country'] == 'France']
```

### Filtering Data Analyst Roles in France
To narrow my study to data analyst roles in the French labour market, I apply filters to the dataset, limiting it to data analyst job posts in France.
```python
df_DA_FR = df[(df['job_country'] == 'France') & (df['job_title_short'] == 'Data Analyst')]
```
#### Data Visualization
```Python
df_plot = df_DA_FR['job_location'].value_counts().head(15).to_frame()

sns.set_theme(style='ticks')
sns.barplot(data=df_plot, x='count', y='job_location', hue='count', palette='dark:blue_r', legend=False)
sns.despine()
plt.title('Job Locations Counts for Data Analyst Jobs in France')
plt.xlabel('Number of Jobs')
plt.ylabel('')
plt.show()
```
#### Results

![Job Locations Counts for Data Analyst Jobs in France](Images/Job%20Locations%20Counts%20for%20Data%20Analyst%20Jobs%20in%20France.png)
*Bar graph visualizing the Job Locations Counts for Data Analyst Jobs in France*

### Top Hiring Companies
This is the represention of the major companies in France that commonly hire people for the role of data analysts.

#### Data Visualization
```Python
df_plot = df_DA_FR['company_name'].value_counts().head(15).to_frame()

sns.set_theme(style='ticks')
sns.barplot(data=df_plot, x='count', y='company_name', hue='count', palette='dark:blue_r', legend=False)
sns.despine()
plt.title('Counts of Companies Postings for Data Analyst in the France')
plt.xlabel('Number of Jobs')
plt.ylabel('')
plt.show()
```
#### Results

![Top Hiring Companies](Images/Top%20Hiring%20Companies.png)
*Bar graph visualizing the Top Hiring Companies*

# **The Process of Analysis**
Each Jupyter notebook for this study focused on a specific facet of the data job market. Here's how I addressed each question:

## 1. What skills are most in-demand for the top three most common data roles?
To determine the most demanded skills for the top three most common data roles. I narrowed down the positions based on popularity and identified the top five talents for the top three roles. This query displays the most popular job titles and their top skills, indicating which skills I should prioritise depending on the role I'm pursuing.

View my notebook with detailed steps here: [Skill_Demand](Skills_Demand.ipynb).

#### Data Visualization
```Python
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_percent[df_skills_percent['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:blue_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)
    # remove the x-axis tick labels for better readability and only keep the last one
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    # label the percentage on the bars
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Skills Requested in France Job Postings', fontsize=15)
fig.tight_layout(h_pad=.8)
plt.show()
```
#### Results
![Likelihood of Skills Requested in the France Job Postings](Images/Likelihood%20of%20Skills%20Requested%20in%20the%20France%20Job%20Postings.png)
*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### **Insights**

**Data Analysts:**

* SQL (45%) and Python (33%) are the most in-demand skills.
* Tools like Power BI (25%), Tableau (21%), and Excel (21%) are also frequently requested, highlighting the need for proficiency in data visualization and spreadsheet tools. 
* This reflects a priority on data visualization and business-oriented tasks.

**Data Engineer:**

* Python (57%) and SQL (49%) dominate the skill requirements.
* Cloud platforms and big data tools such as Spark (35%), AWS (27%), and Azure (26%) are also significant, emphasizing the importance of cloud-based and distributed computing skills for this role.

**Data Scientist:**

* Python (67%) is the most requested skill by far, reflecting its role as the primary programming language in data science.
* SQL (33%) and R (23%) are important for database management and statistical analysis.
* Other tools like SAS (18%) and Spark (12%) highlight the need for experience with specific statistical and big data frameworks.

Python is universally in high demand across all three roles, reflecting its versatility in the data domain. Also , SQL remains critical for both accessing and managing data.


## 2. What are the current in-demand skills for data analysts?

To determine how skills are evolving in 2023 for Data Analysts, I selected data analyst roles and grouped the skills by the month of job posting. This gave me the top five data analyst skills per month, demonstrating how popular these skills were in 2023.

View my notebook with detailed steps here: [Skill_Trend](Skills_Trend.ipynb).

#### Data Visualization
```Python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_FR_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove top and right spines

plt.title('Top Trending Skills for Data Analysts in the France')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

# annotate the plot with the top 5 skills using plt.text()
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.show()
```

#### Results

![Top Trending Skills for Data Analysts in France](images/Top%20Trending%20Skills%20for%20Data%20Analysts%20in%20France.png)  
*Line chart visualizing the top trending skills for data analysts in France in 2023.*

### **Insights**

* SQL and Python are essential skills to focus on, as they consistently show strong demand throughout the year.

* Gaining expertise in at least one BI tool (Power BI or Tableau) is recommended to stand out in business-centric roles.

* Excel remains relevant, so maintaining proficiency in it can be a useful complementary skill.


## 3. How much do Data Analysts earn based on their occupations and skills?

To determine the highest-paying professions and skills, I solely considered jobs in France and their median salaries. But first, I looked into the income distributions for common data jobs such as Data Scientist, Data Engineer, and Data Analyst to get a sense of which ones pay the most. 

View my notebook with detailed steps here: [Salary_Analysis](Salary_Analysis.ipynb).

#### Data Visualization
```Python
sns.boxplot(data=df_FR_top5, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

plt.title('Salary Distributions of Data Jobs in the France')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

#### Results

![Salary Distributions of Data Jobs in France](images/Salary%20Distributions%20of%20Data%20Jobs%20in%20the%20France.png) 
*Box plot visualizing the salary distributions for the top 5 data job titles.*

### **Insights**

* Senior Positions Pay Significantly More. Senior Data Engineers earn significantly more than other roles, reflecting the high demand for leadership, expertise, and experience.

* Specialized Roles Like Machine Learning Engineer and Data Engineer Are Lucrative. These roles have a wider salary range, indicating opportunities for both entry-level professionals and highly experienced experts.

* Data Analysts Have the Lowest Pay. This reflects the role’s relatively lower technical complexity compared to others like Data Scientists or Machine Learning Engineers.

### Highest Paid & Most Demanded Skills for Data Analysts

Subsequently, I concentrated my analysis to solely data analyst positions. I investigated the highest-paid and most in-demand skills. I used two bar charts to demonstrate these.

#### Data Visualization
```Python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:blue_r')
ax[0].legend().remove()
ax[0].set_title('Highest Paid Skills for Data Analysts in the France')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:blue')
ax[1].legend().remove()
ax[1].set_title('Most In-Demand Skills for Data Analysts in the France')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.show()
```

#### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the France:

![The Highest Paid & Most In-Demand Skills for Data Analysts in France](images/Most%20In-Demand%20Skills%20for%20Data%20Analysts%20in%20the%20France.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in France.*

### **Insights**
* Specialized skills like C, GitLab, and Terraform command the highest salaries but may not be as widely in demand as foundational tools like Excel, Python, and Power BI.

* These high-paying skills are likely tied to specific industries or advanced roles within data analysis.

* In-demand skills tend to focus on accessibility and versatility, such as Python and SQL, which are used across a wide range of industries and projects.

## 4. What are the best skills for data analysts to learn?

To determine the most ideal skills to learn (those that are the highest paid and in demand), I estimated the percentage of skill demand and the median salary for these skills. To easily identify the most relevant skills to pursue. 

View my notebook with detailed steps here: [Best_Skills](Best_Skills.ipynb).

#### Data Visualization
```Python
from adjustText import adjust_text     # Importing the adjust_text library

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')  # Assuming this is the label you want for y-axis
plt.title('Best Skills for Data Analysts in France')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Example formatting y-axis

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.show()
```
#### Results

![Best Skills for Data Analysts in France](images/Best%20Skill.png)    
*A scatter plot visualizing the best skills (high paying & high demand) for data analysts in France.*

### **Insights**

* SQL and Python stand out as critical skills that every data analyst should master. Their combination opens opportunities in data manipulation, automation, and advanced analytics.

* Power BI and Tableau are essential for data visualization but not as highly compensated as technical programming skills like Python or SQL. However, they can enhance a candidate's profile for business-centric roles.

* Oracle, SQL Server, and Go offer high salaries despite lower demand, indicating niche markets where expertise in these tools is highly valued.

* Tools like Excel, PowerPoint, and Word remain fundamental for daily tasks but are not differentiators for higher pay.

#### Data Visualization by Technology
```Python
sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')

# Prepare texts for adjustText
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))

# Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

# Set axis labels, title, and legend
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary')
plt.title('Best Skills for Data Analysts in France')
plt.legend(title='Technology')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
# ax.get_legend().remove()


# Adjust layout and display plot 
plt.tight_layout()
plt.show()
```

#### Results

![Best Skills for Data Analysts in France with Coloring by Technology](images/Best%20Skill%20by%20technology.png)  
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in France with color labels for technology.*

### **Insights**

* Programming is indispensable for automating workflows, advanced analytics, and machine learning tasks, making it a key area for career growth. Programming skills, especially Python, are critical for technical roles in data analysis, offering one of the highest salaries.

* Database skills are foundational for all data-related positions, making them universally relevant and a priority for professionals.

* Analyst tools are widely used for reporting and visualization, with high demand but moderate pay. Basic tools like PowerPoint and Word are necessary for reporting and presentations but are not directly tied to technical or analytical expertise.

* Cloud-related tools like Oracle are more specialized, and professionals with expertise in them are compensated accordingly due to their scarcity.


# **Project Challenges**

- **Data Irregularities:** Addressing missing or inconsistent data required meticulous attention and robust data-cleaning methods to preserve the reliability of the analysis.

- **Navigating Breadth and Depth:** Striking the right balance between detailed analysis and maintaining a wide-ranging perspective was crucial to provide thorough coverage without becoming overly focused on specifics.

- **Challenging Data Visualization:** Creating clear and impactful visualizations for complex datasets posed difficulties but was essential for effectively communicating insights.

# **Conclusion**

In conclusion, the analysis highlights the importance of tailoring skills to meet the evolving demands of data analyst roles in France. Programming languages like Python and SQL remain indispensable, combining high demand with competitive salaries. Visualization tools such as Power BI and Tableau are critical for effectively communicating insights, while foundational tools like Excel retain their relevance in day-to-day tasks. Additionally, niche skills in cloud platforms and advanced databases, like Oracle and Go, offer opportunities for specialization and higher earning potential. To succeed in this dynamic field, professionals should aim to build a balanced skill set that integrates technical expertise, data management, and visualization capabilities, ensuring both versatility and value in the job market.
