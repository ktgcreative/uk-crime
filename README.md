# UK Street Crime Data Aggregation

This project aims to aggregate street crime data across the UK, sourced from [data.police.uk/data/](https://data.police.uk/data/). The dataset provides detailed insights into crime patterns across different police forces from July 2020 to July 2023.

![Image of the website showing all the force](/uk_crime_data_website.png)

## Data Structure

The data is organized in the following hierarchy:

- **Main Folder (e.g., uk_street_crime)**
  - **Monthly Folders (e.g., 2020-07, 2020-09, ...)** 
    - **Individual CSVs for each Police Force (e.g., Avon_and_Somerset_Constabulary.csv, Bedfordshire_Police.csv, ...)**

![Image of the website showing all the force](/uk_crime_data_flowchart.png)

 


## Data Aggregation Method

To combine the data from individual CSVs into a single consolidated file, the following Python script is used:

```python
# Your Python code here
import pandas as pd
import os

# Directory where the CSV files are located
root_dir = 'uk_street_crime'

# List to store dataframes
dfs = []

# Loop through each directory and file in the root directory
for subdir, _, files in os.walk(root_dir):
    for file in files:
        # Check if the file is a CSV
        if file.endswith('.csv'):
            # Construct the full file path
            file_path = os.path.join(subdir, file)
            # Read the CSV file into a dataframe and append to the list
            dfs.append(pd.read_csv(file_path))

# Concatenate all dataframes in the list
combined_df = pd.concat(dfs, ignore_index=True)

# Save the combined dataframe to a new CSV file
combined_df.to_csv('uk_street_crimes_data_2020_2023.csv', index=False)

print("All CSV files have been combined!")
```


### 3. Addressing Challenges:

Document any challenges you faced during the process.



## Challenges


1. **Data Granularity:** The data is quite granular, being broken down to the street level . This could pose challenges in terms of data processing and analysis due to its sheer volume.
2. **Data Consistency:** Given that the data is sourced from multiple police forces, there might be inconsistencies in terms of naming conventions, data formats, etc.

Addressing these challenges required careful preprocessing, validation, and consistency checks before data aggregation.

---

### **1. Data Collection and Cleaning:**
- Multiple CSV files containing crime data were combined into a single dataframe.
- The dataset contains over 19 million entries, making it a vast collection of crime data of all different types, from all around the UK.
- Some columns like 'Context' and 'Crime ID', were dropped due to missing values or because they were deemed unnecessary for the analysis.

### **2. Overview of the Dataset:**
- The dataset provides details about various crime types, their locations documented though LSOA's, longitude and latitude, and the police force that reported them.
- There are missing values in some columns, notably in 'Last outcome category' and location-related columns, much of this is due to Anti-Social Behavior having no formal outcome.

### **3. Exploratory Data Analysis (EDA):**
- The distribution of crime types was visualized, showing which crimes are most frequent in the UK and gerneric locations around the UK.
- The data was broken down by the police force that reported the crime, revealing which forces report the most crimes.
- To simplify the analysis, police forces were categorised by geographical regions.
- A focus was placed on generic locations, such as supermarkets, parking areas, and shopping areas, to see which locations are most prone to specific crimes in frequently reported areas, UK wide.

### **Key Insights:**
- **Supermarkets and Parking Areas:** These locations are hotspots for criminal activity, with both accounting for almost 40% of the crimes committed in generic locations.
- **Shopping Areas and Petrol Stations:** These locations are also significant areas of concern. **Shopping areas** are common targets due to the high number of people, while **petrol stations** might be vulnerable due to being open late with minimal staff.
- **Sports/Recreation Areas:** These areas account for a significant ammount of the crimes. Given that these are places of leisure and enjoyment the high crime rate is concerning. The exact nature of these areas and definitions needs further investigation.
- **Anti-social Behaviour:** This crime type is most common in generic areas such as **parking areas**, followed by **supermarkets** and **sports/recreation areas**.
- **Shoplifting:** As expected, **supermarkets** have the highest count of **shoplifting**. 
- **Bicycle Theft:** **Supermarkets** and parking areas are the most common places for **bicycle thefts**.
- **Other Theft:** **Petrol stations** have a surprisingly high count of **other thefts**. This needs to be better defined, but the petrol stations having a high count could leave a sclue to what it means.
- **Violence and Sexual Offences:** **Parking areas** have the highest count for these offenses, followed closely by **sports/recreation areas** and **supermarkets**.
