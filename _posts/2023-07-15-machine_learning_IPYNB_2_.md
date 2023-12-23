---

---

#**Lesson: Exploring Materials Project Data and Predicting Material Properties**
##**Step 1: Importing Libraries**
In this lesson, we'll explore materials project data and perform various data analysis tasks using Python. First, we need to import the necessary libraries to work with data, visualize it, and perform machine learning.


```python
from mp_api.client import MPRester
import pandas as pd
from pymatgen.core import Composition
import datetime
from math import inf
from sklearn.model_selection import train_test_split
from sklearn.model_selection import cross_val_predict, cross_validate, KFold
from sklearn import linear_model
from sklearn.metrics import mean_squared_error, mean_absolute_error
import seaborn as sns

#from sklearn.model_selection import train_test_split
#from sklearn.model_selection import cross_val_predict, cross_validate, KFold
#from sklearn.metrics import r2_score
#from sklearn.cluster import KMeans
#from sklearn.discriminant_analysis import LinearDiscriminantAnalysis, QuadraticDiscriminantAnalysis
#from sklearn.linear_model import LogisticRegression
#from sklearn.cluster import DBSCAN
```

##**Step 2: Accessing Materials Project Data**
The Materials Project is a database containing materials properties data. We'll use the MPRester library to access materials project data related to ABO3 compounds. We'll fetch properties like material_id, formula_pretty, nsites, band_gap, formation_energy_per_atom, theoretical, and energy_above_hull for all ABO3 compounds.


```python
mpr = MPRester(api_key="PT7C2S6MEfO67wSwPtB3KlVvUu7ZI166")
```


```python
ABO3 = mpr.summary.search(formula="**O3", fields=["material_id", "formula_pretty", "nsites", "band_gap", "formation_energy_per_atom", "theoretical", "energy_above_hull"])
```


    Retrieving SummaryDoc documents:   0%|          | 0/2544 [00:00<?, ?it/s]



```python
ABO3_dicts = [i.dict() for i in ABO3]
```


```python
mp_database = pd.DataFrame(data=ABO3_dicts)
```

## **Step 3: Data Manipulation and Cleaning**
Now that we have the materials project data in a DataFrame named 'mp_database,' let's clean and manipulate the data to make it more suitable for analysis.



```python
mp_database = mp_database.drop("fields_not_requested", axis=1)
```


```python
len(mp_database)
```




    2544




```python
grouped = mp_database.groupby('formula_pretty').size()
```


```python
grouped.mean()
```




    1.912781954887218




```python
mp_database.nunique()
```




    nsites                         18
    formula_pretty               1330
    material_id                  2544
    formation_energy_per_atom    2544
    energy_above_hull            2199
    band_gap                     1297
    theoretical                     2
    dtype: int64




```python
mp_database
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
      <th>nsites</th>
      <th>formula_pretty</th>
      <th>material_id</th>
      <th>formation_energy_per_atom</th>
      <th>energy_above_hull</th>
      <th>band_gap</th>
      <th>theoretical</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>NaNbO3</td>
      <td>mp-4681</td>
      <td>-2.833432</td>
      <td>0.012851</td>
      <td>2.3684</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10</td>
      <td>MnZnO3</td>
      <td>mp-754318</td>
      <td>-1.814413</td>
      <td>0.005794</td>
      <td>1.3225</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20</td>
      <td>MnAlO3</td>
      <td>mp-1368992</td>
      <td>-2.573144</td>
      <td>0.161167</td>
      <td>0.5506</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5</td>
      <td>BaBiO3</td>
      <td>mp-545783</td>
      <td>-2.222888</td>
      <td>0.021211</td>
      <td>0.0000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>MnTlO3</td>
      <td>mp-770870</td>
      <td>-1.505971</td>
      <td>0.053737</td>
      <td>0.0000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2539</th>
      <td>5</td>
      <td>BaCdO3</td>
      <td>mp-1183292</td>
      <td>-1.645883</td>
      <td>0.202881</td>
      <td>0.0000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2540</th>
      <td>10</td>
      <td>LaMnO3</td>
      <td>mp-1205375</td>
      <td>-2.968812</td>
      <td>0.161878</td>
      <td>0.0000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2541</th>
      <td>5</td>
      <td>TlSiO3</td>
      <td>mp-1187529</td>
      <td>-1.862867</td>
      <td>0.534038</td>
      <td>0.0000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2542</th>
      <td>5</td>
      <td>GdBeO3</td>
      <td>mp-1184468</td>
      <td>-2.874174</td>
      <td>0.315310</td>
      <td>0.0000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2543</th>
      <td>5</td>
      <td>InSiO3</td>
      <td>mp-1184751</td>
      <td>-2.064795</td>
      <td>0.561967</td>
      <td>0.0000</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
<p>2544 rows × 7 columns</p>
</div>




```python
mp_database['theoretical'].value_counts()[False]
```




    836




```python
mp_database['formation_energy_per_atom_ev_mol'] = mp_database['formation_energy_per_atom'] * 96491.5666
```


```python
mp_database
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
      <th>nsites</th>
      <th>formula_pretty</th>
      <th>material_id</th>
      <th>formation_energy_per_atom</th>
      <th>energy_above_hull</th>
      <th>band_gap</th>
      <th>theoretical</th>
      <th>formation_energy_per_atom_ev_mol</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>NaNbO3</td>
      <td>mp-4681</td>
      <td>-2.833432</td>
      <td>0.012851</td>
      <td>2.3684</td>
      <td>False</td>
      <td>-273402.322598</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10</td>
      <td>MnZnO3</td>
      <td>mp-754318</td>
      <td>-1.814413</td>
      <td>0.005794</td>
      <td>1.3225</td>
      <td>True</td>
      <td>-175075.572015</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20</td>
      <td>MnAlO3</td>
      <td>mp-1368992</td>
      <td>-2.573144</td>
      <td>0.161167</td>
      <td>0.5506</td>
      <td>True</td>
      <td>-248286.682798</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5</td>
      <td>BaBiO3</td>
      <td>mp-545783</td>
      <td>-2.222888</td>
      <td>0.021211</td>
      <td>0.0000</td>
      <td>False</td>
      <td>-214489.986143</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>MnTlO3</td>
      <td>mp-770870</td>
      <td>-1.505971</td>
      <td>0.053737</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-145313.457382</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2539</th>
      <td>5</td>
      <td>BaCdO3</td>
      <td>mp-1183292</td>
      <td>-1.645883</td>
      <td>0.202881</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-158813.786775</td>
    </tr>
    <tr>
      <th>2540</th>
      <td>10</td>
      <td>LaMnO3</td>
      <td>mp-1205375</td>
      <td>-2.968812</td>
      <td>0.161878</td>
      <td>0.0000</td>
      <td>False</td>
      <td>-286465.330840</td>
    </tr>
    <tr>
      <th>2541</th>
      <td>5</td>
      <td>TlSiO3</td>
      <td>mp-1187529</td>
      <td>-1.862867</td>
      <td>0.534038</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-179750.985874</td>
    </tr>
    <tr>
      <th>2542</th>
      <td>5</td>
      <td>GdBeO3</td>
      <td>mp-1184468</td>
      <td>-2.874174</td>
      <td>0.315310</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-277333.565860</td>
    </tr>
    <tr>
      <th>2543</th>
      <td>5</td>
      <td>InSiO3</td>
      <td>mp-1184751</td>
      <td>-2.064795</td>
      <td>0.561967</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-199235.298396</td>
    </tr>
  </tbody>
