```python
# Importing dependencies
import pandas as pd

#Giving file paths
Schools = "Resources/schools_complete.csv"
Students = "Resources/students_complete.csv"

# reading schools and students data file using pandas dataframe
schools_df = pd.read_csv(Schools)
# schools_df.head()


```


```python
#Reading students data in dataframe
students_df = pd.read_csv(Students)
# students_df.head()
```

# District summary


```python
# combining the two dataframes into a single one
schools_complete_df = pd.merge(students_df, schools_df, how = "left", on = "school_name")
# schools_complete_df.head()
```


```python
#calculating total schools
total_schools = len(schools_complete_df["school_name"].unique())
# total_schools

```


```python
#calculating total students
total_students = len(schools_complete_df["student_name"])
# total_students
```


```python
#calculating total budget

#defining new dataframe for unique schools name [note this can also be done by using drop_duplicate function]
unique_schools_df = pd.DataFrame(schools_complete_df.groupby("school_name",as_index=False).first())
# unique_schools_df.head()
```


```python
# calculating total budget 
total_budget = unique_schools_df["budget"].sum()
# total_budget
```


```python
#calculating average math score
average_math_score = schools_complete_df["math_score"].sum()/total_students
# average_math_score
```


```python
#calculating average reading score
average_reading_score = schools_complete_df["reading_score"].sum()/total_students
# average_reading_score

```


```python
# calculating passing math
math_pass_df = pd.DataFrame(schools_complete_df.loc[schools_complete_df.math_score >= 70])
# math_pass_df.head()
```


```python
#calculating % passing math
total_passing_math = len(math_pass_df["student_name"])
percent_passing_math = 100*(total_passing_math/total_students)
# percent_passing_math
```


```python
# calculating % passing reading
reading_pass_df = pd.DataFrame(schools_complete_df.loc[schools_complete_df.reading_score >= 70])
total_passing_reading = len(reading_pass_df["student_name"])
percent_passing_reading = 100*(total_passing_reading/total_students)
# percent_passing_reading
```


```python
# calculating % passing students
total_pass_df = pd.DataFrame(schools_complete_df.loc[(schools_complete_df.math_score >=70) & 
                                                     (schools_complete_df.reading_score >=70)])
total_pass = len(total_pass_df["student_name"])
percent_pass = 100*(total_pass/total_students)
# percent_pass
```


```python
### district summary
district_summary = pd.DataFrame({"total schools": total_schools, "Total students": total_students, "Total Budget": total_budget,
                                "Average Math Score": average_math_score, "Average Reading Score": average_reading_score, 
                                 "Percentage Passing math": percent_passing_math, 
                                 "Percent Passing Reading": percent_passing_reading, "Total Pass Percent": percent_pass},index=[0])
district_summary

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total schools</th>
      <th>Total students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percentage Passing math</th>
      <th>Percent Passing Reading</th>
      <th>Total Pass Percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>24649428</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>74.980853</td>
      <td>85.805463</td>
      <td>65.172326</td>
    </tr>
  </tbody>
</table>
</div>



# School summary


```python
#creating school summary dataframa
school_name = unique_schools_df["school_name"]
school_type = unique_schools_df["type"]
total_school_students = unique_schools_df["size"]
total_school_budget = unique_schools_df["budget"]
per_student_budget = total_school_budget/total_school_students
average_math_score = pd.DataFrame(schools_complete_df.groupby("school_name",as_index=False)["math_score"].mean())
average_reading_score = pd.DataFrame(schools_complete_df.groupby("school_name",as_index=False)["reading_score"].mean())

```


```python
# average_math_score
```


```python
# calculating math pass percentage for each school
math_pass_school = math_pass_df.groupby("school_name", as_index = False)["Student ID"].count()
#math_pass_school
percentage_math_pass_school = 100*(math_pass_school["Student ID"]/total_school_students)
```


```python
# calculating reading pass percentage for each school
reading_pass_school = reading_pass_df.groupby("school_name", as_index = False)["Student ID"].count()
#reading_pass_school
percentage_reading_pass_school = 100*(reading_pass_school["Student ID"]/total_school_students)
```


```python
#calculating overall passing students percentage
total_pass_school = total_pass_df.groupby("school_name", as_index = False)["Student ID"].count()
#total_pass_school
percentage_total_pass_school = 100*(total_pass_school["Student ID"]/total_school_students)
```


```python

