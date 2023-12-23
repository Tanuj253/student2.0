---
layout: post
title: Python Pandas
description: This blog contains basic techniques for analyzing a dataset using the Python Pandas module.  Python is a primary language for data science and is useful for backend operations like analyzing and storing data.
courses: {'csp': {'week': 8}}
categories: ['C4.0']
type: ccc
author: Vardaan Sinha
---

# An Introduction

Interacting with data is something we do everyday, whether consciously or subconsciously. 

If you are interested into going into any field of STEM - whether it be the medical field, data science, or computer science, you will often be working with *large* datasets with tons of useful AND useless information. 

A skillset that is becoming increasingly important in these areas of work is the ability to effectively query and filter for data in large datasets and draw conclusions based on these filters. 

# Pandas

Pandas is a Python library that allows for the manipulation, querying, and filtering of data. 

Over time, it has become one of the most popular Python libraries for data analysis.

Here is the documentation link: https://pandas.pydata.org/docs/



# Our Data

Data is readable in many formats. As someone who is working with datasets, you should be able to recognize what formats are easiest to understand for you and for any program that you write. Here are two of the most common data formats:

1. **JSON**: This is a standard file format that is very easy for humans and computers to read and write. It is a compact way to store data.

2. **CSV**: These are *comma-separated value* files. This is where data has comma delimiters (separaters). 


For the purpose of this notebook, we'll use a **JSON file** containing data about the average income level in each of the 50 states in the USA (located in /assets/datasets/income.json)

We will look to first understand and interpret the data ourselves and use Pandas and Numpy to provide insightful statistical information about the dataset that we may not be as easy to find by ourselves.


```python
import pandas as pd 

df = pd.read_json('files/income.json')

# This defines the dataframe in question. In this case, it is reading from the JSON file (hence read_json) income.json. 

# It is very important that the relative file path is correct, otherwise it won't be reading the file that you want it to.

display(df)

# This is now displaying the readable JSON data.


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
      <th>State</th>
      <th>MeanHouseholdIncome</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>71964</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>98811</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>84380</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>69357</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>111622</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Colorado</td>
      <td>100933</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Connecticut</td>
      <td>115337</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Delaware</td>
      <td>92308</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Florida</td>
      <td>83104</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Georgia</td>
      <td>85961</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hawaii</td>
      <td>107348</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Idaho</td>
      <td>77399</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Illinois</td>
      <td>95115</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Indiana</td>
      <td>76984</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Iowa</td>
      <td>80316</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Kansas</td>
      <td>82103</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Kentucky</td>
      <td>72318</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Louisiana</td>
      <td>73759</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Maine</td>
      <td>78301</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Maryland</td>
      <td>114236</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Massachusetts</td>
      <td>115964</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Michigan</td>
      <td>80803</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Minnesota</td>
      <td>96814</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Mississippi</td>
      <td>65156</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Missouri</td>
      <td>78194</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Montana</td>
      <td>76834</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Nebraska</td>
      <td>82306</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Nevada</td>
      <td>84350</td>
    </tr>
    <tr>
      <th>28</th>
      <td>New Hampshire</td>
      <td>101292</td>
    </tr>
    <tr>
      <th>29</th>
      <td>New Jersey</td>
      <td>117868</td>
    </tr>
    <tr>
      <th>30</th>
      <td>New Mexico</td>
      <td>70241</td>
    </tr>
    <tr>
      <th>31</th>
      <td>New York</td>
      <td>105304</td>
    </tr>
    <tr>
      <th>32</th>
      <td>North Carolina</td>
      <td>79620</td>
    </tr>
    <tr>
      <th>33</th>
      <td>North Dakota</td>
      <td>85506</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Ohio</td>
      <td>78796</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Oklahoma</td>
      <td>74195</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Oregon</td>
      <td>88137</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Pennsylvania</td>
      <td>87262</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Rhode Island</td>
      <td>92427</td>
    </tr>
    <tr>
      <th>39</th>
      <td>South Carolina</td>
      <td>76390</td>
    </tr>
    <tr>
      <th>40</th>
      <td>South Dakota</td>
      <td>77932</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Tennessee</td>
      <td>76937</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Texas</td>
      <td>89506</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Utah</td>
      <td>94452</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Vermont</td>
      <td>83767</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Virginia</td>
      <td>106023</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Washington</td>
      <td>103669</td>
    </tr>
    <tr>
      <th>47</th>
      <td>West Virginia</td>
      <td>65332</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Wisconsin</td>
      <td>82757</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Wyoming</td>
      <td>83583</td>
    </tr>
  </tbody>
</table>
</div>


## Dataset statistics
> Let's find and display some statistics from the dataset..


```python
dfmean = df["MeanHouseholdIncome"].mean()