</table>
<p>2544 rows × 8 columns</p>
</div>



##**Step 4: Classifying Materials**
Let's classify the materials based on their stability and band gap properties. We'll define two functions, 'stability_category' and 'bandgap_category,' to perform the classification.


```python
# Assuming you already have a pandas DataFrame named 'mp_database' with columns 'energy_above_hull' and 'band_gap'

# Function to classify materials based on energy_above_hull
def stability_category(energy):
    if energy > 0.03:
        return 'unstable'
    else:
        return 'potentially_stable'

# Function to classify materials based on band_gap
def bandgap_category(band_gap):
    if band_gap == 0:
        return 'metal'
    elif 0 < band_gap <= 1:
        return 'small_bandgap'
    else:
        return 'large_bandgap'

# Apply the classification functions to the DataFrame
mp_database['stability'] = mp_database['energy_above_hull'].apply(stability_category)
mp_database['bandgap_type'] = mp_database['band_gap'].apply(bandgap_category)

# Create a pivot table to get the desired table structure
new_table = pd.pivot_table(mp_database, index='stability', columns='bandgap_type', aggfunc='size', fill_value=0)

# Reorder the columns to match the desired order
new_table = new_table[['metal', 'small_bandgap', 'large_bandgap']]

print(new_table)
```

    bandgap_type        metal  small_bandgap  large_bandgap
    stability                                              
    potentially_stable    197            103            525
    unstable             1042            212            465



```python
mp_database
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
      <th>nsites</th>
      <th>formula_pretty</th>
      <th>material_id</th>
      <th>formation_energy_per_atom</th>
      <th>energy_above_hull</th>
      <th>band_gap</th>
      <th>theoretical</th>
      <th>formation_energy_per_atom_ev_mol</th>
      <th>stability</th>
      <th>bandgap_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>NaNbO3</td>
      <td>mp-4681</td>
      <td>-2.833432</td>
      <td>0.012851</td>
      <td>2.3684</td>
      <td>False</td>
      <td>-273402.322598</td>
      <td>potentially_stable</td>
      <td>large_bandgap</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10</td>
      <td>MnZnO3</td>
      <td>mp-754318</td>
      <td>-1.814413</td>
      <td>0.005794</td>
      <td>1.3225</td>
      <td>True</td>
      <td>-175075.572015</td>
      <td>potentially_stable</td>
      <td>large_bandgap</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20</td>
      <td>MnAlO3</td>
      <td>mp-1368992</td>
      <td>-2.573144</td>
      <td>0.161167</td>
      <td>0.5506</td>
      <td>True</td>
      <td>-248286.682798</td>
      <td>unstable</td>
      <td>small_bandgap</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5</td>
      <td>BaBiO3</td>
      <td>mp-545783</td>
      <td>-2.222888</td>
      <td>0.021211</td>
      <td>0.0000</td>
      <td>False</td>
      <td>-214489.986143</td>
      <td>potentially_stable</td>
      <td>metal</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>MnTlO3</td>
      <td>mp-770870</td>
      <td>-1.505971</td>
      <td>0.053737</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-145313.457382</td>
      <td>unstable</td>
      <td>metal</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2539</th>
      <td>5</td>
      <td>BaCdO3</td>
      <td>mp-1183292</td>
      <td>-1.645883</td>
      <td>0.202881</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-158813.786775</td>
      <td>unstable</td>
      <td>metal</td>
    </tr>
    <tr>
      <th>2540</th>
      <td>10</td>
      <td>LaMnO3</td>
      <td>mp-1205375</td>
      <td>-2.968812</td>
      <td>0.161878</td>
      <td>0.0000</td>
      <td>False</td>
      <td>-286465.330840</td>
      <td>unstable</td>
      <td>metal</td>
    </tr>
    <tr>
      <th>2541</th>
      <td>5</td>
      <td>TlSiO3</td>
      <td>mp-1187529</td>
      <td>-1.862867</td>
      <td>0.534038</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-179750.985874</td>
      <td>unstable</td>
      <td>metal</td>
    </tr>
    <tr>
      <th>2542</th>
      <td>5</td>
      <td>GdBeO3</td>
      <td>mp-1184468</td>
      <td>-2.874174</td>
      <td>0.315310</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-277333.565860</td>
      <td>unstable</td>
      <td>metal</td>
    </tr>
    <tr>
      <th>2543</th>
      <td>5</td>
      <td>InSiO3</td>
      <td>mp-1184751</td>
      <td>-2.064795</td>
      <td>0.561967</td>
      <td>0.0000</td>
      <td>True</td>
      <td>-199235.298396</td>
      <td>unstable</td>
      <td>metal</td>
    </tr>
  </tbody>
</table>
<p>2544 rows × 10 columns</p>
</div>



##**Step 5: Data Visualization - Formation Energy Distribution**
Visualizing data is crucial for understanding its distribution and characteristics. Let's create a histogram to visualize the distribution of formation energies per atom.


```python
import pandas as pd
import matplotlib.pyplot as plt

# Assuming you have a pandas DataFrame named 'mp_database' with the 'formation_energies_per_atom' column

# Calculate the average and standard deviation
average_energy = mp_database['formation_energy_per_atom'].mean()
std_deviation = mp_database['formation_energy_per_atom'].std()

# Plot the distribution of the column
plt.figure(figsize=(10, 6))
plt.hist(mp_database['formation_energy_per_atom'], bins=30, edgecolor='black', alpha=0.7)

# Add labels and title to the plot
plt.xlabel('Formation Energies per Atom')
plt.ylabel('Frequency')
plt.title('Distribution of Formation Energies per Atom')

# Annotate the plot with average and standard deviation
plt.axvline(average_energy, color='red', linestyle='dashed', linewidth=2, label='Average')
plt.axvline(average_energy + std_deviation, color='orange', linestyle='dashed', linewidth=2, label='Standard Deviation')
plt.axvline(average_energy - std_deviation, color='orange', linestyle='dashed', linewidth=2)
plt.annotate(f'Average: {average_energy:.2f}', xy=(average_energy, 300), xytext=(average_energy + 0.5, 400),
             arrowprops=dict(facecolor='black', shrink=0.05), fontsize=12)
plt.annotate(f'Standard Deviation: {std_deviation:.2f}', xy=(average_energy + std_deviation, 250),
             xytext=(average_energy + std_deviation + 1, 350), arrowprops=dict(facecolor='black', shrink=0.05),
             fontsize=12)

# Show legend
plt.legend()