```


```python

```


```python
#creating summary table
#selecting columns from unique_schools_df
column_list = ["school_name","type","size","budget"]
school_summary_df = pd.DataFrame(unique_schools_df[column_list])
# school_summary_df.head()
```


```python
#adding more columns
school_summary_df["Per Student Budget"] = school_summary_df["budget"]/school_summary_df["size"]
school_summary_df["Average Math Score"] = average_math_score["math_score"]
school_summary_df["Average Reading Score"] = average_reading_score["reading_score"]
school_summary_df["% Passing Math"] = percentage_math_pass_school
school_summary_df["% Passing Reading"] = percentage_reading_pass_school
school_summary_df["% Overall Passing"] = percentage_total_pass_school
# school_summary_df.head()
```


```python
school_summary_df = school_summary_df.rename(columns = {"school_name":"School Name", "type": "School Type", 
                                                        "size":"Total Students", "budget":"Total Budget"})
school_summary_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>628.0</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>54.642283</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>91.334769</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>53.204476</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>54.289887</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>90.599455</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

# Highest performing schools (by % overall passing)


```python
# sorting school sumamry by % overall passing
school_summary_sorted_df = school_summary_df.sort_values(by = "% Overall Passing", ascending = False)
highest_performing_df = pd.DataFrame(school_summary_sorted_df.iloc[0:5,:])

highest_performing_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>91.334769</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>90.948012</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>90.599455</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>90.582567</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>90.540541</td>
    </tr>
  </tbody>
</table>
</div>



# Lowest performing schools (by % overall passing)


```python
# sorting school sumamry by % overall passing
school_summary_sorted_df = school_summary_df.sort_values(by = "% Overall Passing", ascending = True)
lowest_performing_df = pd.DataFrame(school_summary_sorted_df.iloc[0:5,:])

lowest_performing_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>52.988247</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>53.204476</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>53.513884</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>53.527508</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>53.539172</td>
    </tr>
  </tbody>
</table>
</div>



# calculating math score by grade


```python
ninth_grade_df = pd.DataFrame(schools_complete_df.loc[schools_complete_df["grade"]=="9th",:].groupby("school_name").mean())
tenth_grade_df = pd.DataFrame(schools_complete_df.loc[schools_complete_df["grade"]=="10th",:].groupby("school_name").mean())
eleventh_grade_df = pd.DataFrame(schools_complete_df.loc[schools_complete_df["grade"]=="11th",:].groupby("school_name").mean())
twelvth_grade_df = pd.DataFrame(schools_complete_df.loc[schools_complete_df["grade"]=="12th",:].groupby("school_name").mean())

math_score_df = ninth_grade_df.drop(["Student ID","reading_score","School ID","size","budget"], axis = 1)
math_score_df=math_score_df.rename(columns = {"math_score": "9th grade"})
math_score_df["10th grade"]= tenth_grade_df["math_score"]
math_score_df["11th grade"]= eleventh_grade_df["math_score"]
math_score_df["12th grade"]= twelvth_grade_df["math_score"]

math_score_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th grade</th>
      <th>10th grade</th>
      <th>11th grade</th>
      <th>12th grade</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
  </tbody>
</table>
</div>



# calculating Reading score by grade


```python
reading_score_df = ninth_grade_df.drop(["Student ID","math_score","School ID","size","budget"], axis = 1)
reading_score_df=reading_score_df.rename(columns = {"reading_score": "9th grade"})
reading_score_df["10th grade"]= tenth_grade_df["reading_score"]
reading_score_df["11th grade"]= eleventh_grade_df["reading_score"]
reading_score_df["12th grade"]= twelvth_grade_df["reading_score"]

reading_score_df.head()