# Defines dfmean as using the "mean" operation (finds average) of the dataframe in question.

# A label is given to make sure that the user knows what is being computed.

print("Mean Household Income: $" + str(dfmean))

# The dfmean value is converted into a string format so that there is no space between the dollar sign and the mean value.

```

    Mean Household Income: $87461.46


### Dataframe sort, Household Income
> In this example, analytical data is sorted.


```python
df.sort_values(by="MeanHouseholdIncome")

# 50 states are being sorted based on the "MeanHouseholdIncome" column in ascending order
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
      <th>State</th>
      <th>MeanHouseholdIncome</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>Mississippi</td>
      <td>65156</td>
    </tr>
    <tr>
      <th>47</th>
      <td>West Virginia</td>
      <td>65332</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>69357</td>
    </tr>
    <tr>
      <th>30</th>
      <td>New Mexico</td>
      <td>70241</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>71964</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Kentucky</td>
      <td>72318</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Louisiana</td>
      <td>73759</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Oklahoma</td>
      <td>74195</td>
    </tr>
    <tr>
      <th>39</th>
      <td>South Carolina</td>
      <td>76390</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Montana</td>
      <td>76834</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Tennessee</td>
      <td>76937</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Indiana</td>
      <td>76984</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Idaho</td>
      <td>77399</td>
    </tr>
    <tr>
      <th>40</th>
      <td>South Dakota</td>
      <td>77932</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Missouri</td>
      <td>78194</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Maine</td>
      <td>78301</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Ohio</td>
      <td>78796</td>
    </tr>
    <tr>
      <th>32</th>
      <td>North Carolina</td>
      <td>79620</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Iowa</td>
      <td>80316</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Michigan</td>
      <td>80803</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Kansas</td>
      <td>82103</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Nebraska</td>
      <td>82306</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Wisconsin</td>
      <td>82757</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Florida</td>
      <td>83104</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Wyoming</td>
      <td>83583</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Vermont</td>
      <td>83767</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Nevada</td>
      <td>84350</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>84380</td>
    </tr>
    <tr>
      <th>33</th>
      <td>North Dakota</td>
      <td>85506</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Georgia</td>
      <td>85961</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Pennsylvania</td>
      <td>87262</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Oregon</td>
      <td>88137</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Texas</td>
      <td>89506</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Delaware</td>
      <td>92308</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Rhode Island</td>
      <td>92427</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Utah</td>
      <td>94452</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Illinois</td>
      <td>95115</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Minnesota</td>
      <td>96814</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>98811</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Colorado</td>
      <td>100933</td>
    </tr>
    <tr>
      <th>28</th>
      <td>New Hampshire</td>
      <td>101292</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Washington</td>
      <td>103669</td>
    </tr>
    <tr>
      <th>31</th>
      <td>New York</td>
      <td>105304</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Virginia</td>
      <td>106023</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hawaii</td>
      <td>107348</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>111622</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Maryland</td>
      <td>114236</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Connecticut</td>
      <td>115337</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Massachusetts</td>
      <td>115964</td>
    </tr>
    <tr>
      <th>29</th>
      <td>New Jersey</td>
      <td>117868</td>
    </tr>
  </tbody>
</table>
</div>



### Dataframe sort, State
> In this example, categorical can be sorted.