# Display the plot
plt.show()
```


    
![png](output_21_0.png)
    


##**Step 6: Filtering and Preparing Data for Analysis**
Next, we'll filter the data to keep only the most relevant materials. We'll sort the data based on formation energy and remove duplicate formulas to keep only the materials with the lowest formation energy for each formula.


```python
orig_data = pd.read_csv("data2022.csv", na_filter=False)
```


```python
len(orig_data)
```




    681




```python
orig_data
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
      <th>Unnamed: 0</th>
      <th>task_id</th>
      <th>formula</th>
      <th>formation_energy_per_atom</th>
      <th>e_above_hull</th>
      <th>band_gap</th>
      <th>has_bandstructure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>297</td>
      <td>mp-570140</td>
      <td>AuBr</td>
      <td>-0.125181</td>
      <td>0.011579</td>
      <td>1.9716</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>573</td>
      <td>mp-1206190</td>
      <td>ZnI6</td>
      <td>0.387945</td>
      <td>0.652470</td>
      <td>0.1076</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>579</td>
      <td>mp-1208424</td>
      <td>TeBr4</td>
      <td>-0.390060</td>
      <td>0.000000</td>
      <td>2.5202</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>738</td>
      <td>mp-570480</td>
      <td>TcBr4</td>
      <td>-0.404404</td>
      <td>0.000000</td>
      <td>0.6025</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>880</td>
      <td>mp-1064459</td>
      <td>PBr</td>
      <td>1.067789</td>
      <td>1.327882</td>
      <td>0.0000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>676</th>
      <td>118973</td>
      <td>mp-974371</td>
      <td>Rh3Br</td>
      <td>0.670866</td>
      <td>0.820678</td>
      <td>0.0000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>677</th>
      <td>121600</td>
      <td>mp-867231</td>
      <td>TbI3</td>
      <td>-1.382737</td>
      <td>0.000000</td>
      <td>2.0937</td>
      <td>True</td>
    </tr>
    <tr>
      <th>678</th>
      <td>121602</td>
      <td>mp-867247</td>
      <td>TbBr3</td>
      <td>-1.860767</td>
      <td>0.000000</td>
      <td>2.8570</td>
      <td>True</td>
    </tr>
    <tr>
      <th>679</th>
      <td>121619</td>
      <td>mp-867355</td>
      <td>TeBr2</td>
      <td>-0.186842</td>
      <td>0.140200</td>
      <td>0.6529</td>
      <td>True</td>
    </tr>
    <tr>
      <th>680</th>
      <td>121834</td>
      <td>mp-867893</td>
      <td>SmI3</td>
      <td>-1.399012</td>
      <td>0.000000</td>
      <td>2.0145</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
<p>681 rows × 7 columns</p>
</div>




```python
import pandas as pd

# Assuming you have a pandas DataFrame named 'orig_data' with 'formation_energy_per_atom' and 'formula' columns

# Sort the DataFrame by 'formation_energy_per_atom' in ascending order
orig_data.sort_values(by='formation_energy_per_atom', ascending=True, inplace=True)

# Drop duplicates based on the 'formula' column and keep the first occurrence (smallest formation_energy_per_atom)
filtered_data = orig_data.drop_duplicates(subset='formula', keep='first')

# Display the resulting DataFrame
filtered_data
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
      <th>Unnamed: 0</th>
      <th>task_id</th>
      <th>formula</th>
      <th>formation_energy_per_atom</th>
      <th>e_above_hull</th>
      <th>band_gap</th>
      <th>has_bandstructure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>191</th>
      <td>29313</td>
      <td>mp-27456</td>
      <td>BaBr2</td>
      <td>-2.227832</td>
      <td>0.000000</td>
      <td>4.3731</td>
      <td>True</td>
    </tr>
    <tr>
      <th>475</th>
      <td>80856</td>
      <td>mp-567744</td>
      <td>SrBr2</td>
      <td>-2.156160</td>
      <td>0.000000</td>
      <td>4.4697</td>
      <td>True</td>
    </tr>
    <tr>
      <th>219</th>
      <td>35022</td>
      <td>mp-1071541</td>
      <td>YbBr2</td>
      <td>-2.109686</td>
      <td>0.000000</td>
      <td>4.3169</td>
      <td>False</td>
    </tr>
    <tr>
      <th>414</th>
      <td>70294</td>
      <td>mp-27972</td>
      <td>AcBr3</td>
      <td>-2.098107</td>
      <td>0.000000</td>
      <td>4.1045</td>
      <td>True</td>
    </tr>
    <tr>
      <th>304</th>
      <td>51723</td>
      <td>mp-22888</td>
      <td>CaBr2</td>
      <td>-2.052632</td>
      <td>0.000000</td>
      <td>4.5304</td>
      <td>True</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>268</th>
      <td>44109</td>
      <td>mp-1184756</td>
      <td>IrI</td>
      <td>0.949140</td>
      <td>1.047085</td>
      <td>0.0000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>601</th>
      <td>105675</td>
      <td>mp-1197830</td>
      <td>CI</td>
      <td>0.950707</td>
      <td>0.950707</td>
      <td>3.4057</td>
      <td>False</td>
    </tr>
    <tr>
      <th>541</th>
      <td>93486</td>
      <td>mp-975905</td>
      <td>Mo3I</td>
      <td>0.977325</td>
      <td>1.122018</td>
      <td>0.0000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>517</th>
      <td>88638</td>
      <td>mp-1209830</td>
      <td>PBr2</td>
      <td>1.261139</td>
      <td>1.607930</td>
      <td>1.7322</td>
      <td>False</td>
    </tr>
    <tr>
      <th>587</th>
      <td>102482</td>
      <td>mp-974474</td>
      <td>ReBr</td>
      <td>1.306459</td>
      <td>1.507749</td>
      <td>0.0000</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
<p>392 rows × 7 columns</p>
</div>




```python
filtered_data = filtered_data[filtered_data["formation_energy_per_atom"] < 0]
```


```python
filtered_data
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
      <th>Unnamed: 0</th>
      <th>task_id</th>
      <th>formula</th>
      <th>formation_energy_per_atom</th>
      <th>e_above_hull</th>
      <th>band_gap</th>
      <th>has_bandstructure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>191</th>
      <td>29313</td>
      <td>mp-27456</td>
      <td>BaBr2</td>
      <td>-2.227832</td>
      <td>0.000000</td>
      <td>4.3731</td>
      <td>True</td>
    </tr>
    <tr>
      <th>475</th>
      <td>80856</td>
      <td>mp-567744</td>
      <td>SrBr2</td>
      <td>-2.156160</td>
      <td>0.000000</td>
      <td>4.4697</td>
      <td>True</td>
    </tr>
    <tr>
      <th>219</th>
      <td>35022</td>
      <td>mp-1071541</td>
      <td>YbBr2</td>
      <td>-2.109686</td>
      <td>0.000000</td>
      <td>4.3169</td>
      <td>False</td>
    </tr>
    <tr>
      <th>414</th>
      <td>70294</td>
      <td>mp-27972</td>
      <td>AcBr3</td>
      <td>-2.098107</td>
      <td>0.000000</td>
      <td>4.1045</td>
      <td>True</td>
    </tr>
    <tr>
      <th>304</th>
      <td>51723</td>
      <td>mp-22888</td>
      <td>CaBr2</td>
      <td>-2.052632</td>
      <td>0.000000</td>
      <td>4.5304</td>
      <td>True</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>222</th>
      <td>35973</td>
      <td>mp-862773</td>
      <td>TeI2</td>
      <td>-0.036698</td>
      <td>0.070018</td>
      <td>0.7144</td>
      <td>False</td>
    </tr>
    <tr>
      <th>481</th>
      <td>82174</td>
      <td>mp-1185549</td>
      <td>Mg149Br</td>
      <td>-0.022094</td>
      <td>0.000000</td>
      <td>0.0021</td>
      <td>False</td>
    </tr>
    <tr>
      <th>600</th>
      <td>105624</td>
      <td>mp-1185590</td>
      <td>Mg149I</td>
      <td>-0.017586</td>
      <td>0.000000</td>
      <td>0.0025</td>
      <td>False</td>
    </tr>
    <tr>
      <th>512</th>
      <td>87707</td>
      <td>mp-1214775</td>
      <td>AuBr2</td>
      <td>-0.007624</td>
      <td>0.143039</td>
      <td>0.0000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>544</th>
      <td>94095</td>
      <td>mp-1179985</td>
      <td>PBr</td>
      <td>-0.007386</td>
      <td>0.252708</td>
      <td>1.3954</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>338 rows × 7 columns</p>