```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th grade</th>
      <th>10th grade</th>
      <th>11th grade</th>
      <th>12th grade</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
  </tbody>
</table>
</div>



# scores by school spending


```python
# print(school_summary_df["Per Student Budget"].max())
# print(school_summary_df["Per Student Budget"].min())
```


```python
#defining bins to divide the spending 
bins = [0, 585,605, 635,660]
group_labels = ["< 585", "585 - 605", "605-635","635-660" ]
school_spending_df = school_summary_df[["Per Student Budget","Average Math Score", "Average Reading Score","% Passing Math", 
                                        "% Passing Reading","% Overall Passing"]]
spending_cut = pd.cut(school_spending_df["Per Student Budget"],bins, labels=group_labels)
school_spending_df["Per Student spending"] = spending_cut
spending_group = school_spending_df.groupby(["Per Student spending"]).mean()
spending_group_df = spending_group.drop(["Per Student Budget"], axis = 1)
spending_group_df.head()
```

    <ipython-input-28-8d997a52cc19>:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      school_spending_df["Per Student spending"] = spending_cut
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>Per Student spending</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 585</th>
      <td>83.455399</td>
      <td>83.933814</td>
      <td>93.460096</td>
      <td>96.610877</td>
      <td>90.369459</td>
    </tr>
    <tr>
      <th>585 - 605</th>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>89.892107</td>
    </tr>
    <tr>
      <th>605-635</th>
      <td>81.413283</td>
      <td>82.965140</td>
      <td>84.889010</td>
      <td>91.672730</td>
      <td>78.594093</td>
    </tr>
    <tr>
      <th>635-660</th>
      <td>77.866721</td>
      <td>81.368774</td>
      <td>70.347325</td>
      <td>82.995575</td>
      <td>58.858741</td>
    </tr>
  </tbody>
</table>
</div>



# score by school size


```python
# #defining bins based on school size
# print(school_summary_df["Total Students"].max())
# print(school_summary_df["Total Students"].min())
```


```python
bins_size = [0, 1000,3000,6000]
group_labels_size = ["small (<1000)", "medium (1000-3000)","large (>3000)"]
school_size_df = school_summary_df[["Total Students","Average Math Score", "Average Reading Score","% Passing Math", 
                                        "% Passing Reading","% Overall Passing"]]
size_cut = pd.cut(school_size_df["Total Students"],bins_size, labels=group_labels_size)
school_size_df["Student Size"] = size_cut
size_group = school_size_df.groupby(["Student Size"]).mean()
size_group_df = size_group.drop(["Total Students"], axis = 1)
size_group_df.head()

```

    <ipython-input-30-06dfc7ca9c1e>:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      school_size_df["Student Size"] = size_cut
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>Student Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>small (&lt;1000)</th>
      <td>83.821598</td>
      <td>83.929843</td>
      <td>93.550225</td>
      <td>96.099437</td>
      <td>89.883853</td>
    </tr>
    <tr>
      <th>medium (1000-3000)</th>
      <td>81.176821</td>
      <td>82.933187</td>
      <td>84.649798</td>
      <td>91.316412</td>
      <td>78.299832</td>
    </tr>
    <tr>
      <th>large (&gt;3000)</th>
      <td>77.063340</td>
      <td>80.919864</td>
      <td>66.464293</td>
      <td>81.059691</td>
      <td>53.674303</td>
    </tr>
  </tbody>
</table>
</div>



# Score by school type



```python
# grouping by school type
school_type_df = school_summary_df[["School Type","Average Math Score", "Average Reading Score","% Passing Math", 
                                        "% Passing Reading","% Overall Passing"]]

type_group_df = school_type_df.groupby(["School Type"]).mean()

type_group_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>93.620830</td>
      <td>96.586489</td>
      <td>90.432244</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>66.548453</td>
      <td>80.799062</td>
      <td>53.672208</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Printing results
print("District summary")
print(district_summary.to_markdown)
print("--------------------------------------------")
print("school Summary")
print(school_summary_df.to_markdown)
print("-------------------------------------------")
print("Highest Performing Schools")
print(highest_performing_df.to_markdown)
print("-------------------------------------------")
print("Lowest Performing Schools")
print(lowest_performing_df.to_markdown)

print("-------------------------------------------")

print("Math Score")
print(math_score_df.to_markdown)

print("-------------------------------------------")
print("Reading Score")
print(reading_score_df.to_markdown)