```python
df.sort_values(by="State")

# The data is sorted alphabetically based on the "State" column.

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
      <th>State</th>
      <th>MeanHouseholdIncome</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>71964</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>98811</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>84380</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>69357</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>111622</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Colorado</td>
      <td>100933</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Connecticut</td>
      <td>115337</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Delaware</td>
      <td>92308</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Florida</td>
      <td>83104</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Georgia</td>
      <td>85961</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hawaii</td>
      <td>107348</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Idaho</td>
      <td>77399</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Illinois</td>
      <td>95115</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Indiana</td>
      <td>76984</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Iowa</td>
      <td>80316</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Kansas</td>
      <td>82103</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Kentucky</td>
      <td>72318</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Louisiana</td>
      <td>73759</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Maine</td>
      <td>78301</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Maryland</td>
      <td>114236</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Massachusetts</td>
      <td>115964</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Michigan</td>
      <td>80803</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Minnesota</td>
      <td>96814</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Mississippi</td>
      <td>65156</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Missouri</td>
      <td>78194</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Montana</td>
      <td>76834</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Nebraska</td>
      <td>82306</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Nevada</td>
      <td>84350</td>
    </tr>
    <tr>
      <th>28</th>
      <td>New Hampshire</td>
      <td>101292</td>
    </tr>
    <tr>
      <th>29</th>
      <td>New Jersey</td>
      <td>117868</td>
    </tr>
    <tr>
      <th>30</th>
      <td>New Mexico</td>
      <td>70241</td>
    </tr>
    <tr>
      <th>31</th>
      <td>New York</td>
      <td>105304</td>
    </tr>
    <tr>
      <th>32</th>
      <td>North Carolina</td>
      <td>79620</td>
    </tr>
    <tr>
      <th>33</th>
      <td>North Dakota</td>
      <td>85506</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Ohio</td>
      <td>78796</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Oklahoma</td>
      <td>74195</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Oregon</td>
      <td>88137</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Pennsylvania</td>
      <td>87262</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Rhode Island</td>
      <td>92427</td>
    </tr>
    <tr>
      <th>39</th>
      <td>South Carolina</td>
      <td>76390</td>
    </tr>
    <tr>
      <th>40</th>
      <td>South Dakota</td>
      <td>77932</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Tennessee</td>
      <td>76937</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Texas</td>
      <td>89506</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Utah</td>
      <td>94452</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Vermont</td>
      <td>83767</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Virginia</td>
      <td>106023</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Washington</td>
      <td>103669</td>
    </tr>
    <tr>
      <th>47</th>
      <td>West Virginia</td>
      <td>65332</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Wisconsin</td>
      <td>82757</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Wyoming</td>
      <td>83583</td>
    </tr>
  </tbody>
</table>
</div>



### Statistical summary
> In this example, all the summary statistics generated using: `df.describe`.


```python
print(df.describe())
```

           MeanHouseholdIncome
    count            50.000000
    mean          87461.460000
    std           13945.982845
    min           65156.000000
    25%           77532.250000
    50%           83675.000000
    75%           96389.250000
    max          117868.000000


#### Statistical Review
As seen in the above output, the dataframe is being described from the information for the column where applicable statistics are present. The "count" statistic for example, is the number of not-empty cells in the mean household income column. The mean is the average mean household income across all 50 states, and the standard deviation is how much the values within the mean household income column deviate from the mean. 

It is important to note that in many more complex datasets, there will be multiple columns with explanatory data. In those cases, the df.describe() method will need to be specified based on a specific column. 

## Conclusion

What are the key takeaways from this lesson?

The purpose is to obtain a basic understanding of working with a dataset, using Pandas dataframes. To obtain a more comprehensive understanding of Pandas capabilities, research operations such as filtering data based on certain criteria, grouping data, or performing calculations on multiple columns.  Additional work can be done with these Python modules (ie numpy, matplotlib). 

Explain each example briefly and provide a real-world scenario where such an operation would be useful.  Every dataset that you work with should have a purpose - that's what the field of data science is all about. 

For instance, in the Household income example, we analyzed a mean household income by state dataset. This could be applicable if someone wanted to find out where the most affordable place to live.

- Find the minimum household income 
- Expand data to look at affordability of areas within state
- Perhaps add other factors like employment in those areas


## Additional Resources
1. [Pandas Documentation](https://pandas.pydata.org/docs/)
- This is an essential resource for learning about Pandas and its various functionalities. It provides detailed documentation, examples, and explanations of different methods and operations.
2. [Data Science Applications](https://www.geeksforgeeks.org/major-applications-of-data-science/)
- This resource provides an overview of major applications of data science across various domains. It can help students understand the practical implications of data analysis and how it is used in different industries.
3. [Kaggle Datasets](https://www.kaggle.com/datasets)
- Kaggle is a popular platform for data science and machine learning. It offers a wide range of datasets for practice and exploration. Students can find interesting datasets on different topics to apply their Pandas learning and gain hands-on experience.
4. [NumPy Documentation](https://numpy.org/doc/)
- NumPy is another important Python library often used in conjunction with Pandas for numerical operations and scientific computing. The official NumPy documentation provides in-depth explanations and examples of working with arrays, mathematical functions, and more.
5. [Matplotlib Documentation](https://matplotlib.org/stable/contents.html)
- Matplotlib is a powerful data visualization library in Python. It allows students to create a wide range of plots and charts to visualize their data. The Matplotlib documentation offers comprehensive guidance on creating different types of visualizations, customizing plots, and using various plotting functions.
By referring to these resources, students can further expand their knowledge and explore advanced topics in Pandas, NumPy, and data visualization.

## Hacks

1. Find a CSV/JSON Dataset that interests you. Refer to Kaggle Datasets mentioned above.

2. Try to show your Pandas learning by illustrating **5** different numerical analysis operations being done on the dataset. After showing each operation in a separate code block, add a sentence explaining what that operation is showing and what real-world implication it has. It is important to make sure that you are not only able to run code to analyze data, but also understand its implications.

3. EXTRA: Research Matplotlib Documentation mentioned and implement a code block where you create a graph showing and visualize relationship in your chosen dataset. Then, add a sentence or two explaining what the relationship shows.