</div>



## **Step 7: Calculating Composition-Averaged Properties**
We will now calculate the composition-averaged atomic radius for each material in 'filtered_data' using the element properties data available in 'element_data' DataFrame.


```python
element_data = pd.read_csv("element_properties.csv", index_col=0)
```


```python
element_data
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
      <th>AtomicRadius</th>
      <th>AtomicVolume</th>
      <th>AtomicWeight</th>
      <th>BoilingT</th>
      <th>CovalentRadius</th>
      <th>Density</th>
      <th>ElectronAffinity</th>
      <th>Electronegativity</th>
      <th>FirstIonizationEnergy</th>
      <th>HeatCapacityMass</th>
      <th>SecondIonizationEnergy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>H</th>
      <td>0.25</td>
      <td>18618.051940</td>
      <td>1.007940</td>
      <td>20.13</td>
      <td>31</td>
      <td>0.0899</td>
      <td>72.8</td>
      <td>2.20</td>
      <td>13.598443</td>
      <td>14.304</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>He</th>
      <td>NaN</td>
      <td>37236.035560</td>
      <td>4.002602</td>
      <td>4.07</td>
      <td>28</td>
      <td>0.1785</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>24.587387</td>
      <td>5.130</td>
      <td>54.41776</td>
    </tr>
    <tr>
      <th>Li</th>
      <td>1.45</td>
      <td>21.544058</td>
      <td>6.941000</td>
      <td>1615.00</td>
      <td>128</td>
      <td>535.0000</td>
      <td>59.6</td>
      <td>0.98</td>
      <td>5.391719</td>
      <td>3.582</td>
      <td>75.64000</td>
    </tr>
    <tr>
      <th>Be</th>
      <td>1.05</td>
      <td>8.098176</td>
      <td>9.012182</td>
      <td>2743.00</td>
      <td>96</td>
      <td>1848.0000</td>
      <td>0.0</td>
      <td>1.57</td>
      <td>9.322700</td>
      <td>1.825</td>
      <td>18.21114</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.85</td>
      <td>7.297767</td>
      <td>10.811000</td>
      <td>4273.00</td>
      <td>84</td>
      <td>2460.0000</td>
      <td>26.7</td>
      <td>2.04</td>
      <td>8.298020</td>
      <td>1.026</td>
      <td>25.15480</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Pa</th>
      <td>1.80</td>
      <td>24.961161</td>
      <td>231.035860</td>
      <td>4273.00</td>
      <td>200</td>
      <td>15370.0000</td>
      <td>NaN</td>
      <td>1.50</td>
      <td>5.890000</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>U</th>
      <td>1.75</td>
      <td>20.748847</td>
      <td>238.028910</td>
      <td>4200.00</td>
      <td>196</td>
      <td>19050.0000</td>
      <td>NaN</td>
      <td>1.38</td>
      <td>6.194100</td>
      <td>0.116</td>
      <td>10.60000</td>
    </tr>
    <tr>
      <th>Np</th>
      <td>1.75</td>
      <td>19.244839</td>
      <td>237.000000</td>
      <td>4273.00</td>
      <td>190</td>
      <td>20450.0000</td>
      <td>NaN</td>
      <td>1.36</td>
      <td>6.265700</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Pu</th>
      <td>1.75</td>
      <td>20.447164</td>
      <td>244.000000</td>
      <td>3503.00</td>
      <td>187</td>
      <td>19816.0000</td>
      <td>NaN</td>
      <td>1.28</td>
      <td>6.026000</td>
      <td>NaN</td>
      <td>11.20000</td>
    </tr>
    <tr>
      <th>Am</th>
      <td>1.75</td>
      <td>29.518685</td>
      <td>243.000000</td>
      <td>2284.00</td>
      <td>180</td>
      <td>13670.0000</td>
      <td>NaN</td>
      <td>1.30</td>
      <td>5.973800</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>95 rows × 11 columns</p>
</div>




```python
type(element_data["AtomicRadius"][1])
```




    numpy.float64




```python
comp = Composition('Al2O3')
```


```python
print(comp.elements)
```

    [Element Al, Element O]



```python
print(comp.to_data_dict['unit_cell_composition'])
```

    defaultdict(<class 'float'>, {'Al': 2.0, 'O': 3.0})



```python
import numpy as np

count = 0
column_nans = {}
for column in element_data.columns:
    for element in element_data[column]:
        if np.isnan(element):
            count += 1
    column_nans[column] = count
    count = 0
```


```python
column_nans
```




    {'AtomicRadius': 7,
     'AtomicVolume': 2,
     'AtomicWeight': 0,
     'BoilingT': 2,
     'CovalentRadius': 0,
     'Density': 2,
     'ElectronAffinity': 9,
     'Electronegativity': 4,
     'FirstIonizationEnergy': 1,
     'HeatCapacityMass': 10,
     'SecondIonizationEnergy': 12}




```python
# Set of commands to compute the mean value for each column, replace the NaNs by the mean and output the updated data set
for i in element_data.columns:
    col_mean = element_data[i].mean(skipna=True)
    element_data[i] = element_data[i].fillna(col_mean)