print("-------------------------------------------")

Print("Spending based grouping")
print(spending_group_df.to_markdown)

print("-------------------------------------------")
print("Size based grouping")
print(size_group_df.to_markdown)

print("-------------------------------------------")
print("Type based grouping")
print(type_group_df.to_markdown)
```

    District summary
    <bound method DataFrame.to_markdown of    total schools  Total students  Total Budget  Average Math Score  \
    0             15           39170      24649428           78.985371   
    
       Average Reading Score  Percentage Passing math  Percent Passing Reading  \
    0               81.87784                74.980853                85.805463   
    
       Total Pass Percent  
    0           65.172326  >
    --------------------------------------------
    school Summary
    <bound method DataFrame.to_markdown of               School Name School Type  Total Students  Total Budget  \
    0      Bailey High School    District            4976       3124928   
    1     Cabrera High School     Charter            1858       1081356   
    2    Figueroa High School    District            2949       1884411   
    3        Ford High School    District            2739       1763916   
    4     Griffin High School     Charter            1468        917500   
    5   Hernandez High School    District            4635       3022020   
    6      Holden High School     Charter             427        248087   
    7       Huang High School    District            2917       1910635   
    8     Johnson High School    District            4761       3094650   
    9        Pena High School     Charter             962        585858   
    10  Rodriguez High School    District            3999       2547363   
    11    Shelton High School     Charter            1761       1056600   
    12     Thomas High School     Charter            1635       1043130   
    13     Wilson High School     Charter            2283       1319574   
    14     Wright High School     Charter            1800       1049400   
    
        Per Student Budget  Average Math Score  Average Reading Score  \
    0                628.0           77.048432              81.033963   
    1                582.0           83.061895              83.975780   
    2                639.0           76.711767              81.158020   
    3                644.0           77.102592              80.746258   
    4                625.0           83.351499              83.816757   
    5                652.0           77.289752              80.934412   
    6                581.0           83.803279              83.814988   
    7                655.0           76.629414              81.182722   
    8                650.0           77.072464              80.966394   
    9                609.0           83.839917              84.044699   
    10               637.0           76.842711              80.744686   
    11               600.0           83.359455              83.725724   
    12               638.0           83.418349              83.848930   
    13               578.0           83.274201              83.989488   
    14               583.0           83.682222              83.955000   
    
        % Passing Math  % Passing Reading  % Overall Passing  
    0        66.680064          81.933280          54.642283  
    1        94.133477          97.039828          91.334769  
    2        65.988471          80.739234          53.204476  
    3        68.309602          79.299014          54.289887  
    4        93.392371          97.138965          90.599455  
    5        66.752967          80.862999          53.527508  
    6        92.505855          96.252927          89.227166  
    7        65.683922          81.316421          53.513884  
    8        66.057551          81.222432          53.539172  
    9        94.594595          95.945946          90.540541  
    10       66.366592          80.220055          52.988247  
    11       93.867121          95.854628          89.892107  
    12       93.272171          97.308869          90.948012  
    13       93.867718          96.539641          90.582567  
    14       93.333333          96.611111          90.333333  >
    -------------------------------------------
    Highest Performing Schools
    <bound method DataFrame.to_markdown of             School Name School Type  Total Students  Total Budget  \
    1   Cabrera High School     Charter            1858       1081356   
    12   Thomas High School     Charter            1635       1043130   
    4   Griffin High School     Charter            1468        917500   
    13   Wilson High School     Charter            2283       1319574   
    9      Pena High School     Charter             962        585858   
    
        Per Student Budget  Average Math Score  Average Reading Score  \
    1                582.0           83.061895              83.975780   
    12               638.0           83.418349              83.848930   
    4                625.0           83.351499              83.816757   
    13               578.0           83.274201              83.989488   
    9                609.0           83.839917              84.044699   
    
        % Passing Math  % Passing Reading  % Overall Passing  
    1        94.133477          97.039828          91.334769  
    12       93.272171          97.308869          90.948012  
    4        93.392371          97.138965          90.599455  
    13       93.867718          96.539641          90.582567  
    9        94.594595          95.945946          90.540541  >
    -------------------------------------------
    Lowest Performing Schools
    <bound method DataFrame.to_markdown of               School Name School Type  Total Students  Total Budget  \
    10  Rodriguez High School    District            3999       2547363   
    2    Figueroa High School    District            2949       1884411   
    7       Huang High School    District            2917       1910635   
    5   Hernandez High School    District            4635       3022020   
    8     Johnson High School    District            4761       3094650   
    
        Per Student Budget  Average Math Score  Average Reading Score  \
    10               637.0           76.842711              80.744686   
    2                639.0           76.711767              81.158020   
    7                655.0           76.629414              81.182722   
    5                652.0           77.289752              80.934412   
    8                650.0           77.072464              80.966394   
    
        % Passing Math  % Passing Reading  % Overall Passing  
    10       66.366592          80.220055          52.988247  
    2        65.988471          80.739234          53.204476  
    7        65.683922          81.316421          53.513884  
    5        66.752967          80.862999          53.527508  
    8        66.057551          81.222432          53.539172  >
    -------------------------------------------
    Math Score
    <bound method DataFrame.to_markdown of                        9th grade  10th grade  11th grade  12th grade
    school_name                                                         
    Bailey High School     77.083676   76.996772   77.515588   76.492218
    Cabrera High School    83.094697   83.154506   82.765560   83.277487
    Figueroa High School   76.403037   76.539974   76.884344   77.151369
    Ford High School       77.361345   77.672316   76.918058   76.179963
    Griffin High School    82.044010   84.229064   83.842105   83.356164
    Hernandez High School  77.438495   77.337408   77.136029   77.186567
    Holden High School     83.787402   83.429825   85.000000   82.855422
    Huang High School      77.027251   75.908735   76.446602   77.225641
    Johnson High School    77.187857   76.691117   77.491653   76.863248
    Pena High School       83.625455   83.372000   84.328125   84.121547
    Rodriguez High School  76.859966   76.612500   76.395626   77.690748
    Shelton High School    83.420755   82.917411   83.383495   83.778976
    Thomas High School     83.590022   83.087886   83.498795   83.497041
    Wilson High School     83.085578   83.724422   83.195326   83.035794
    Wright High School     83.264706   84.010288   83.836782   83.644986>
    -------------------------------------------
    Reading Score
    <bound method DataFrame.to_markdown of                        9th grade  10th grade  11th grade  12th grade
    school_name                                                         
    Bailey High School     81.303155   80.907183   80.945643   80.912451
    Cabrera High School    83.676136   84.253219   83.788382   84.287958
    Figueroa High School   81.198598   81.408912   80.640339   81.384863
    Ford High School       80.632653   81.262712   80.403642   80.662338
    Griffin High School    83.369193   83.706897   84.288089   84.013699
    Hernandez High School  80.866860   80.660147   81.396140   80.857143
    Holden High School     83.677165   83.324561   83.815534   84.698795
    Huang High School      81.290284   81.512386   81.417476   80.305983
    Johnson High School    81.260714   80.773431   80.616027   81.227564
    Pena High School       83.807273   83.612000   84.335938   84.591160
    Rodriguez High School  80.993127   80.629808   80.864811   80.376426
    Shelton High School    84.122642   83.441964   84.373786   82.781671
    Thomas High School     83.728850   84.254157   83.585542   83.831361
    Wilson High School     83.939778   84.021452   83.764608   84.317673
    Wright High School     83.833333   83.812757   84.156322   84.073171>
    -------------------------------------------
    


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-32-e0d83c32e7fa> in <module>
         23 print("-------------------------------------------")
         24 
    ---> 25 Print("Spending based grouping")
         26 print(spending_group_df.to_markdown)
         27 
    

    NameError: name 'Print' is not defined


# Conclusions


```python
# Based on the data above, follwoing points can be made: 

print("1. Higher per student spending does not necessarily result into better performance by schools.")
print("2. The students in the charter schools tend to have better performance than the students in District schools.")
print("3. The schools with smaller no of students tend to hvae better results in terms of overall passing. However,")
print("    this may be correlated to the possible difference in student number in charter and district type schools. ")
print("     Additional analysis comparing average number of students in district and charter type schools can clearify this.")
```