element_data
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
      <th>AtomicRadius</th>
      <th>AtomicVolume</th>
      <th>AtomicWeight</th>
      <th>BoilingT</th>
      <th>CovalentRadius</th>
      <th>Density</th>
      <th>ElectronAffinity</th>
      <th>Electronegativity</th>
      <th>FirstIonizationEnergy</th>
      <th>HeatCapacityMass</th>
      <th>SecondIonizationEnergy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>H</th>
      <td>0.250000</td>
      <td>18618.051940</td>
      <td>1.007940</td>
      <td>20.13</td>
      <td>31</td>
      <td>0.0899</td>
      <td>72.800000</td>
      <td>2.200000</td>
      <td>13.598443</td>
      <td>14.304000</td>
      <td>18.947504</td>
    </tr>
    <tr>
      <th>He</th>
      <td>1.500682</td>
      <td>37236.035560</td>
      <td>4.002602</td>
      <td>4.07</td>
      <td>28</td>
      <td>0.1785</td>
      <td>0.000000</td>
      <td>1.747033</td>
      <td>24.587387</td>
      <td>5.130000</td>
      <td>54.417760</td>
    </tr>
    <tr>
      <th>Li</th>
      <td>1.450000</td>
      <td>21.544058</td>
      <td>6.941000</td>
      <td>1615.00</td>
      <td>128</td>
      <td>535.0000</td>
      <td>59.600000</td>
      <td>0.980000</td>
      <td>5.391719</td>
      <td>3.582000</td>
      <td>75.640000</td>
    </tr>
    <tr>
      <th>Be</th>
      <td>1.050000</td>
      <td>8.098176</td>
      <td>9.012182</td>
      <td>2743.00</td>
      <td>96</td>
      <td>1848.0000</td>
      <td>0.000000</td>
      <td>1.570000</td>
      <td>9.322700</td>
      <td>1.825000</td>
      <td>18.211140</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.850000</td>
      <td>7.297767</td>
      <td>10.811000</td>
      <td>4273.00</td>
      <td>84</td>
      <td>2460.0000</td>
      <td>26.700000</td>
      <td>2.040000</td>
      <td>8.298020</td>
      <td>1.026000</td>
      <td>25.154800</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Pa</th>
      <td>1.800000</td>
      <td>24.961161</td>
      <td>231.035860</td>
      <td>4273.00</td>
      <td>200</td>
      <td>15370.0000</td>
      <td>76.162209</td>
      <td>1.500000</td>
      <td>5.890000</td>
      <td>0.635447</td>
      <td>18.947504</td>
    </tr>
    <tr>
      <th>U</th>
      <td>1.750000</td>
      <td>20.748847</td>
      <td>238.028910</td>
      <td>4200.00</td>
      <td>196</td>
      <td>19050.0000</td>
      <td>76.162209</td>
      <td>1.380000</td>
      <td>6.194100</td>
      <td>0.116000</td>
      <td>10.600000</td>
    </tr>
    <tr>
      <th>Np</th>
      <td>1.750000</td>
      <td>19.244839</td>
      <td>237.000000</td>
      <td>4273.00</td>
      <td>190</td>
      <td>20450.0000</td>
      <td>76.162209</td>
      <td>1.360000</td>
      <td>6.265700</td>
      <td>0.635447</td>
      <td>18.947504</td>
    </tr>
    <tr>
      <th>Pu</th>
      <td>1.750000</td>
      <td>20.447164</td>
      <td>244.000000</td>
      <td>3503.00</td>
      <td>187</td>
      <td>19816.0000</td>
      <td>76.162209</td>
      <td>1.280000</td>
      <td>6.026000</td>
      <td>0.635447</td>
      <td>11.200000</td>
    </tr>
    <tr>
      <th>Am</th>
      <td>1.750000</td>
      <td>29.518685</td>
      <td>243.000000</td>
      <td>2284.00</td>
      <td>180</td>
      <td>13670.0000</td>
      <td>76.162209</td>
      <td>1.300000</td>
      <td>5.973800</td>
      <td>0.635447</td>
      <td>18.947504</td>
    </tr>
  </tbody>
</table>
<p>95 rows × 11 columns</p>
</div>




```python
# Set of commands to compute and output the composition-averaged Atomic Radius for all materials
begin_time = datetime.datetime.now()
atomic_radius = pd.DataFrame()
filtered_data["composition_dict"] = [Composition(i).as_dict() for i in filtered_data.formula]
atomic_radius["formula"] = filtered_data.formula
atomic_radius["atomic_radius_avg"] = [np.average([element_data.loc[[el]]["AtomicRadius"][0] for el in comp], weights=[comp[el] for el in comp]) for comp in filtered_data.composition_dict]
print(datetime.datetime.now() - begin_time)
atomic_radius
```

    
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy


    0:00:00.211715





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
      <th>formula</th>
      <th>atomic_radius_avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>191</th>
      <td>BaBr2</td>
      <td>1.483333</td>
    </tr>
    <tr>
      <th>475</th>
      <td>SrBr2</td>
      <td>1.433333</td>
    </tr>
    <tr>
      <th>219</th>
      <td>YbBr2</td>
      <td>1.350000</td>
    </tr>
    <tr>
      <th>414</th>
      <td>AcBr3</td>
      <td>1.350000</td>
    </tr>
    <tr>
      <th>304</th>
      <td>CaBr2</td>
      <td>1.366667</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>222</th>
      <td>TeI2</td>
      <td>1.400000</td>
    </tr>
    <tr>
      <th>481</th>
      <td>Mg149Br</td>
      <td>1.497667</td>
    </tr>
    <tr>
      <th>600</th>
      <td>Mg149I</td>
      <td>1.499333</td>
    </tr>
    <tr>
      <th>512</th>
      <td>AuBr2</td>
      <td>1.216667</td>
    </tr>
    <tr>
      <th>544</th>
      <td>PBr</td>
      <td>1.075000</td>
    </tr>
  </tbody>
</table>
<p>338 rows × 2 columns</p>
</div>




```python
atomic_radius_2 = pd.DataFrame()
atomic_radius_2["formula"] = filtered_data.formula
atomic_radius_2["composition_dict"] = filtered_data.composition_dict
composition_averaged_list = []
for composition in atomic_radius_2["composition_dict"]:
    total_atoms_in_formula = 0
    element_radius_weighted = 0
    for element in composition:
        total_atoms_in_formula += composition[element]
        element_radius_weighted += element_data.loc[element]["AtomicRadius"] * composition[element]
    average = element_radius_weighted / total_atoms_in_formula
    composition_averaged_list.append(average)
atomic_radius_2["atomic_radius_avg"] = composition_averaged_list
```


```python
atomic_radius_2
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
      <th>formula</th>
      <th>composition_dict</th>
      <th>atomic_radius_avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>191</th>
      <td>BaBr2</td>
      <td>{'Ba': 1.0, 'Br': 2.0}</td>
      <td>1.483333</td>
    </tr>
    <tr>
      <th>475</th>
      <td>SrBr2</td>
      <td>{'Sr': 1.0, 'Br': 2.0}</td>
      <td>1.433333</td>
    </tr>
    <tr>
      <th>219</th>
      <td>YbBr2</td>
      <td>{'Yb': 1.0, 'Br': 2.0}</td>
      <td>1.350000</td>
    </tr>
    <tr>
      <th>414</th>
      <td>AcBr3</td>
      <td>{'Ac': 1.0, 'Br': 3.0}</td>
      <td>1.350000</td>
    </tr>
    <tr>
      <th>304</th>
      <td>CaBr2</td>
      <td>{'Ca': 1.0, 'Br': 2.0}</td>
      <td>1.366667</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>222</th>
      <td>TeI2</td>
      <td>{'Te': 1.0, 'I': 2.0}</td>
      <td>1.400000</td>
    </tr>
    <tr>
      <th>481</th>
      <td>Mg149Br</td>
      <td>{'Mg': 149.0, 'Br': 1.0}</td>
      <td>1.497667</td>
    </tr>
    <tr>
      <th>600</th>
      <td>Mg149I</td>
      <td>{'Mg': 149.0, 'I': 1.0}</td>
      <td>1.499333</td>
    </tr>
    <tr>
      <th>512</th>
      <td>AuBr2</td>
      <td>{'Au': 1.0, 'Br': 2.0}</td>
      <td>1.216667</td>
    </tr>
    <tr>
      <th>544</th>
      <td>PBr</td>
      <td>{'P': 1.0, 'Br': 1.0}</td>
      <td>1.075000</td>
    </tr>
  </tbody>
</table>
<p>338 rows × 3 columns</p>
</div>




```python
average_properties = pd.DataFrame()
average_properties["formula"] = filtered_data.formula
average_properties["composition_dict"] = filtered_data.composition_dict
for prop in element_data.columns:
    composition_averaged_list = []
    for composition in average_properties["composition_dict"]:
        total_atoms_in_formula = 0
        element_property_weighted = 0
        for element in composition:
            total_atoms_in_formula += composition[element]
            element_property_weighted += element_data.loc[element][prop] * composition[element]
        average = element_property_weighted / total_atoms_in_formula
        composition_averaged_list.append(average)
    average_properties[prop] = composition_averaged_list
```


```python
average_properties
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
      <th>formula</th>
      <th>composition_dict</th>
      <th>AtomicRadius</th>
      <th>AtomicVolume</th>
      <th>AtomicWeight</th>
      <th>BoilingT</th>
      <th>CovalentRadius</th>
      <th>Density</th>
      <th>ElectronAffinity</th>
      <th>Electronegativity</th>
      <th>FirstIonizationEnergy</th>
      <th>HeatCapacityMass</th>
      <th>SecondIonizationEnergy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>191</th>
      <td>BaBr2</td>
      <td>{'Ba': 1.0, 'Br': 2.0}</td>
      <td>1.483333</td>
      <td>50.008311</td>
      <td>99.045000</td>
      <td>935.666667</td>
      <td>151.666667</td>
      <td>3250.000000</td>
      <td>221.050000</td>
      <td>2.270000</td>
      <td>9.613088</td>
      <td>0.384000</td>
      <td>17.728610</td>
    </tr>
    <tr>
      <th>475</th>
      <td>SrBr2</td>
      <td>{'Sr': 1.0, 'Br': 2.0}</td>
      <td>1.433333</td>
      <td>46.792927</td>
      <td>82.476000</td>
      <td>773.000000</td>
      <td>145.000000</td>
      <td>2956.666667</td>
      <td>218.076667</td>
      <td>2.290000</td>
      <td>9.774150</td>
      <td>0.418000</td>
      <td>18.070700</td>
    </tr>
    <tr>
      <th>219</th>
      <td>YbBr2</td>
      <td>{'Yb': 1.0, 'Br': 2.0}</td>
      <td>1.350000</td>
      <td>42.931774</td>
      <td>110.954000</td>
      <td>711.000000</td>
      <td>142.333333</td>
      <td>4270.000000</td>
      <td>233.066667</td>
      <td>2.393333</td>
      <td>9.960587</td>
      <td>0.367667</td>
      <td>18.452667</td>
    </tr>
    <tr>
      <th>414</th>
      <td>AcBr3</td>
      <td>{'Ac': 1.0, 'Br': 3.0}</td>
      <td>1.350000</td>
      <td>41.254141</td>
      <td>116.678000</td>
      <td>1117.250000</td>
      <td>143.750000</td>
      <td>4857.500000</td>
      <td>262.490552</td>
      <td>2.495000</td>
      <td>10.152850</td>
      <td>0.385500</td>
      <td>19.130750</td>
    </tr>
    <tr>
      <th>304</th>
      <td>CaBr2</td>
      <td>{'Ca': 1.0, 'Br': 2.0}</td>
      <td>1.366667</td>
      <td>42.664279</td>
      <td>66.628667</td>
      <td>807.000000</td>
      <td>138.666667</td>
      <td>2596.666667</td>
      <td>217.190000</td>
      <td>2.306667</td>
      <td>9.913587</td>
      <td>0.531667</td>
      <td>18.351240</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>222</th>
      <td>TeI2</td>
      <td>{'Te': 1.0, 'I': 2.0}</td>
      <td>1.400000</td>
      <td>39.758135</td>
      <td>127.136313</td>
      <td>725.200000</td>
      <td>138.666667</td>
      <td>5373.333333</td>
      <td>260.200000</td>
      <td>2.473333</td>
      <td>9.970707</td>
      <td>0.210000</td>
      <td>18.954200</td>
    </tr>
    <tr>
      <th>481</th>
      <td>Mg149Br</td>
      <td>{'Mg': 149.0, 'Br': 1.0}</td>
      <td>1.497667</td>
      <td>23.350997</td>
      <td>24.675660</td>
      <td>1356.126667</td>
      <td>140.860000</td>
      <td>1747.213333</td>
      <td>2.164000</td>
      <td>1.321000</td>
      <td>7.674019</td>
      <td>1.019340</td>
      <td>15.078975</td>
    </tr>
    <tr>
      <th>600</th>
      <td>Mg149I</td>
      <td>{'Mg': 149.0, 'I': 1.0}</td>
      <td>1.499333</td>
      <td>23.351870</td>
      <td>24.988996</td>
      <td>1356.962000</td>
      <td>140.986667</td>
      <td>1759.346667</td>
      <td>1.968000</td>
      <td>1.319000</td>
      <td>7.664935</td>
      <td>1.017607</td>
      <td>15.062577</td>
    </tr>
    <tr>
      <th>512</th>
      <td>AuBr2</td>
      <td>{'Au': 1.0, 'Br': 2.0}</td>
      <td>1.216667</td>
      <td>34.000905</td>
      <td>118.924856</td>
      <td>1264.333333</td>
      <td>125.333333</td>
      <td>8513.333333</td>
      <td>290.666667</td>
      <td>2.820000</td>
      <td>10.951043</td>
      <td>0.359000</td>
      <td>21.127333</td>
    </tr>
    <tr>
      <th>544</th>
      <td>PBr</td>
      <td>{'P': 1.0, 'Br': 1.0}</td>
      <td>1.075000</td>
      <td>35.370974</td>
      <td>55.438881</td>
      <td>442.750000</td>
      <td>113.500000</td>
      <td>2471.500000</td>
      <td>198.300000</td>
      <td>2.575000</td>
      <td>11.150245</td>
      <td>0.621500</td>
      <td>20.680250</td>
    </tr>
  </tbody>
</table>
<p>338 rows × 13 columns</p>
</div>




```python
max_properties = pd.DataFrame()
max_properties["formula"] = filtered_data.formula
max_properties["composition_dict"] = filtered_data.composition_dict
for prop in element_data.columns:
    composition_max_list = []
    max_property = 0
    for composition in max_properties["composition_dict"]:
        for element in composition:
            if element_data.loc[element][prop] > max_property:
                max_property = element_data.loc[element][prop]
        composition_max_list.append(max_property)
    max_properties[prop] = composition_max_list
```


```python
max_properties
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
      <th>formula</th>
      <th>composition_dict</th>
      <th>AtomicRadius</th>
      <th>AtomicVolume</th>
      <th>AtomicWeight</th>
      <th>BoilingT</th>
      <th>CovalentRadius</th>
      <th>Density</th>
      <th>ElectronAffinity</th>
      <th>Electronegativity</th>
      <th>FirstIonizationEnergy</th>
      <th>HeatCapacityMass</th>
      <th>SecondIonizationEnergy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>191</th>
      <td>BaBr2</td>
      <td>{'Ba': 1.0, 'Br': 2.0}</td>
      <td>2.15</td>
      <td>64.969282</td>
      <td>137.327</td>
      <td>2143.0</td>
      <td>215.0</td>
      <td>3510.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>11.813800</td>
      <td>0.474</td>
      <td>21.591</td>
    </tr>
    <tr>
      <th>475</th>
      <td>SrBr2</td>
      <td>{'Sr': 1.0, 'Br': 2.0}</td>
      <td>2.15</td>
      <td>64.969282</td>
      <td>137.327</td>
      <td>2143.0</td>
      <td>215.0</td>
      <td>3510.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>11.813800</td>
      <td>0.474</td>
      <td>21.591</td>
    </tr>
    <tr>
      <th>219</th>
      <td>YbBr2</td>
      <td>{'Yb': 1.0, 'Br': 2.0}</td>
      <td>2.15</td>
      <td>64.969282</td>
      <td>173.054</td>
      <td>2143.0</td>
      <td>215.0</td>
      <td>6570.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>11.813800</td>
      <td>0.474</td>
      <td>21.591</td>
    </tr>
    <tr>
      <th>414</th>
      <td>AcBr3</td>
      <td>{'Ac': 1.0, 'Br': 3.0}</td>
      <td>2.15</td>
      <td>64.969282</td>
      <td>227.000</td>
      <td>3473.0</td>
      <td>215.0</td>
      <td>10070.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>11.813800</td>
      <td>0.474</td>
      <td>21.591</td>
    </tr>
    <tr>
      <th>304</th>
      <td>CaBr2</td>
      <td>{'Ca': 1.0, 'Br': 2.0}</td>
      <td>2.15</td>
      <td>64.969282</td>
      <td>227.000</td>
      <td>3473.0</td>
      <td>215.0</td>
      <td>10070.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>11.813800</td>
      <td>0.647</td>
      <td>21.591</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>222</th>
      <td>TeI2</td>
      <td>{'Te': 1.0, 'I': 2.0}</td>
      <td>2.60</td>
      <td>18618.051940</td>
      <td>244.000</td>
      <td>5869.0</td>
      <td>244.0</td>
      <td>22590.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>13.598443</td>
      <td>14.304</td>
      <td>75.640</td>
    </tr>
    <tr>
      <th>481</th>
      <td>Mg149Br</td>
      <td>{'Mg': 149.0, 'Br': 1.0}</td>
      <td>2.60</td>
      <td>18618.051940</td>
      <td>244.000</td>
      <td>5869.0</td>
      <td>244.0</td>
      <td>22590.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>13.598443</td>
      <td>14.304</td>
      <td>75.640</td>
    </tr>
    <tr>
      <th>600</th>
      <td>Mg149I</td>
      <td>{'Mg': 149.0, 'I': 1.0}</td>
      <td>2.60</td>
      <td>18618.051940</td>
      <td>244.000</td>
      <td>5869.0</td>
      <td>244.0</td>
      <td>22590.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>13.598443</td>
      <td>14.304</td>
      <td>75.640</td>
    </tr>
    <tr>
      <th>512</th>
      <td>AuBr2</td>
      <td>{'Au': 1.0, 'Br': 2.0}</td>
      <td>2.60</td>
      <td>18618.051940</td>
      <td>244.000</td>
      <td>5869.0</td>
      <td>244.0</td>
      <td>22590.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>13.598443</td>
      <td>14.304</td>
      <td>75.640</td>
    </tr>
    <tr>
      <th>544</th>
      <td>PBr</td>
      <td>{'P': 1.0, 'Br': 1.0}</td>
      <td>2.60</td>
      <td>18618.051940</td>
      <td>244.000</td>
      <td>5869.0</td>
      <td>244.0</td>
      <td>22590.0</td>
      <td>324.6</td>
      <td>2.96</td>
      <td>13.598443</td>
      <td>14.304</td>
      <td>75.640</td>
    </tr>
  </tbody>
</table>
<p>338 rows × 13 columns</p>
</div>




```python
min_properties = pd.DataFrame()
min_properties["formula"] = filtered_data.formula
min_properties["composition_dict"] = filtered_data.composition_dict
for prop in element_data.columns:
    composition_min_list = []
    min_property = inf
    for composition in min_properties["composition_dict"]:
        for element in composition:
            if element_data.loc[element][prop] < min_property:
                min_property = element_data.loc[element][prop]
        composition_min_list.append(min_property)
    min_properties[prop] = composition_min_list
```


```python
min_properties
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
      <th>formula</th>
      <th>composition_dict</th>
      <th>AtomicRadius</th>
      <th>AtomicVolume</th>
      <th>AtomicWeight</th>
      <th>BoilingT</th>
      <th>CovalentRadius</th>
      <th>Density</th>
      <th>ElectronAffinity</th>
      <th>Electronegativity</th>
      <th>FirstIonizationEnergy</th>
      <th>HeatCapacityMass</th>
      <th>SecondIonizationEnergy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>191</th>
      <td>BaBr2</td>
      <td>{'Ba': 1.0, 'Br': 2.0}</td>
      <td>1.15</td>
      <td>42.527825</td>
      <td>79.90400</td>
      <td>332.00</td>
      <td>120.0</td>
      <td>3120.0000</td>
      <td>13.95</td>
      <td>0.89</td>
      <td>5.211664</td>
      <td>0.204</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>475</th>
      <td>SrBr2</td>
      <td>{'Sr': 1.0, 'Br': 2.0}</td>
      <td>1.15</td>
      <td>42.527825</td>
      <td>79.90400</td>
      <td>332.00</td>
      <td>120.0</td>
      <td>2630.0000</td>
      <td>5.03</td>
      <td>0.89</td>
      <td>5.211664</td>
      <td>0.204</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>219</th>
      <td>YbBr2</td>
      <td>{'Yb': 1.0, 'Br': 2.0}</td>
      <td>1.15</td>
      <td>42.527825</td>
      <td>79.90400</td>
      <td>332.00</td>
      <td>120.0</td>
      <td>2630.0000</td>
      <td>5.03</td>
      <td>0.89</td>
      <td>5.211664</td>
      <td>0.155</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>414</th>
      <td>AcBr3</td>
      <td>{'Ac': 1.0, 'Br': 3.0}</td>
      <td>1.15</td>
      <td>37.433086</td>
      <td>79.90400</td>
      <td>332.00</td>
      <td>120.0</td>
      <td>2630.0000</td>
      <td>5.03</td>
      <td>0.89</td>
      <td>5.170000</td>
      <td>0.120</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>304</th>
      <td>CaBr2</td>
      <td>{'Ca': 1.0, 'Br': 2.0}</td>
      <td>1.15</td>
      <td>37.433086</td>
      <td>40.07800</td>
      <td>332.00</td>
      <td>120.0</td>
      <td>1550.0000</td>
      <td>2.37</td>
      <td>0.89</td>
      <td>5.170000</td>
      <td>0.120</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>222</th>
      <td>TeI2</td>
      <td>{'Te': 1.0, 'I': 2.0}</td>
      <td>0.25</td>
      <td>7.297767</td>
      <td>1.00794</td>
      <td>20.13</td>
      <td>31.0</td>
      <td>0.0899</td>
      <td>0.00</td>
      <td>0.79</td>
      <td>3.893905</td>
      <td>0.116</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>481</th>
      <td>Mg149Br</td>
      <td>{'Mg': 149.0, 'Br': 1.0}</td>
      <td>0.25</td>
      <td>7.297767</td>
      <td>1.00794</td>
      <td>20.13</td>
      <td>31.0</td>
      <td>0.0899</td>
      <td>0.00</td>
      <td>0.79</td>
      <td>3.893905</td>
      <td>0.116</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>600</th>
      <td>Mg149I</td>
      <td>{'Mg': 149.0, 'I': 1.0}</td>
      <td>0.25</td>
      <td>7.297767</td>
      <td>1.00794</td>
      <td>20.13</td>
      <td>31.0</td>
      <td>0.0899</td>
      <td>0.00</td>
      <td>0.79</td>
      <td>3.893905</td>
      <td>0.116</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>512</th>
      <td>AuBr2</td>
      <td>{'Au': 1.0, 'Br': 2.0}</td>
      <td>0.25</td>
      <td>7.297767</td>
      <td>1.00794</td>
      <td>20.13</td>
      <td>31.0</td>
      <td>0.0899</td>
      <td>0.00</td>
      <td>0.79</td>
      <td>3.893905</td>
      <td>0.116</td>
      <td>10.00383</td>
    </tr>
    <tr>
      <th>544</th>
      <td>PBr</td>
      <td>{'P': 1.0, 'Br': 1.0}</td>
      <td>0.25</td>
      <td>7.297767</td>
      <td>1.00794</td>
      <td>20.13</td>
      <td>31.0</td>
      <td>0.0899</td>
      <td>0.00</td>
      <td>0.79</td>
      <td>3.893905</td>
      <td>0.116</td>
      <td>10.00383</td>
    </tr>
  </tbody>
</table>
<p>338 rows × 13 columns</p>
</div>




```python
average_properties = average_properties.drop("composition_dict", axis=1)
average_properties = average_properties.drop("formula", axis=1)
max_properties = max_properties.drop("composition_dict", axis=1)
max_properties = max_properties.drop("formula", axis=1)
min_properties = min_properties.drop("composition_dict", axis=1)
min_properties = min_properties.drop("formula", axis=1)
```

## **Step 8: Combining Property Data**
Next, we'll combine the composition-averaged, maximum, and minimum property DataFrames ('average_properties', 'max_properties', and 'min_properties') into a single design matrix DataFrame named 'design_matrix'.



```python
design_matrix = pd.concat([average_properties, max_properties, min_properties], axis=1)
```

## **Step 9: Splitting Data for Machine Learning**
To train and evaluate our machine learning model, we need to split the data into training and testing sets.


```python
targets = filtered_data[["band_gap", "formation_energy_per_atom"]]
targets.index = filtered_data.formula
```


```python
targets
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
      <th>band_gap</th>
      <th>formation_energy_per_atom</th>
    </tr>
    <tr>
      <th>formula</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BaBr2</th>
      <td>4.3731</td>
      <td>-2.227832</td>
    </tr>
    <tr>
      <th>SrBr2</th>
      <td>4.4697</td>
      <td>-2.156160</td>
    </tr>
    <tr>
      <th>YbBr2</th>
      <td>4.3169</td>
      <td>-2.109686</td>
    </tr>
    <tr>
      <th>AcBr3</th>
      <td>4.1045</td>
      <td>-2.098107</td>
    </tr>
    <tr>
      <th>CaBr2</th>
      <td>4.5304</td>
      <td>-2.052632</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>TeI2</th>
      <td>0.7144</td>
      <td>-0.036698</td>
    </tr>
    <tr>
      <th>Mg149Br</th>
      <td>0.0021</td>
      <td>-0.022094</td>
    </tr>
    <tr>
      <th>Mg149I</th>
      <td>0.0025</td>
      <td>-0.017586</td>
    </tr>
    <tr>
      <th>AuBr2</th>
      <td>0.0000</td>
      <td>-0.007624</td>
    </tr>
    <tr>
      <th>PBr</th>
      <td>1.3954</td>
      <td>-0.007386</td>
    </tr>
  </tbody>
</table>
<p>338 rows × 2 columns</p>
</div>




```python
train_X, test_X, train_y, test_y = train_test_split(design_matrix, targets, test_size=0.10, random_state=42)
```

## **Step 10: Data Normalization**
Normalization is an essential preprocessing step in machine learning. We'll normalize the training data by subtracting the mean and dividing by the standard deviation.


```python
# Set of commands to obtain the mean and standard deviation of train_X
train_mean = train_X.mean()
train_std = train_X.std()

# Set of commands to obtain the new normalized train and test data sets
norm_train_X = (train_X - train_mean) / (train_std)
norm_test_X = (test_X - train_mean) / (train_std)
```

## **Step 11: Cross-Validated Linear Regression**
We'll perform linear regression using scikit-learn and apply cross-validation to evaluate the model's performance.


```python
# Set of commands to run the 5-fold cross-validation on the training set, pick the best-scoring model, and apply it to the test set
kfold = KFold(n_splits=5, shuffle=True, random_state=42)
mlr = linear_model.LinearRegression()
mlr.fit(train_X, train_y["formation_energy_per_atom"])
cross_val = cross_validate(mlr, train_X, train_y["formation_energy_per_atom"], cv=kfold, return_estimator=True)
cv_mlr = cross_val["estimator"][np.argmax(cross_val["test_score"])]
cv_mlr.fit(test_X, test_y["formation_energy_per_atom"])
yhat = cv_mlr.predict(test_X)

# Set of commands to obtain the RMSE and MAE
MAE = mean_absolute_error(test_y["formation_energy_per_atom"], yhat)
RMSE = mean_squared_error(test_y["formation_energy_per_atom"], yhat, squared=False)
print("The mean absolute error for this cross-validated linear model is " + str(MAE))
print("The root mean squared error for this cross-validated linear model is " + str(RMSE))
```

    The mean absolute error for this cross-validated linear model is 0.028795650045063535
    The root mean squared error for this cross-validated linear model is 0.03491365888584181


## **Step 12: Data Visualization - Pairplot**
Finally, we'll use seaborn to create a pairplot to visualize the relationships between different element properties.


```python
features = [c for c in element_data.columns]
grid = sns.pairplot(element_data[features])
```


    
![png](output_60_0.png)
    



```python
train_y
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
      <th>band_gap</th>
      <th>formation_energy_per_atom</th>
    </tr>
    <tr>
      <th>formula</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>PtI2</th>
      <td>1.2280</td>
      <td>-0.165995</td>
    </tr>
    <tr>
      <th>TiBr2</th>
      <td>0.0000</td>
      <td>-1.044361</td>
    </tr>
    <tr>
      <th>WI2</th>
      <td>1.9586</td>
      <td>-0.208133</td>
    </tr>
    <tr>
      <th>Mg3Br</th>
      <td>0.0000</td>
      <td>-0.118789</td>
    </tr>
    <tr>
      <th>NpBr3</th>
      <td>0.0000</td>
      <td>-1.358265</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>CdI2</th>
      <td>2.4223</td>
      <td>-0.592137</td>
    </tr>
    <tr>
      <th>HfBr4</th>
      <td>4.2902</td>
      <td>-1.394327</td>
    </tr>
    <tr>
      <th>SmBr</th>
      <td>0.0000</td>
      <td>-1.149715</td>
    </tr>
    <tr>
      <th>PdI2</th>
      <td>0.9081</td>
      <td>-0.251194</td>
    </tr>
    <tr>
      <th>ThI3</th>
      <td>0.0000</td>
      <td>-1.212139</td>
    </tr>
  </tbody>
</table>
<p>304 rows × 2 columns</p>
</div>



## **Conclusion**
Congratulations! You have completed the lesson on exploring materials project data and predicting material properties. We covered data access, cleaning, manipulation, visualization, and machine learning using Python and various libraries.

Keep exploring and experimenting with different data analysis techniques and machine learning models to gain deeper insights into materials science and make accurate predictions for material properties.
